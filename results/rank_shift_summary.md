# Rank shifts and answer excerpts

Built from the result logs in this folder (`query_log`, `explore_log`, `screen_run_log`, `gen_run_log`, `pool50_log`). Hardware for the latency figures: Apple M4 Pro, 12 cores, 24 GB, CPU only, torch 2.14.1, Python 3.12.

The main set is the 30 queries fixed before the run; the other sets were added afterwards and are described in the README. Ranks are among the 10 retrieved chunks unless a table says otherwise.

## A. Rank-shift table (gold chunk, baseline rank to re-ranked rank among the 10 retrieved)

| id | set | query | gold chunk | baseline | re-ranked | in top 3 (base / re-ranked) |
|---|---|---|---|---|---|---|
| Q05 | main | What is a jump start penalised with? | B5.11.1: During any standing start (Articles B5.7.2 or B5.10.8) or standing sta... | #3 | #9 | yes / no |
| Q07 | main | How are the results decided if the race is stopped and cannot be restarted? | B5.14.2#2: In exceptional circumstances, for reasons of safety the pit entry may... | #8 | #7 | no / no |
| Q14 | main | Can drivers overtake while the Safety Car is deployed? | B5.13.2#2: At the sole discretion of the Race Director, the maximum allowable gap... | outside top 10 | outside top 10 | - / - |
| Q14 | main | Can drivers overtake while the Safety Car is deployed? | B5.13.2#3: vi. Subject to the provisions of Article B5.13.3, whilst in the Pit En... | #2 | #1 | yes / yes |
| Q14 | main | Can drivers overtake while the Safety Car is deployed? | B5.13.4#1: Order of Cars Behind the SC a. When instructed by the Race Director th... | #3 | #6 | yes / no |
| Q15 | main | When can a team change parts on a car that is in parc fermé? | B3.5.3: With the exceptions of work listed in Appendix B2 or approved by the T... | #1 | #1 | yes / yes |
| Q15 | main | When can a team change parts on a car that is in parc fermé? | B3.5.4: Any parts which are removed from the F1 Car in order to carry out any... | #10 | #7 | no / no |
| Q15 | main | When can a team change parts on a car that is in parc fermé? | B3.5.5: Exceptionally, six (6) times during the Championship a request made un... | #8 | #9 | no / no |
| Q22 | main | Can a driver try to start a car that has been pushed off the grid? | B5.7.1: At any time after the formation lap has commenced and prior to the sta... | #3 | #1 | yes / yes |
| Q25 | main | Can the stewards' decision on a drive-through penalty be appealed? | B1.9.7: Appeal of a Penalty Pursuant to Articles A7.5.1 and A7.5.6, appeals ma... | #2 | #2 | yes / yes |
| Q25 | main | Can the stewards' decision on a drive-through penalty be appealed? | B1.9.5#1: Penalties for Incident(s) during a TTCS In the event of an incident du... | #4 | #4 | no / no |
| Q26 | main | What penalties can the stewards impose for an incident during the Race? | B1.9.5#1: Penalties for Incident(s) during a TTCS In the event of an incident du... | #2 | #1 | yes / yes |
| Q26 | main | What penalties can the stewards impose for an incident during the Race? | B1.9.5#2: g. A Competitor (F1 Team) reprimand. h. A drop of any number of grid p... | #9 | #9 | no / no |
| Q04 | main | How many mechanics can be on the grid before the start? | B5.5.4: When the three (3) minute signal is shown: a. No more than sixteen (16... | outside top 10 | outside top 10 | - / - |
| Q12 | main | Can a driver receive physical help after stopping on the circuit during the Race | B5.1.6: Except for the circumstances described in Article B1.5.4d or Article B... | outside top 10 | outside top 10 | - / - |
| Q12 | main | Can a driver receive physical help after stopping on the circuit during the Race | B1.5.4: During the period commencing fifteen (15) minutes prior to and ending... | outside top 10 | outside top 10 | - / - |
| Q23 | main | Can a team cover the car in the pit lane when it rains? | B3.3.3: The following are permitted: a. Covers which are placed over damaged F... | outside top 10 | outside top 10 | - / - |
| X10 | followup | What can the stewards do about an incident during Qualifying? | B1.9.4: Penalties for Incident(s) during a LTCS In the event of an incident du... | #6 | #1 | no / yes |
| Y03 | screened | Can a car that broke down on its way to the grid before the start still start fr | B5.2.4: Any F1 Car which does not complete a reconnaissance lap and reach the... | #10 | #5 | no / no |

Model-written questions kept by the screen (gold at baseline rank 4 to 10), 19 of 150:

| id | set | query | gold chunk | baseline | re-ranked | in top 3 (base / re-ranked) |
|---|---|---|---|---|---|---|
| Z021 | model-written | What's the rule for when cars are allowed to pass each other behind the Safety C | B5.13.2#2: At the sole discretion of the Race Director, the maximum allowable gap... | #4 | #9 | no / no |
| Z027 | model-written | Where do the cars go and in what order if the start is halted after the formatio | B5.14.1: Suspension of a Starting Procedure If at any time after the formation... | #6 | #1 | no / yes |
| Z042 | model-written | How many people working on the cars can a team have at the circuit during a race | B9.2.1: At each Competition during the period beginning twenty-nine (29) hours... | #7 | #1 | no / yes |
| Z044 | model-written | How long after the race director says it's safe to end the safety car period do  | B4.1.4#2: iii. Whilst in the Pit Entry Road, Pit Lane or Pit Exit Road a driver... | #4 | #3 | no / yes |
| Z045 | model-written | What happens to the starting order if qualifying gets cancelled and the stewards | B2.5.4#1: Grid for the Race Session a. The grid for the Race will be formed in a... | #9 | #4 | no / no |
| Z050 | model-written | Who besides drivers is allowed on the track or in the pit lane around practice,  | B1.5.4: During the period commencing fifteen (15) minutes prior to and ending... | #10 | #2 | no / yes |
| Z059 | model-written | How are the grid spots for the race decided for drivers who got knocked out in Q | B2.4.3#1: Race Qualifying Classification The Qualifying Classification will be d... | #4 | #2 | no / yes |
| Z064 | model-written | If my engine stalls while I'm serving a stop-and-go in the pit lane and I can't  | B1.9.6#4: iv. Whilst an F1 Car is stationary in the Pit Lane as a result of incu... | #5 | #1 | no / yes |
| Z074 | model-written | Once my car has left the pit lane for the first time in qualifying, when does it | B3.5.1: Each F1 Car will be deemed to be in parc fermé from the time: a. At wh... | #5 | #1 | no / yes |
| Z101 | model-written | Which older parts can a team use on its test cars before the first race of the s | B11.4.10#3: ii. Be a suitably modified car designed and constructed in order to co... | #6 | #5 | no / no |
| Z102 | model-written | How long do I have to wait before my mechanics can touch the car if it's stopped | B1.9.6#3: ii. Unless the driver was already in the Pit Entry Road or Pit Lane fo... | #6 | #2 | no / yes |
| Z105 | model-written | When are we allowed to run cars for tyre checks, and what are the time and locat | B11.2.1: TCC running may only take place: a. Between the times of 09:00 and 19:... | #9 | #4 | no / no |
| Z115 | model-written | When a rolling start is called behind the safety car, when are drivers allowed t | B5.10.9#1: Rolling Start a. If after the formation laps behind the Safety Car hav... | #8 | #3 | no / yes |
| Z124 | model-written | How far apart can cars be when they're queued behind the safety car, and does th | B5.15.2#4: f. At the resumption time the Pit Lane exit will be opened, all F1 Car... | #4 | #3 | no / yes |
| Z125 | model-written | If a driver has a drive-through or stop-and-go penalty, can they still serve it  | B1.9.6#2: ii. The relevant driver may elect not to serve the penalty during the... | #4 | #3 | no / yes |
| Z128 | model-written | How long does a team have to receive a decision about one of its drivers, and do | B1.1.2: Instructions And Communications To Competitors a. The stewards or Race... | #4 | #3 | no / yes |
| Z130 | model-written | How many cars can our team use for testing per day, and what hours does a testin | B11.3.4: Each Competitor may not have more than one (1) car available for the p... | #10 | #1 | no / yes |
| Z131 | model-written | Once the leader is behind the Safety Car, when does the Safety Car actually head | B5.13.5: Duration of SC Period a. Except under Article B5.13.4c, the Safety Car... | #5 | #1 | no / yes |
| Z135 | model-written | If a car was in its garage when the restart was paused, where does it line up wh | B5.15.1#2: iii. In all cases the order will be taken at the last point at which i... | #7 | #3 | no / yes |

Reading of the answers against the clause text (author's assessment). Answer-level restorations, where Answer 1 says the passages do not cover the question or is wrong and Answer 2 is correct: Z027, Z042, Z050, Z059, Z064, Z074, Z115, Z128, Z130, Z131. Rank only or weak gold label: Z044, Z102, Z124, Z125, Z135. Not restored: Z021 (rank 4 to 9), Z045, Z101, Z105.

## B. Pool of 50 (outside the assignment design), gold rank among the 50 nearest chunks

| id | gold chunk | cosine rank | cross-encoder rank (50 pairs) |
|---|---|---|---|
| Q23 | B3.3.3: The following are permitted: a. Covers which are placed over damaged F... | #40 | #1 |
| Q12 | B5.1.6: Except for the circumstances described in Article B1.5.4d or Article B... | #19 | #2 |
| Q12 | B1.5.4: During the period commencing fifteen (15) minutes prior to and ending... | #45 | #14 |
| Q04 | B5.5.4: When the three (3) minute signal is shown: a. No more than sixteen (16... | #11 | #21 |
| X09 | B4.1.4#1: The VSC procedure may be initiated to neutralise a LTCS on the order o... | #15 | #35 |

## C. Answer excerpts (verbatim from the saved logs, cut at the length shown)

### X10 (followup): What can the stewards do about an incident during Qualifying?
Gold B1.9.4; rank [6, 1]; baseline top 3 ['B1.9.3', 'B1.9.1', 'B1.9.2']; re-ranked top 3 ['B1.9.4', 'B1.9.2', 'B1.9.3']

**Answer 1:**
> The passages don't address qualifying specifically, but they set out the stewards' general powers over any incident, which would apply there:
> 
> - **Decide whether to investigate:** The stewards decide whether to proceed with an investigation. They may also investigate an incident they notice themselves [3]. The Race Director may report incidents or suspected breaches to them [2].
> - **Notify competitors:** If an incident is under investigation, a message naming the drivers involved is sent to all competitors [3].
> - **Restrict drivers' movements:** Provided that message is displayed no later than 60 minutes after the end-of-session signal of a T...

**Answer 2:**
> During a Lap Time Competition Session (LTCS), the stewards may:
> 
> - **Delete a driver's lap time or lap times**, or
> - **Drop the driver a number of grid positions** that they consider appropriate [1].
> 
> Incidents are normally investigated after the relevant session, unless it is completely clear that a driver committed a driving infringement [1]. Stewards also decide whether to investigate an incident at all, and they may investigate incidents they noticed themselves [2].
> 
> Grid position penalties are served in the Race. The exception is an infringement during Sprint Qualifying, where the penalty is applied to the Sprint grid [1].
> 
> When deciding...

### Z042 (model-written): How many people working on the cars can a team have at the circuit during a race weekend, and does that limit still apply on race day?
Gold B9.2.1; rank [7, 1]; baseline top 3 ['B7.2.1#4', 'B1.7.1', 'B9.5.3']; re-ranked top 3 ['B9.2.1', 'B1.5.8', 'B11.4.10#1']

**Answer 1:**
> The provided passages do not answer this. None of them states how many people working on the cars a team may have at the circuit during a race weekend, or whether that limit applies on race day.
> 
> The closest passage is Article B9.5.3 [3]. It gives each Competitor a number of exceptions to the Article B9.5 requirements during the Championship: six for Restricted Period One, five for Restricted Period Two, and four for Restricted Period Three. It also says these exceptions may not be used consecutively during a single Competition. However, it does not define the Article B9.5 requirements themselves, so it cannot tell you the headcount limit. Fo...

**Answer 2:**
> A team may have at most **60 Operational Personnel** at the circuit, meaning people associated in any way with operating the F1 Cars within the circuit. This applies from 29 hours before the scheduled start of FP1 until 2 hours after the start of the Race [1].
> 
> **On race day, the limit still applies only in part.** It is lifted from 45 minutes before the start of the first formation lap until 15 minutes after the scheduled start of the Race, when the number of Operational Personnel is unlimited [1]. Before that window opens, the 60-person cap still applies.
> 
> A separate restriction covers the signalling area. Only twelve team members per Compe...

### Z064 (model-written): If my engine stalls while I'm serving a stop-and-go in the pit lane and I can't restart it myself, can the mechanics help me get it going right away?
Gold B1.9.6#4; rank [5, 1]; baseline top 3 ['B5.7.3', 'B5.13.3', 'APPENDIX B2#4']; re-ranked top 3 ['B1.9.6#4', 'B5.15.4#3', 'B5.7.3']

**Answer 1:**
> The provided passages do not cover this. None of them address stop-and-go penalties, stalled engines, or whether mechanics may help restart a car in the pit lane.
> 
> The closest material is limited:
> 
> - Article B5.7.3 covers the pit lane start procedure, where cars must keep their engines running while queuing at the pit lane exit [1]. It does not address restarting a stalled engine.
> - Article B5.13.3 covers pit lane use during a Safety Car deployment and a drive-through penalty for failing to enter the pit lane when required [2]. It does not mention stop-and-go penalties or stalls.
> - Appendix B2 lists permitted parc fermé works, such as safety ...

**Answer 2:**
> No. If the engine stops and you cannot restart it unaided, any work needed to restart it may only be carried out after the time penalty period has elapsed [1]. While the car is stationary in the Pit Lane for a Stop-and-Go Penalty, it may not be worked on, and touching the car or driver by hand, with tools, or with other equipment all count as working [1].

### Z074 (model-written): Once my car has left the pit lane for the first time in qualifying, when does it count as being locked away until the race starts, and what happens if it never leaves the pit lane?
Gold B3.5.1; rank [5, 1]; baseline top 3 ['B1.1.3', 'APPENDIX B1#3', 'B5.13.3']; re-ranked top 3 ['B3.5.1', 'B5.7.3', 'B5.15.2#4']

**Answer 1:**
> The provided passages do not cover this. They don't address parc fermé, when a car is locked away after leaving the pit lane in qualifying, or what happens if a car never leaves the pit lane.
> 
> The passages do cover some related topics:
> 
> - **Circuit use:** Unless the FIA gives written permission, the circuit may be used for non-competition purposes only after the last free practice session each day, after Sprint Qualifying, after Qualifying, and on race day at least one hour before the Pit Lane exit opens for reconnaissance laps [1].
> - **Pit lane layout:** The Pit Entry Road runs from the first safety car line to the start of the Pit Lane, and...

**Answer 2:**
> Once your car first leaves the pit lane during qualifying, it is deemed to be in parc fermé from that moment until the start of the Race [1].
> 
> If it never leaves the pit lane during qualifying, it is deemed to be in parc fermé at the end of Q1 [1].

### Q05 (main): What is a jump start penalised with?
Gold B5.11.1; rank [3, 9]; baseline top 3 ['B5.10.8#2', 'B5.10.6#2', 'B5.11.1']; re-ranked top 3 ['B5.8.2', 'B1.9.6#4', 'B5.10.8#2']

**Answer 1:**
> The passages don't use the term "jump start," but the false start rules in Article B5.11.1 appear to cover it. Under that article, cars must be stationary in their grid positions until the start signal, and the stewards can impose a **5-second penalty, a 10-second penalty, a drive-through penalty, or a stop-and-go penalty** on any driver judged to have breached this, at their sole discretion [3].

**Answer 2:**
> The passages do not cover jump starts, so I can't say how one is penalised.
> 
> They do cover related situations. A Stop-and-Go Penalty is imposed on any driver who fails to start the TTCS from the Pit Lane [1][3]. Drivers who cause an aborted start and can then restart must start from the end of the Pit Lane [1]. Also, a Stop-and-Go Penalty imposed after a TTCS adds 30 seconds to the driver's elapsed time [2].

### Q07 (main): How are the results decided if the race is stopped and cannot be restarted?
Gold B5.14.2#2; rank [8, 7]; baseline top 3 ['B2.4.1', 'B5.8.1', 'B5.15.4#3']; re-ranked top 3 ['B6.3.6', 'B5.15.4#3', 'B4.3.1']

**Answer 1:**
> The provided passages do not cover how results are decided if a race is stopped and cannot be restarted. They describe the qualifying session timing [1], the procedure for an aborted start before the race begins [2], and the standing start resumption procedure after a Safety Car period [3], but none of them address a race that is halted permanently.

**Answer 2:**
> The passages don't fully answer this. They only cover one consequence of a race that is suspended and cannot be restarted: the tyre-compliance penalty.
> 
> Under the tyre rules, if the race is suspended and cannot be re-started, 30 seconds is added to the elapsed time of any driver who did not, when required, use at least two specifications of dry-weather tyre during the race [1]. Disqualification applies only where the race is not suspended, or where it is suspended but can be restarted [1].
> 
> The passages do not explain how the final classification is otherwise determined in that situation. [2] covers resumption procedures for a Total Time Clas...

### Q14 (main): Can drivers overtake while the Safety Car is deployed?
Gold B5.13.2#2; rank None; baseline top 3 ['B7.2.4', 'B5.13.2#3', 'B5.13.4#1']; re-ranked top 3 ['B5.13.2#3', 'B7.2.2', 'B7.2.3']

**Answer 1:**
> Generally, the passages suggest drivers cannot overtake while the Safety Car is deployed, but only the exceptions are spelled out. The passages do not state the general prohibition itself.
> 
> The exceptions include:
> 
> - **Pit areas:** While in the Pit Entry Road, Pit Lane or Pit Exit Road, a driver may overtake another F1 Car that is also in one of those areas, subject to Article B5.13.3 [2].
> - **Stopped cars:** An F1 Car stopping in its Designated Garage Area while the Safety Car is using the Pit Lane may be overtaken [2].
> - **Slow cars with an obvious problem:** Any F1 Car that slows with an obvious problem may be overtaken [2].
> - **Cars requi...

**Answer 2:**
> No, not in general. When the Safety Car is deployed, Overtake is disabled [2]. It is re-enabled only after all F1 Cars have crossed the Control Line following the Safety Car's entry into the Pit Entry Road to return to the Pit Lane [2].
> 
> There are limited exceptions in the Safety Car provisions. While the Safety Car is deployed, a driver may overtake another F1 Car that is also in the Pit Entry Road, Pit Lane or Pit Exit Road, subject to Article B5.13.3 [1]. A car stopping in its Designated Garage Area while the Safety Car is using the Pit Lane may be overtaken [1]. A car that slows with an obvious problem may also be overtaken [1].
> 
> The exce...

### Q25 (main): Can the stewards' decision on a drive-through penalty be appealed?
Gold B1.9.7; rank [2, 2]; baseline top 3 ['B1.9.3', 'B1.9.7', 'B1.9.6#1']; re-ranked top 3 ['B1.9.3', 'B1.9.7', 'B1.9.6#1']

**Answer 1:**
> The passages don't give a definitive answer. Drive-Through Penalties are named in [3], which sets out how they are served, but none of the passages says which article governs them.
> 
> [2] states that appeals may not be made against penalties imposed under Articles B1.9.5 a. to h. Whether a drive-through penalty falls within that range depends on Article B1.9.5, which is not included here. If it does, the stewards' decision could not be appealed. The passages also don't cover the appeal rules in Articles A7.5.1 and A7.5.6, which [2] cross-references.

**Answer 2:**
> The passages don't give a definitive answer. Drive-Through Penalties are named in [3], which sets out how they are served, but none of the passages says which article governs them.
> 
> [2] states that appeals may not be made against penalties imposed under Articles B1.9.5 a. to h. Whether a drive-through penalty falls within that range depends on Article B1.9.5, which is not included here. If it does, the stewards' decision could not be appealed. The passages also don't cover the appeal rules in Articles A7.5.1 and A7.5.6, which [2] cross-references.

