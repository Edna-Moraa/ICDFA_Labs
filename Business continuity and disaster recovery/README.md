# Business Continuity and Disaster Recovery

**GRC205 · Incident Management and Business Continuity · Week 21**  
International Cybersecurity and Digital Forensics Academy

Fictional training scenario. Northbridge Skills Institute is not a real organisation.

| | |
| --- | --- |
| Student | Edna Moraa Maburi |
| Student ID | 2025/GCR/12688 |
| Official email | [gcr2512688@students.icdfa.ed.ng](mailto:gcr2512688@students.icdfa.ed.ng) |
| Programme | Governance, Compliance, and Risk Engineering |
| Word count | 3,137 words, excluding the cover, tables, and references |

## Contents

1. [Business impact analysis](#task-1--business-impact-analysis)
2. [Recovery objectives and backup design](#task-2--recovery-objectives-and-backup-design)
3. [Recovery strategy and budget](#task-3--recovery-strategy-and-budget)
4. [Business continuity and disaster recovery plan](#task-4--business-continuity-and-disaster-recovery-plan)
5. [Recovery schedule and exercise](#task-5--recovery-schedule-and-exercise)
6. [Governance and performance measures](#task-6--governance-and-performance-measures)
7. [References](#references)

---

## Task 1 — Business impact analysis

### BIA five functions

The analysis covers all five functions. Table 1 gives the owner, the deadline, the hourly cost, the minimum service, and the business priority. Table 1.1 gives the people, premises, suppliers and information each function needs, because a plan that only names servers fails when the building is shut and only two technicians are. F1 to F4 depend on the recovery network, AUTH01, DB01, and their own application. F5 depends on the network, AUTH01, and FS01. Minimum service is deliberately below a full return: a supervised split sitting, a dual-approved fee list, a timestamped application capture, a payroll file that is not sent until it is checked and a certificate queue. Integrity controls sit inside those minimums, especially for wallets, bank payments, and certificates.

**Table 1. Business impact summary and minimum service**

| Function | Owner | Deadline from 08:00 | Hourly cost (NGN) | Minimum service | Business priority |
| --- | --- | --- | --- | --- | --- |
| F1 Teaching and assessment | Academic Director | 11:00 Day 0 (3 h), plus NGN 120,000 if missed | 60,000 | Supervised split sitting; integrity kept; not all 40 centre learners at once | 1 |
| F2 Fees and finance | Finance Manager | 16:00 Day 0 (8 h) | 40,000 | Provider report, dual approval, no unverified wallet update | 2 |
| F3 Admissions | Admissions Manager | 18:00 Day 0 (10 h) | 20,000 | Timestamped capture in Microsoft 365, entered into REG01 later | 3 |
| F4 Payroll | HR Manager | 12:00 Day 1 (28 h) | 10,000 | Verified file only; no duplicate bank instruction | 4 |
| F5 Records and certificates | Registry Manager | 48 h internal commitment | 5,000 | Queue only until FS01 is accepted; no certificates from memory | 5 |

**Table 1.1. People, premises, suppliers, and information.**

| Function | People | Premises and suppliers | Information needed |
| --- | --- | --- | --- |
| F1 | 120 exam learners, invigilators, Academic Director | Alternate room, 25 workstations, mobile internet | Candidate list, access arrangements, clean paper |
| F2 | Finance staff and learners awaiting confirmation | Payment provider, which stays available | Provider settlement report after the last checkpoint |
| F3 | Applicants and admissions staff | Microsoft 365 mailbox | Email time, sender and identity check |
| F4 | 90 staff, HR and a second checker | Bank portal | Last verified run plus approved changes |
| F5 | Registry staff and requesters | No main-site file room | Request log; no certificate from memory |

### Disruption cost calculations

The brief says the hourly amounts are separate and do not overlap. I apply each rate for the hours of disruption and I add the examination rescheduling cost once, because both the 8-hour and the 24-hour calculations assume no recovery and no continuity measure. The examination trigger is therefore met: nothing acceptable is available at 11:00. I do not add the NGN 120,000 a second time at 24 hours.

The combined hourly rate is 60,000 + 40,000 + 20,000 + 10,000 + 5,000 = NGN 135,000. For 8 hours the time-based cost is 135,000 × 8 = NGN 1,080,000. Adding the examination trigger once gives NGN 1,200,000. For 24 hours the time-based cost is 135,000 × 24 = NGN 3,240,000. Adding the same trigger once gives NGN 3,360,000. The function lines are shown so the total can be checked without relying on the combined rate alone.

**Table 2: Worked 8-hour and 24-hour impact. Amounts are exercise estimates.**

| Function | Rate × 8 hours | 8-hour amount (NGN) | Rate × 24 hours | 24-hour amount (NGN) |
| --- | --- | --- | --- | --- |
| F1 | 60,000 × 8 | 480,000 | 60,000 × 24 | 1,440,000 |
| F2 | 40,000 × 8 | 320,000 | 40,000 × 24 | 960,000 |
| F3 | 20,000 × 8 | 160,000 | 20,000 × 24 | 480,000 |
| F4 | 10,000 × 8 | 80,000 | 10,000 × 24 | 240,000 |
| F5 | 5,000 × 8 | 40,000 | 5,000 × 24 | 120,000 |
| Time-based subtotal | 135,000 × 8 | 1,080,000 | 135,000 × 24 | 3,240,000 |
| Examination trigger, added once | Missed 11:00 | 120,000 | Missed 11:00 | 120,000 |
| Total estimated disruption | | 1,200,000 | | 3,360,000 |

Two limitations apply before these totals justify spending. The rates are defined for the first 24 hours only, so a five-day closure is not five times NGN 3,360,000. They are also financial increments. They do not price a missed examination, a double payment, a wrong certificate or reputation. I use them to compare orders of magnitude and I keep learner welfare beside the Naira figure rather than converting one into the other.

### Priorities and dependency risks

The 11:00 examination sets the first business priority because it is the earliest deadline, the highest hourly cost and the only one-off trigger. Fees follow because 16:00 is the next hard stop and the provider is still taking money that the local system cannot see. Admissions follow at 18:00. Payroll is later, at 12:00 on Day 1, but it carries an integrity rule that outranks speed. Records sit last because the commitment is 48 hours and a wrong document is worse than a queued one.

That business order is not the technical order. LMS01 cannot start until the network, AUTH01, the security checks and DB01 are done. Business priority decides only the application order after DB01: LMS01, then FIN01, then REG01, then HR01. FS01 does not need DB01, so it uses the second technician from 09:30 and happens to finish before the higher priorities. That is accepted because it does not delay LMS01.

- **Dependency risk 1.** DB01 sits in front of F1 to F4. If it fails, four functions stop.
- **Dependency risk 2.** Network, AUTH01 and the security checks are one chain. The second technician is idle and any delay moves every later finish.
- **Dependency risk 3.** Two technicians cannot split an action. After DB01, four applications compete and FIN01 cannot be used to hurry LMS01.
- **Dependency risk 4.** One shared administrator, with undocumented off-site access, can make the backups unusable even though no attack is confirmed.

A further constraint is physical. Forty learners need a centre workstation and the room has 25, so a perfect technical recovery still cannot run one 11:00 sitting.

---

## Task 2 — Recovery objectives and backup design

### Definitions and targets

Recovery Time Objective (RTO) - is the time from 08:00 to the minimum service, Recovery Point Objective (RPO) - RPO is the oldest data I will accept, measured back from 08:00. Maximum Tolerable Downtime (MTD) - is the longest outage before the harm is unacceptable and it is longer than the RTO so that checks and acceptance have a buffer. A running virtual machine is not a recovered function.

F1’s RTO is three hours because 11:00 is the business requirement and the point at which the NGN 120,000 trigger fires. I do not relax it to match the warm site. MTD is six hours, 14:00, because a same-morning sitting can still be invigilated and a later start should be deferred. F2’s RTO is six hours so two hours remain to reconcile before the eight-hour deadline. F3 leaves two hours between an eight-hour RTO and a ten-hour MTD to type captured applications into REG01. F4’s eight-hour buffer, from 20 hours to the 28-hour bank deadline, is for a second officer to check the file. An earlier unchecked file would create the duplicate payment the brief warns about. F5’s 24-hour buffer is for file integrity and access rights before a certificate is issued.

**Table 3. Recovery objective matrix. Times are from 08:00 on Day 0.**

| Function | RTO | RPO | MTD | Buffer (MTD − RTO) | Service level at which recovery is declared |
| --- | --- | --- | --- | --- | --- |
| F1 | 3 h (11:00) | 1 h | 6 h (14:00) | 3 h | A candidate can authenticate, open the scheduled paper and submit; the paper matches the invigilator copy |
| F2 | 6 h (14:00) | 1 h | 8 h (16:00) | 2 h | Finance can see receipts to the checkpoint and post a reconciled provider item. Wallets are not guessed |
| F3 | 8 h (16:00) | 4 h | 10 h (18:00) | 2 h | A captured application can be created in REG01 and is still there after a restart |
| F4 | 20 h (04:00 Day 1) | 4 h | 28 h (12:00 Day 1) | 8 h | A payroll file opens for a second officer. It is not sent to the bank until both sign |
| F5 | 24 h | 8 h | 48 h | 24 h | A registry officer opens a file whose hash matches the catalogue, with access lists intact |
| AUTH01 (shared) | Before any application | 4 h | Same as the tightest function, F1 | Held inside F1’s buffer | A known account can log on. Accounts changed after the snapshot are listed as exceptions |

### Checkpoint ages at 08:00

Age is measured back from 08:00. B02, the DB01 incremental at 07:00, is one hour old and only if B01 at 23:00 on Day minus 1 and every incremental through 07:00 are present. AUTH01 at 00:00 is eight hours old. FS01 at 23:00 on Day minus 1 is nine hours old. The disconnected full backup at 17:00 on Day minus 3 is 63 hours old, because Friday 17:00 to Monday 17:00 is 72 hours and 08:00 is nine hours earlier.

The one-hour DB01 age meets the F1 and F2 RPO with no margin, and the F3 and F4 RPO with three hours of margin. AUTH01 misses a four-hour RPO by four hours, so account changes after midnight are absent. FS01 misses the eight-hour F5 RPO by one hour. These are backup facts at 08:00, not restoration durations.

Some gaps can be rebuilt from records that were not only on the flooded site, and some cannot. Provider reports can list fees after 07:00. Email can show applications and approved payroll changes. Examination answers do not exist yet, but paper edits after 07:00 must be checked against the invigilator copy. Certificate files after 23:00 on Day minus 1 cannot be rebuilt from the provider. A successful backup flag does not close any of these gaps.

### Backup design

The design uses the 3-2-1 rule: production, a disk backup under a separate administrator and an encrypted immutable copy outside the flood plain, with keys held by the Director and the BC Coordinator. The two media are disk and immutable object storage.

DB01 keeps the 23:00 full backup and a 30-minute incremental from 06:00 to 20:00. Today’s chain is B01, then every incremental through B02 at 07:00. A gap is not skipped. AUTH01 is snapshotted every four hours, plus 07:00 on examination days. FS01 adds a four-hour incremental to its 23:00 full backup. Retention is 14 days of incrementals, eight weeks of weekly full backups, and 12 months of monthly full backups. Longer academic retention is an assumption, not this design.

An off-site copy or a live replica does not, by itself, prove recovery. The provider’s access independence is undocumented and the shared administrator could delete the copy or be unavailable in the flood. A replica would also copy a bad or malicious change. I will not call a backup recoverable until three checks pass: integrity, meaning the checksum matches and an isolated restore completes; access independence, meaning a break-glass account other than the production administrator can retrieve the immutable copy; and usability, meaning the business owner recognises a sample. One system is tested each month, and DB01, AUTH01, and FS01 are covered at least quarterly. Whether today’s provider already meets the independence test is unverified.

---

## Task 3 — Recovery strategy and budget

The packages are estimates. I compare them with Table 3, two technicians, the flood, an off-site supplier whose independence is undocumented and the risk of restoring corrupted data. Quoted first-service times are 24 to 48 hours for cold, 3 to 5 hours for warm, and about one hour for hot. Quoted data-loss windows are up to 24 hours, 1 to 6 hours and up to 15 minutes. Every package also carries NGN 400,000 for protected backups and NGN 200,000 for training and tests.

**Table 4. Annual cost of each package, including both required additions.**

| Package | Base (NGN) | Backups (NGN) | Training (NGN) | Annual total (NGN) | Headroom under 3,000,000 | Fits the limit |
| --- | --- | --- | --- | --- | --- | --- |
| Cold | 600,000 | 400,000 | 200,000 | 1,200,000 | 1,800,000 | Yes |
| Warm | 1,500,000 | 400,000 | 200,000 | 2,100,000 | 900,000 | Yes |
| Hot | 2,400,000 | 400,000 | 200,000 | 3,000,000 | 0 | Yes, with no contingency |

Cold misses F1, F2, and F3. Twenty-four to 48 hours is outside their RTOs and up to 24 hours of data loss misses every RPO. Two technicians cannot build five applications from a cold start before the examination. Warm meets F2 to F5. It does not reliably meet F1’s three-hour RTO: the quoted band crosses three hours and the model finishes assessment at 3 hours 45 minutes. A 1-to-6-hour data-loss claim meets a one-hour RPO only at its best edge. The warm site is usable only if it is outside the same flood water, which is not evidenced. An integrity check limits corruption. A shared administrator who can push a bad chain does not.

Hot meets the quoted one-hour recovery and 15-minute data loss, and it spends all of NGN 3,000,000. Two technicians have not shown they can fail over and validate inside an hour. A replica can copy a bad change while the administrator identity is shared and a same-city hot site may share the flood. I do not credit the one-hour claim until those points are evidenced.

I recommend warm at NGN 2,100,000, with NGN 900,000 of headroom. The unmet objective is plain: assessment returns at 11:45, 45 minutes past the RTO and the examination trigger and still inside the six-hour MTD. The Academic Director accepts that risk. The split sitting is the mitigation. The Director accepts not buying hot. Finance, Registry and IT Operations accept the one-hour database boundary, the one-hour FS01 gap and the four-hour AUTH01 gap until the new schedule has run for a week.

One eight-hour outage costs NGN 1,200,000 against a warm year of NGN 2,100,000. Break-even is 2,100,000 / 1,200,000 = 1.75 events a year. One 24-hour outage costs NGN 3,360,000 and break-even is 2,100,000 / 3,360,000 = 0.625 events a year. Neither rate is known. One scenario is a severity, not a frequency and the model ignore welfare, so it cannot prove an annual return. Before spending or a supplier claim is accepted, I want two timed two-technician restores, a written 3-to-5-hour method, flood-plain evidence, a separate backup administrator and a quote split into base, NGN 400,000 and NGN 200,000.

---

## Task 4 — Business continuity and disaster recovery plan

### Activation, authority and teams

Scope is the five functions, their shared systems, the alternate facility and the return home. The plan activates if the site or its local systems will miss a deadline or if the outage will exceed one hour. The flood meets both tests at 08:00. A suspect backup, AUTH01, or administrator logon also activates incident response. The flood alone does not. The Institute Director approves activation, external statements and the return. The Academic Director is the deputy. If neither is reached within 15 minutes, the BC Coordinator starts the temporary procedures and records the gap. Contacts are printed role cards and the Microsoft 365 group, not a file that lives only on FS01.

The continuity team runs minimum services and learner messages. The recovery team is the two technicians, led by Technician 1 as DR Lead. Facilities control the flooded building and the alternate room. Incident response is not in command of the flood. It is called on a security indicator and it can stop a restore. Situation reports go to the Director at 08:30, then hourly until each function is accepted or deferred, then every four hours.

**Table 5. RACI. Each activity has one accountable role and at least one responsible role.**

| Activity | Responsible | Accountable | Consulted | Informed |
| --- | --- | --- | --- | --- |
| Activation | BC Coordinator | Institute Director | DR Lead, Facilities | All staff |
| Temporary assessment delivery | Examinations officer and business coordinators | Academic Director | BC Coordinator | Learners, Director |
| Backup validation | Technician 2 | DR Lead | Incident response if a check fails | Function owners |
| Restoration | Both technicians | DR Lead | Function owners | BC Coordinator, Director |
| Stakeholder communication | BC Coordinator | Communications Lead | Academic Director, Legal if data are in doubt | Director, learners, staff |
| Return to the primary site | Facilities Manager | Institute Director | DR Lead and each function owner | All staff and learners |

Each row has one accountable person. Communications is accountable for the words. The BC Coordinator sends them. The Director remains accountable for activation and for the return.

### Temporary procedures for 72 hours

The building may be shut for 72 hours while technology returns sooner. Until a function is accepted, the business uses the minimum in Table 1. Minimum people are the Academic Director or a deputy, two invigilators and one officer each for examinations, finance, admissions, HR and registry, plus the two technicians. The technicians are not pulled into invigilation. Equipment is 25 workstations from 10:00, mobile internet, a printer, a locked box for registers and Microsoft 365. Suppliers are the alternate site, the payment provider and the bank.

F1 cannot be one 11:00 sitting: 40 learners need a centre machine, only 25 exist and LMS01 is not expected before 11:45. The Academic Director approves waves of 25 and 15, access arrangements first, with a second paper if the groups cannot be separated. Own-device learners start after LMS acceptance, using the SMS link. The paper register is typed by two officers into a restricted Microsoft 365 library and the invigilated script is the record. F2 uses the provider report, agreed by two officers, with no wallet update until reconciliation. F3 treats email before 18:00 as in time. F4 sends nothing to the bank before a second signature. F5 issues nothing until FS01 is accepted.

After restoration, four reconciliation rules apply. The provider wins on money received. The email timestamp wins on whether an application was in time. The invigilated script wins on the examination. An approved payroll change wins over a silent database. Conflicts are listed and signed. They are not overwritten quietly.

### Disaster recovery runbook

The runbook restores technology in order. Row 9, signed by the function owner, is what resumes the business service. This order assumes the integrity checks pass.

**Table 6. Disaster-recovery runbook. Rows 3 to 8 restore technology. Row 9 resumes a business service.**

| Order | Action | Prerequisite | Owner | Completion check | If the step fails |
| --- | --- | --- | --- | --- | --- |
| 1 | Confirm activation and warn both technicians | Flood report | BC Coordinator | Director or deputy informed | Deputy activates and the log records the gap |
| 2 | Open the alternate room for 10:00 | Activation | Facilities | 25 workstations powered | Drop to the 15-workstation pattern and tell the Academic Director |
| 3 | Select the backup before any data move: B01 plus the incremental chain through B02, AUTH01 at 00:00, FS01 at 23:00 Day minus 1 | Catalogue readable | DR Lead | Chain listed with no missing incremental | Stop. Do not skip a broken incremental. Call the BC Coordinator |
| 4 | Recovery network, 30 min | None | Technician 1 | Recovery hosts can be addressed | Repeat once. If it fails again, extend manual procedures and report an unknown RTO |
| 5 | Restore AUTH01, 30 min | Network | Technician 1 | Known test account logs on | Do not start DB01 or FS01 |
| 6 | Security and backup checks, 30 min | AUTH01 | Technician 1 | Checksums match and no unexpected admin logon | Stop data restores. Activate incident response |
| 7 | Restore DB01, 90 min, and FS01, 60 min, in parallel | Checks passed; two technicians free | Technician 1 on DB01, Technician 2 on FS01 | Both jobs finish their local validation | The failed job does not release its dependent applications |
| 8 | Restore LMS01 (45), then FIN01 (30) in parallel from 11:00, then REG01 (30), then HR01 (45) | DB01 accepted by the technician | Both technicians, LMS01 first | Each application opens on the restored data | Keep that function on its manual procedure |
| 9 | Business acceptance, function by function | Application check passed | Each function owner | Signed minimum-service statement | Service stays manual. Technology is not announced as open |
| 10 | Rollback | A failed acceptance or a later integrity alarm | DR Lead | Restored segment disconnected | Return to the manual procedure and preserve the failed copy for investigation |

### Communication, incident response and return

**Table 7. Stakeholder communication during the site outage.**

| Stakeholder | Trigger | Owner | Channel | Frequency |
| --- | --- | --- | --- | --- |
| Learners | Activation, and any change to the examination | Communications issued by the BC Coordinator | SMS and Microsoft 365. Not the main-site phones | At 08:30, at 12:00, then when a sitting time is firm |
| Staff | Activation | BC Coordinator | Microsoft 365 and the printed phone tree | At activation and at each situation report |
| Director | Activation and any missed deadline or failed backup | BC Coordinator | Mobile | 08:30, then hourly until services are accepted or deferred |
| Payment provider | Need for a settlement report | Finance Manager | Provider portal | Once before 14:00, and again if exceptions remain |
| Alternate-site supplier | Room open, or a shortfall in workstations | Facilities | Supplier phone | At 08:15 and if the room changes |
| Incident response | Failed integrity check or unexpected backup-administrator logon | DR Lead | Phone to the IR Lead | Immediately, then on the IR cadence |

> Northbridge Skills Institute learner notice, issued at 08:30 WAT on Day 0. Flooding and a power failure have closed the main site and the local teaching and records systems are unavailable. Email through Microsoft 365 on a mobile connection is still working. This message is also being sent by SMS. The examination cannot start at 11:00 for the whole cohort. An alternate centre opens at 10:00 with 25 workstations, and 40 learners need a centre machine, so the sitting will be divided. Do not travel to the main site. Wait for an SMS naming your sitting or telling you that you may use your own device. Bring your student identity card. If you use your own device, open only the institute link in that SMS. Assessment rules still apply. Learners with an access arrangement should phone the examinations office on +254 123 000 2010 before 10:00. Your place is held. This notice is about the site closure. It is not a cyber-incident notice. The next update will be at 12:00 WAT.

Incident response is engaged if a backup fails its integrity check, if a backup administrator logs on unexpectedly, if ransom notes or mass file changes appear or if restored data show signs of tampering. A flood, a power cut and a late examination do not by themselves engage it. Once engaged, incident response can halt restoration. Continuity continues on the manual procedures while that halt lasts.

Return to the main site waits for facilities clearance of structure, water, and electrics. There is no automatic cut-back. Records created at the alternate site move under the section 5.2 rules, with manual entry frozen during the transfer. Each function owner signs, and the Director signs last. Until then, the alternate procedures remain the authorised service.

---

## Task 5 — Recovery schedule and exercise

### Modelled baseline schedule

Table 8 is a model of the current warm-site timings, not a live test. Each action occupies one technician for its whole duration. Two actions run together only when the prerequisites are done and a technician is free. Actions are not split. Technical completion is not business acceptance.

Technician 2 is idle from 08:00 to 09:30 because network, AUTH01 and the security checks are one chain and idle again from 10:30 to 11:00 while DB01 finishes. From 09:30, DB01 and FS01 run together. From 11:00, LMS01 and FIN01 run together, then REG01, then HR01. Putting another application ahead of LMS01 cannot make the examination earlier.

**Table 8. Modelled baseline schedule. Not a live test. All five technical restores finish at 12:30.**

| Action | Technician | Start | Finish | Prerequisite |
| --- | --- | --- | --- | --- |
| Recovery network (30 min) | T1 | 08:00 | 08:30 | None |
| AUTH01 restoration (30 min) | T1 | 08:30 | 09:00 | Recovery network |
| Security and backup checks (30 min) | T1 | 09:00 | 09:30 | AUTH01 |
| DB01 restoration (90 min) | T1 | 09:30 | 11:00 | Security and backup checks |
| FS01 restoration (60 min) | T2 | 09:30 | 10:30 | AUTH01 and security checks |
| LMS01 restoration (45 min) | T1 | 11:00 | 11:45 | DB01 |
| FIN01 restoration (30 min) | T2 | 11:00 | 11:30 | DB01 |
| REG01 restoration (30 min) | T2 | 11:30 | 12:00 | DB01 |
| HR01 restoration (45 min) | T1 | 11:45 | 12:30 | DB01 |

The earliest technical restoration of the assessment service is 11:45. The path that controls that time is the recovery network (30 minutes), AUTH01 (30), the security and backup checks (30), DB01 (90) and LMS01 (45). The sum is 225 minutes, which is 3 hours 45 minutes. FS01 is not on this path. The other applications are not on it either. All five functions are technically complete at 12:30, which is 4 hours 30 minutes from 08:00.

**Table 9. Modelled elapsed time and checkpoint age compared with the targets. Baseline only.**

| Function | Technical finish | Elapsed | RTO | MTD | Checkpoint used | Checkpoint age | RPO result |
| --- | --- | --- | --- | --- | --- | --- | --- |
| F1 LMS01 | 11:45 | 3 h 45 m | 3 h — missed by 45 m | 6 h — met | DB01 07:00 | 1 h | Met, no margin |
| F2 FIN01 | 11:30 | 3 h 30 m | 6 h — met | 8 h — met | DB01 07:00 | 1 h | Met, no margin |
| F3 REG01 | 12:00 | 4 h 00 m | 8 h — met | 10 h — met | DB01 07:00 | 1 h | Met, margin 3 h |
| F4 HR01 | 12:30 | 4 h 30 m | 20 h — met | 28 h — met | DB01 07:00 | 1 h | Met, margin 3 h |
| F5 FS01 | 10:30 | 2 h 30 m | 24 h — met | 48 h — met | FS01 23:00 Day −1 | 9 h | Missed by 1 h |
| AUTH01 | 09:00 | 1 h 00 m | Before applications | Inside F1 MTD | AUTH01 00:00 | 8 h | Missed by 4 h against a 4 h RPO |

Even on this successful model, the 11:00 examination needs a continuity workaround. Assessment is 45 minutes late and 15 of the 40 centre learners still have no seat in the first wave. Fee confirmation at 16:00 does not need a late system, because FIN01 is back at 11:30, but payments after 07:00 are not in the database and must be read from the provider. Admissions at 18:00 can be met if email capture starts in the morning. Payroll on Day 1 is met if the file is still checked. Records are back at 10:30. Files saved after 23:00 on Day

### Stress exercise kept separate from the baseline

The following 45-minute walk-through is a simulation. I played each role. None of these injects changes Table 8. The baseline remains the clean model. This section is what the team does when the clean model breaks.

**Table 10. Simulated stress decisions. Separate from the baseline schedule.**

| Inject | Decision and revised action | Owner | Rationale and effect |
| --- | --- | --- | --- |
| Minute 10. The closure becomes five days, not 72 hours. | Keep the technical schedule. Extend roster, consumables, and the manual procedures to five days. Do not wait for the building before serving restored systems at the warm site. | BC Coordinator, accepted by the Director | The building and the systems are different constraints. A longer flood does not, by itself, move the 11:45 LMS time. It does exhaust a 72-hour staffing plan. F5 can still be served from the restored FS01. |
| Minute 20. Only 15 workstations work. | Three sittings for the 40 centre learners: 15, 15, and 10. Access arrangements go first. Use a second paper form if cohorts cannot be separated. The 80 own-device learners wait for LMS acceptance. | Academic Director | Fifteen machines cannot hold the planned first wave of 25. Integrity falls if one paper is reused across a gap. The examination day becomes longer. The RTO miss grows in practice even though the server clock is unchanged. |
| Minute 30. A DB01 incremental fails its integrity check. A backup administrator has logged on unexpectedly. | Do not apply the failed incremental. Fall back to B01 at 23:00 on Day minus 1, which is nine hours old. Disable the shared administrator. Activate incident response before any further restore. | DR Lead for the fallback. IR Lead for the logon. | F1 and F2 miss the one-hour RPO by eight further hours. Examination papers must be checked against the invigilator copy. The logon means the flood is no longer the only hypothesis. This does not rewrite Table 8; it replaces the data point for this stress case only. |
| Minute 40. The provider shows payments that are absent from the restored finance records. | Treat the provider report as the record of money received. Do not mark those learners unpaid. Hold wallet updates. Finance produces an exception list before F2 is accepted. | Finance Manager | The gap is expected after 07:00, and it is larger if minute 30 has already dropped the team back to B01. It is not, by itself, proof of fraud. Combined with the unexpected logon, incident response is told. F2 stays unaccepted until the exceptions are posted or formally deferred. |

**Table 11. Corrective actions from the stress exercise.**

| Priority | Corrective action | Owner | Target | Completion evidence |
| --- | --- | --- | --- | --- |
| High | Separate the backup administrator from the production administrator, with MFA, and disable the shared identity. | IT Operations Manager | 30 days | Access review and a failed logon test with the old shared account |
| High | Put an immutable off-site copy in place and complete one isolated DB01 restore with a business sample. | DR Lead | 60 days | Restore report with start, finish, checksum, and the owner’s sample note |
| High | Write the 25-workstation and 15-workstation examination patterns, including a second paper form. | Academic Director | 45 days | Approved procedure and a 30-minute tabletop attendance sheet |
| Medium | Agree a payment-reconciliation runbook with the provider and test it on a sample file. | Finance Manager | 45 days | Signed sample reconciliation |
| Medium | Contract the alternate site for 25 workstations, a surge toward 40, five-day occupancy, and a location outside the main flood plain. | Facilities Manager | 90 days | Supplier letter stating capacity, duration, and location |
| Medium | Change AUTH01 to a four-hour snapshot and FS01 to a four-hour incremental, and run DB01 every 30 minutes in the teaching day. | Backup administrator | 30 days | One week of successful job records from the console |

---

## Task 6 — Governance and performance measures

The BC/DR Program Manager owns the plan. The Institute Director approves it. The Academic Director may activate if the Director cannot. Version 1.0 starts when the Director signs. Review follows every activation, exercise or material system change and otherwise happens quarterly. Copies sit in the Microsoft 365 group and in print at the alternate site. The only copy must not be the one on FS01.

Testing follows risk. One critical system is restored each month, rotating DB01, AUTH01 and FS01. A quarterly tabletop rotates flood, ransomware and supplier loss. A timed two-technician drill is annual and is scored against Table 3. Named roles train yearly and a new post-holder train within 30 days. The NGN 200,000 line funds that calendar. A full production interruption is not proposed this year.

**Table 12. Educational alignment only. This mapping is not a claim of certification.**

| Output in this paper | NIST SP 800-34 Rev. 1 step | ISO 22301:2019 theme |
| --- | --- | --- |
| Activation authority, deputy, and escalation | Step 1. Contingency planning policy | Clause 5. Leadership and roles |
| Five-function BIA, costs, and priorities | Step 2. Business impact analysis | Clause 8. Operation, which includes the BIA |
| Warm-site choice, backup schedule, and the decision not to rely on a generator against a flood | Steps 3 and 4. Preventive controls and contingency strategies | Clause 6. Planning and clause 8 strategies |
| Runbook, temporary procedures, and return rules | Step 5. Information system contingency plan | Clause 8. Plans and procedures |
| Tabletop, timed schedule, and corrective actions | Step 6. Testing, training, and exercises | Clause 9. Performance evaluation, and clause 10. Improvement |
| Version control, quarterly review, and the KPIs below | Step 7. Plan maintenance | Clause 9 measurement and clause 10 corrective action |

The flood is the climate point under ISO 22301:2019 Amendment 1:2024, which puts climate change into organisational context. The 72-hour estimate is uncertain, as the exercise extends it to five days, and the alternate site and the immutable copy must sit outside the main flood plain. A generator does not restore a flooded teaching room. This alignment is not a certification claim.

**Table 13. Performance measures. Each KPI has a formula, a source, a target, a frequency, and an owner.**

| KPI | Formula | Data source | Target | Frequency | Owner |
| --- | --- | --- | --- | --- | --- |
| Recovery performance | Elapsed time to the declared minimum service, divided by the RTO, for each function | Exercise and incident logs | At or below 1.0 for F1 to F5 | Each drill, reported quarterly | DR Lead |
| Restoration-test coverage | Critical systems with a successful restore test in the last 90 days, divided by DB01, AUTH01, FS01, and LMS01 | Backup console and test reports | 100 percent of those four | Monthly | Backup administrator |
| Corrective-action closure | Actions closed with the evidence in Table 11 by the target date, divided by actions due | Action tracker | At least 90 percent | Monthly | BC/DR Program Manager |
| Training | Named roles who completed the annual exercise or training, divided by named roles | Training register | 100 percent of named roles | Quarterly | BC Coordinator |

On the slide 13 model, Northbridge is Level 2, Developing. A timing sheet, a backup catalogue and a counted alternate room already exist, so Level 1 would be too low. Recovery is not yet consistent: access independence is undocumented, the administrator is shared, the RPO gaps are open and a 90-day restore test is not evidenced. The 12-month target is Level 3, Defined, shown by a signed plan, two exercises, a separate backup account, one immutable restore, the new schedule and the training KPI at target. Level 4 is beyond this budget and these two technicians.

---

## References

International Cybersecurity and Digital Forensics Academy. (Week 21). Business continuity and disaster recovery [PowerPoint slides]. GRC205, slides 1–15.

International Organization for Standardization. (2019). ISO 22301:2019. Security and resilience — Business continuity management systems — Requirements. https://www.iso.org/standard/75106.html

International Organization for Standardization. (2024). ISO 22301:2019/Amd 1:2024. Climate action changes. https://www.iso.org/standard/88412.html

National Institute of Standards and Technology. (2010). Contingency planning guide for federal information systems (NIST Special Publication 800-34 Revision 1). https://csrc.nist.gov/publications/detail/sp/800-34/rev-1/final
