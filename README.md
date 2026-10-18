# Baseline retrieval vs cross-encoder re-ranking

A retrieval-augmented question answering experiment over one regulatory PDF, comparing two pipelines on
the same chunks, the same prompt and the same language model.

* Pipeline 1: embed the question with a bi-encoder, take the 10 nearest chunks by cosine similarity,
  send the top 3 to the language model (Answer 1).
* Pipeline 2: score the same 10 chunks with a cross-encoder, sort by that score, send the new top 3 to the
  same model with the same prompt (Answer 2).

Everything is in one notebook, `rag_rerank_eval.ipynb`, which is saved with its outputs.

## Document

FIA 2026 Formula 1 Regulations, Section B (Sporting), Issue 09, dated 1 October 2026.
Source: https://www.fia.com/system/files/documents/fia_2026_f1_regulations_-_section_b_sporting_-_iss_09_-_2026-10-01.pdf

SHA-256 of the file: `5e156f50f1ca529d43551f52e82f5f2872bdfbe088e96fab39b861f6b0704b85`. The notebook
downloads the PDF into `data/` on first run and stops if the checksum differs. The PDF is not stored in
this repository.

## What was changed in the source before indexing

Every change is listed here, because each one changes what the retrieval can find.

* Pages 1 to 3 (cover, colour legend, contents) are not indexed.
* Pages 95 to 101 (approved changes for 2027 to 2029, not in force) are not indexed. Pages 4 to 94 are.
* Struck-out text is removed. The PDF marks deleted rules with a thin magenta bar through magenta text.
  The bars are found in the page drawing data, and any character whose centre falls on a bar is dropped.
  Plain text extraction would index the deleted wording as if it were in force.
* The running page header and footer are cut by position (the top 50 and bottom 65 points of a page).
* The row-number column in the appendix tables is dropped.
* Text in red, orange and green (annotations such as the "Advisory Committee" lines) is kept.
* The table in clause B6.2.4 (tyre allocation) loses its column layout in text extraction, so its column
  headers are scrambled in the chunk. Q09 is scored on retrieval only for that reason.

## Chunking

One chunk per numbered clause, with the article, section and clause number written in front of the text
(for example `ARTICLE B1: ... > B1.6 ... > B1.6.3`). Clauses longer than 400 tokens are split at
paragraph ends with one paragraph of overlap, and later pieces keep the clause title. The result is 375
chunks of 32 to 395 tokens (median 138). The notebook checks the chunk text against a pinned hash
(`ac164cf33589cd0daf4f91928023a02f74af9bb3fb4881fc5107ad8c45ea407e`). Saved answers depend on the exact chunk text, so a
change in extraction or chunking fails that check on purpose.

## Models

| Role | Model | Notes |
|---|---|---|
| Bi-encoder | `BAAI/bge-small-en-v1.5` | revision `5c38ec7c405ec4b44b94cc5a9bb96e735b38267a`; queries get the phrase `Represent this sentence for searching relevant passages: `, passages get none |
| Index | FAISS `IndexFlatIP` | exact search on unit vectors, checked against a plain matrix product |
| Cross-encoder | `cross-encoder/ms-marco-MiniLM-L-6-v2` | revision `233902d25c440f23af6f7d6e94d2946bac0bee0a`; reads the plain question and the chunk text |
| Generator | `claude-haiku-5-5` | through the OpenCode Go service; answers generated on 18 and 19 October 2026 |

The generator rejects a temperature setting, so its output is not deterministic. Every answer is saved in
`results/answers_cache.json` (keyed by model, prompt text and output budget) and the notebook reads the
saved answer when the API key is not set. The output budget is 800 tokens. The model's thinking tokens
count toward it, so some answers were cut off (see Limitations).

Versions are pinned in `requirements.txt` (numpy 2.5.4, pymupdf 1.28.2, sentence-transformers 6.1.0,
faiss-cpu 1.15.1). The notebook was run with Python 3.12 and torch 2.14.1 on an Apple Silicon CPU.

## How to run

Local: create a Python 3.12 environment, `pip install -r requirements.txt jupyter`, set nothing for a run
that reads the saved answers, and run all cells. To generate new answers, export `OPENCODE_API_KEY` first.
The key is read from the environment only; `.env*` files are gitignored.

Colab: open the notebook and run all cells. The first cell installs the packages and clones this
repository if the working directory does not contain `queries.json`. Without an API key the saved answers
are used.

Every result file is written once. If a file already exists the notebook reads it instead of running the
queries again, so re-running the notebook does not overwrite a record. To redo a step, move the file away.

## Files

| File | Content |
|---|---|
| `rag_rerank_eval.ipynb` | the notebook, with outputs |
| `queries.json` | the 30 main queries with gold chunks and failure types, fixed before they were run |
| `queries_explore.json` | 14 follow-up queries on near-duplicate clauses |
| `queries_screen.json` | 18 hand-written candidates for the screened batch |
| `results/query_log.jsonl` | the record of the main run: ranks, top 3, answers, cold single timings |
| `results/score_log.jsonl` | cosine and cross-encoder scores for the main run |
| `results/answers_cache.json` | every saved answer (800 token budget) |
| `results/answers_cache_cap2000.json`, `results/truncation_supplement.jsonl`, `results/gen_truncation_supplement.jsonl` | answers generated again at 2000 tokens where an answer reached 800 |
| `results/explore_log.jsonl`, `results/explore_score_log.jsonl` | the 14 follow-up queries |
| `results/pool50_log.jsonl` | scores for the pool of 50 analysis |
| `results/screen_log.jsonl`, `results/screen_run_log.jsonl` | the screened batch |
| `results/rank_shift_summary.md` | rank-shift tables for the main cases and the model-written questions, the pool of 50 table, and answer excerpts, built from the logs |
| `results/gen_all_score_log.jsonl` | scores and ranks for all 150 model-written questions, from the retrieval-only analysis of section 18 |
| `results/gen_queries.json`, `results/questions_cache.json`, `results/gen_screen_log.jsonl`, `results/gen_run_log.jsonl` | the model-written question set |

## Results

All numbers below are produced by the notebook; the saved outputs in it are the reference.

### Main set (30 queries, 29 scored)

Gold is the smallest set of chunks needed for a complete answer. hit@3: any gold chunk in the top 3.
recall@3: gold chunks in the top 3 divided by the smaller of 3 and the number of gold chunks. MRR: one over
the rank of the best-placed gold chunk, 0 if none is in the top 10. Baseline / re-ranked:

| Group | n | hit@3 | recall@3 | MRR |
|---|---|---|---|---|
| control | 3 | 1.00 / 1.00 | 1.00 / 1.00 | 1.00 / 1.00 |
| keyword trap | 6 | 0.67 / 0.50 | 0.67 / 0.50 | 0.47 / 0.46 |
| exception | 9 | 0.89 / 0.89 | 0.70 / 0.67 | 0.70 / 0.83 |
| negation | 7 | 0.86 / 0.86 | 0.79 / 0.79 | 0.62 / 0.62 |
| scattered | 4 | 1.00 / 1.00 | 0.75 / 0.75 | 0.75 / 1.00 |
| all | 29 | 0.86 / 0.83 | 0.75 / 0.71 | 0.67 / 0.74 |

Re-ranking moved the best gold chunk up for 8 queries and down for 3. It brought a gold chunk into the top
3 for none and took one out for two (Q05, Q14). Gold was outside the top 10 for three queries (Q04, Q12,
Q23), where re-ranking cannot help. The main set therefore contains no query where re-ranking restores
relevance; that is a result, not an omission.

### Latency (warm models, Apple Silicon CPU, torch 2.14.1)

Median per stage over the 30 queries, 5 runs each:

| Stage | Time |
|---|---|
| baseline retrieval (embed the question and search) | 6.8 ms |
| re-ranking 10 pairs | 68.9 ms |
| pipeline 2 total (retrieval and re-ranking) | 75.7 ms |
| answer generation (median of 60 calls, time of the original call) | 3.25 s |

Re-ranking costs about 7 ms per pair; 50 pairs take about 347 ms (section 15 of the notebook). The
per-query timings stored in the log are single cold measurements and are not used for this table.

### Additions made after the main run

These were added after seeing the main results, are labelled in the notebook, and are never merged into the
table above.

* **Follow-up batch (14 queries on near-duplicate clauses).** Written from the clause text and committed
  with their gold labels and scoring rules before they were run. 1 of 14 restored (gold from rank 6 to 1),
  none degraded; the gold chunk ranked above its near-duplicate sibling in 12 of 14 queries for the baseline
  and 13 of 14 after re-ranking.
* **Pool of 50 (outside the assignment design, retrieval only).** Re-ranking the 50 nearest chunks instead
  of 10 lifted the gold chunk of Q23 from rank 40 to 1 and of Q12 from rank 19 to 2, and pushed Q04
  (11 to 21) and one follow-up query down.
* **Screened batch (18 hand-written candidates).** Cosine screening kept 1 whose gold was at baseline rank 4
  to 10; it moved from 10 to 5, outside the top 3. 0 of 1 restored.
* **Model-written questions (150 generated, 19 kept, 15 restored).** `claude-haiku-5-5` wrote one question
  for each of 150 randomly sampled chunks (seed 20261019); the source chunk is the gold. The instruction,
  fixed beforehand, asked for a question in everyday words using plain words instead of the regulation's
  own defined terms where a plain word exists, which deliberately targets the bi-encoder's weakness with
  vocabulary mismatch. After a
  cosine-only screen kept the 19 questions whose gold was at baseline rank 4 to 10, the two pipelines were
  run once. The gold chunk entered the top 3 for 15 of 19. Reading the answers against the clause text, in
  11 of those 15 Answer 1 says the passages do not cover the question or is wrong while Answer 2 is correct;
  in the other 4 the rank changes but the answer does not, or the gold label is weak. The questions are
  model written and are not the author's wording. A random reordering of 10 chunks would put the gold chunk
  in the top 3 with probability 0.3, so 15 or more of 19 has a binomial tail probability of about 1.5e-5.
  That null only rules out a random reordering. Re-ranking all 150 questions (retrieval only, section 18 of
  the notebook) gives hit@3 0.79 for the baseline and 0.85 re-ranked: 15 questions gained and 7 lost
  (2 of the 94 with gold at rank 1 and 5 of the 25 with gold at ranks 2 to 3). Among the 131 questions
  that were not kept, hit@3 fell from 119 to 112.

## Limitations

* Small query sets over one document. The main set has 29 scored queries; differences of one or two
  queries are not evidence of a gain or a loss.
* The domain differs from the re-ranker's training data (web search passages), and a clause-level chunk can
  lose the qualifier that sits in a neighbouring clause or in a definition. Several hedged answers trace to
  a definition chunk that never reaches the top 3.
* A 10-chunk re-ranker can only help a query whose best gold chunk is at baseline ranks 4 to 10. Of the 61
  queries in the first three sets, three were (Q07, one follow-up query and the screened one).
* Output budget: five saved answers in the main run reached the 800 token budget, in six answer slots
  because both answers of Q11 share one prompt (Q09 Answer 2, Q11 both, Q15 Answer 2, Q26 both). Both answers of the affected queries were generated again at 2000 tokens as a
  labelled supplement and the original answers stay primary. Five queries of the model-written set were
  treated the same way.
* The generator is not deterministic. Where both pipelines pass the same three chunks in a different order
  (Q16, Q18, X01) an answer difference is partly noise.
* Gold labels are single source clauses written by the author or taken from the sampled chunk, and a few
  queries have a second chunk that also answers the question.
* Scores closer than about 1e-4 can order differently on other hardware. The notebook reports such
  differences against the saved logs instead of stopping.
* The OpenCode Go service is described as aimed at coding agents; a short identifying user agent and a
  session header are sent.
