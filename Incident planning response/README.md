# Incident Response Planning

**GRC105 · Incident Management and Business Continuity · Week 20**  
International Cybersecurity and Digital Forensics Academy

Fictional training scenario. Northbridge Skills Institute is not a real organisation, and the times, systems, and telephone numbers below are exercise data.

| | |
| --- | --- |
| Student | Edna Moraa Maburi |
| Student ID | 2025/GCR/12688 |
| Official email | [gcr2512688@students.icdfa.ed.ng](mailto:gcr2512688@students.icdfa.ed.ng) |
| Programme | Governance, Compliance, and Risk Engineering |
| Role in the case | Incident Response Lead, from 08:45 WAT on Day 0 |
| Classification | Critical · suspected ransomware |

## Case at a glance

At 08:45 on Day 0 the institute is facing mass file changes on FIN-PC07, `.locked` files and a ransom note on FS01, and an online examination for 100 learners at 11:00. The decision in this plan is to isolate first, postpone the examination, and refuse an unverified restore of the 02:00 backup. Confidentiality impact is not confirmed.

## Contents

1. [Incident classification and initial assessment](#task-1--incident-classification-and-initial-assessment)
2. [Incident response plan and team responsibilities](#task-2--incident-response-plan-and-team-responsibilities)
3. [Response playbook and evidence preservation](#task-3--response-playbook-and-evidence-preservation)
4. [Communication and escalation](#task-4--communication-and-escalation)
5. [Tabletop exercise and improvement plan](#task-5--tabletop-exercise-and-improvement-plan)
6. [Case study analysis and critical reflection](#task-6--case-study-analysis-and-critical-reflection)
7. [References](#references)

---

## Task 1 — Incident classification and initial assessment

### Timeline using E01 to E08

| ID | Time (WAT) | Confirmed observation | Working hypothesis | Unresolved |
| --- | --- | --- | --- | --- |
| E01 | Day minus 1, 16:40 | A finance officer opened an unexpected payroll attachment on FIN-PC07. A mail record exists. | The attachment may have been the initial malware delivery. | Execution is not verified. The file may be benign or may have failed to run. |
| E02 | Day 0, 01:15 | Microsoft 365 records a sign-in of the finance-officer account from an unfamiliar IP and a new device. | The session may be an attacker using that account. | MFA result and whether the officer took the action are unknown. |
| E03 | Day 0, 01:35 | svc_backup made remote administrator logons to FS01 and BAK01 outside its usual schedule. | Stolen backup credentials may be in use against the file server and the backups. | A legitimate out-of-hours job has not been ruled out. The reason is unknown. |
| E04 | Day 0, 01:50 | FIN-PC07 uploaded 1.2 GB over HTTPS to unfamiliar cloud storage. The content is not in the record. | The upload may be staged personal or finance data. | Content, destination owner, and whether the transfer completed usefully to an attacker are unknown. |
| E05 | Day 0, 02:00 | The backup console reports success for the FS01 and LMS01 jobs. | The job may have copied already hostile files, or it may be a clean copy. | Recoverability and freedom from compromise are not established. The job has not been restored. |
| E06 | Day 0, 08:30 | Mass file changes on FIN-PC07. FS01 shared folders contain `.locked` files and a ransom note. EDR and the helpdesk both report this. | Ransomware is encrypting or renaming data on the finance PC and the shared server. | The malware family is unconfirmed. Scope beyond these two systems is unknown. |
| E07 | Day 0, 08:40 | The portal login page loads. Learners cannot open materials or submit assessments. | LMS01 depends on FS01, so the file damage is breaking the learning service. | Damage to the LMS01 operating system itself is unknown. |
| E08 | Day 0, 08:45 | The incident contact list on FS01 cannot be opened. Staff phones work. I take command at this time. | Command communication that depends on FS01 or on corporate email is unreliable. | Whether mailboxes are trustworthy is unverified, because of E02. |

### Ambiguous events that have alternative explanations

- **E02.** The unfamiliar IP and new device are consistent with an attacker, and they are also consistent with the finance officer signing in from home on a new phone. The officer may have approved an MFA prompt, or MFA may have been bypassed, or MFA may not be enforced on that account. Those are different findings.
- **E04.** A 1.2 GB HTTPS upload is consistent with data theft, and it is also consistent with the officer sending a large legitimate payroll archive to a personal or vendor cloud store. The proxy record cannot decide between them.

### Incident type and severity

The incident type is suspected ransomware affecting a finance workstation and the shared file server, with a possible but unproven account compromise and a possible but unproven data transfer.

| Level | Criteria |
| --- | --- |
| Critical | Active encryption or destruction of a shared service, a likely personal-data breach of the learner population, or loss of a time-critical service such as today’s examination, where full-team response and executive notification cannot wait. |
| High | Confirmed malware or unauthorised access on a limited system, with a defined response time and management notification, but without active encryption of a shared service and without an immediate examination or breach decision. |
| Medium | A contained policy violation or single-host suspicion, with no shared-service outage and no indication of data leaving the institute. Handled in the ticket queue. |
| Low | A reported anomaly that is explained by a fault or by user error, with no security impact left open. |

I classify the incident as **Critical**. Availability is already broken for the shared folders and for assessment submission (E06, E07), and the examination is two hours and fifteen minutes away. Integrity of FIN-PC07 and of the FS01 shares is confirmed damaged by mass file changes, `.locked` files, and a ransom note (E06). Confidentiality is not confirmed, but E04 keeps a large unexplained transfer inside the impact assessment, and the files on FS01 include learner contact details, admission documents, assessment records, and finance files. Uncertainty raises the priority of investigation. It does not lower the operational severity, because the examination and the backups cannot wait for a finished forensic report.

### Assets, CIA, and priorities

| Asset | Status | Confidentiality | Integrity | Availability |
| --- | --- | --- | --- | --- |
| FIN-PC07 | Confirmed affected | Possible: 1.2 GB left the host (E04). Content unknown. | Confirmed: mass file changes (E06). | Degraded. Host will be isolated while left powered on. |
| FS01 shares | Confirmed affected | Learner, admission, assessment, and finance files reside here. Theft not proven. | Confirmed: `.locked` files and a ransom note (E06). | Confirmed unavailable for normal use (E06, E08). |
| LMS01 | Potentially affected | No evidence of direct access. | Unknown. Login page still loads (E07). | Partially down: login loads, materials and submission do not (E07). |
| BAK01 and both backup routes | Potentially affected | Backup sets may contain the same personal data. | Not established. Jobs are not immutable and svc_backup logged on (E03, E05). | Must be treated as untrusted until a restore test passes. |
| Finance-officer account and svc_backup | Potentially compromised | Account access could expose mail and admin paths (E02, E03). | Unknown whether mailbox rules changed. | Sessions will be revoked. Accounts are not available for normal use. |
| Microsoft 365 | Potentially affected | Identity provider for email. Sign-in anomaly (E02). | Unverified. | Email must not be the command channel until the anomaly is explained (E08). |
| Friday 17:00 disconnected backup | Candidate, not confirmed clean | Holds historical personal data. | Last disconnected copy. Not tested in 90 days. | Offline from the network, which is its value. |

**Response priorities, in order**

1. **Contain the spread.** Stop active encryption and stop svc_backup from writing to or deleting BAK01. Isolation is authorised now because E06 shows file damage already in progress at 08:30.
2. **Preserve evidence that containment will not destroy.** Keep FIN-PC07 powered on, copy logs off FS01, and record what isolation itself will destroy. Evidence comes before rebuild, not before containment.
3. **Protect the examination from a bad recovery.** Decide by 09:15 that the 11:00 examination will not run on LMS01. Chasing a portal reopen on an untested backup is the wrong priority.
4. **Secure privileged and cloud accounts.** Revoke the finance-officer sessions and svc_backup, then look for the same pattern on other hosts and in Microsoft 365.
5. **Communicate.** Tell learners and the Director what is known, on a channel that does not depend on FS01 or on untrusted mail.

### Priority investigation questions

| Question | Check that can resolve it | What would change severity or scope |
| --- | --- | --- |
| Did the Day −1 attachment execute, and is it the same activity as the `.locked` files? | EDR process tree and file reputation on FIN-PC07; mail-gateway record for E01; planned disk review. Interview the finance officer by phone. | If the file never executed and the `.locked` files are confined to one copied folder, host scope narrows. Severity stays at least High because FS01 is already damaged. If the same binary is on LMS01 or BAK01, scope expands and the portal is a confirmed victim, not only a dependent. |
| What was in the 1.2 GB upload, and did the finance officer recognise it? | Proxy or secure-web-gateway record for destination and URL; cloud-app sign-in if the destination is a known service; finance-officer interview; mailbox send/download audit. | If the content is learner or finance personal data, counsel starts the Nigeria Data Protection Act assessment and confidentiality impact becomes confirmed. If it is a known vendor transfer the officer can explain, confidentiality impact is reduced. Severity would not fall below High while E06 stands. |
| Did the svc_backup logons alter, encrypt, or delete backup sets? | Windows Security log detail for E03, backup-catalog change history, file timestamps on BAK01, and a read-only listing of the Friday disconnected set. | If the Friday 17:00 set is intact and the Monday 02:00 set is not, the recovery point moves to Friday and the data-loss window is about 63 hours. If both sets are damaged, availability impact deepens and the examination cannot be rebuilt from institute backups in the short term. |
| Is the finance-officer session still active, and was MFA satisfied? | Microsoft 365 sign-in log, MFA claim, and impossible-travel or new-device fields for E02. Force sign-out is done regardless. | An active session means identity containment is incomplete and email remains unsafe. A confirmed MFA fatigue prompt widens the incident to the identity provider. A log that shows the officer’s own registered device, with the officer’s confirmation, supports the benign alternative for E02 only. |

---

## Task 2 — Incident response plan and team responsibilities

### Policy and plan

**Mission.** Northbridge’s incident-response plan exists to limit harm to learners, staff, assessment integrity, and personal data, and to return only those services that can be shown to be safe.

**Objectives.** Detect and classify within minutes of a credible report. Contain active harm before forensic completeness. Preserve evidence with a written custody record. Recover the examination service only from a validated source.

**Scope.** The plan covers FIN-PC07, FS01, LMS01, BAK01, Microsoft 365 identity and email, privileged accounts, and the examination service that depends on them. It covers security incidents. A flood or power loss with no security indicator is handled by the business-continuity plan unless a security indicator appears.

**Declaration.** Any staff member can report. The Incident Manager declares the incident and its severity using the criteria in section 2.2. A ransom note, active mass file changes, or an unexplained privileged logon to BAK01 is enough to declare. Declaration does not wait for a malware-family name.

**Activation and escalation.** Critical: Director and Academic Director by phone within 15 minutes, legal adviser within 30 minutes if personal data may be involved. High: management within one hour. The Incident Manager may escalate severity without permission. Only the Incident Manager, with the Director informed, may de-escalate.

**Authority to isolate.** The Incident Manager may order immediate isolation of a host, a share, or an account when encryption is in progress, when a privileged backup account is active outside schedule, or when a session is sending data out. The business owner is informed. The business owner cannot delay that isolation in order to keep a service online. Returning a service to learners is a different decision and needs the business owner’s approval.

**Documentation.** From 08:45 the incident log records time, evidence ID, decision, owner, and what was not known. Evidence handling follows a register. Notifications are kept, with the words that were sent.

**Review.** The plan is reviewed after every Critical incident, after every exercise, and at least once a year. The post-incident review is held within two weeks of closure.

**Link to business continuity.** Incident response decides whether a system is safe to use. Business continuity decides how teaching continues while it is not. For this incident the continuity measure is a postponed examination and an out-of-band learner message, not a forced reopen of LMS01. Ransomware needs both containment and an alternate way to deliver the critical service. A continuity request that would restore an unvalidated backup is refused, because it can import the incident into the recovered service.

### CSIRT and extra support

The standing technical team is three IT staff and one security analyst. Six response roles are still named. People hold more than one role only where the matrix shows it, and the gaps are filled by named external support.

| Role | Who holds it today | Authority | Out-of-band contact |
| --- | --- | --- | --- |
| Incident Manager | Security analyst, acting as IR Lead | Declare, classify, order isolation, keep the log, brief the Director | +254 123 000 2001 |
| Security Analyst | Same analyst, analysis tasks first; IT staff member 3 takes triage if the lead is in command calls | Scope checks, identity signals, EDR queries | +254 123 000 2001 |
| Forensics | External retainer, engaged at 09:35. Until arrival, IT staff member 2 preserves images under the lead’s direction and does not analyse by guessing | Custody, imaging, malware identification | +254 123 000 2007 |
| IT Operations | IT staff members 1 and 2 | Switch ports, shares, backup jobs, rebuilds | +254 123 000 2006 |
| Business owner | Academic Director, examinations | Postpone or accept return of the examination service. Cannot veto emergency isolation | +254 123 000 2003 |
| Legal and privacy | External counsel | Decide whether a statutory notification threshold is met. Does not send operational notices | +254 123 000 2004 |
| Communications | Registrar, not a technical post | Learner and staff wording. Journalists go only through this role | +254 123 000 2005 |

Additional support is obtained in a defined order. Microsoft 365 support is opened from the admin portal for the E02 sign-in, not by mailing a personal Microsoft address. The backup-product vendor is asked how to mount the Friday media read-only. A sector information-sharing contact may be used after counsel agrees. Law enforcement is a Director decision on counsel’s advice, not an automatic call. If a second campus host starts encrypting, the Incident Manager asks the retainer for an on-site analyst the same day rather than pulling the third IT staff member off containment.

### RACI

R = Responsible · A = Accountable · C = Consulted · I = Informed. Each activity has one accountable role.

| Activity | Incident Manager | Security Analyst | Forensics | IT Operations | Business owner | Legal / privacy | Communications |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Detection | A | R | I | R | I | I | I |
| Analysis | A | R | R | C | I | C | I |
| Containment | A/R | R | C | R | C | I | I |
| Evidence handling | A | C | R | C | I | C | I |
| Stakeholder communication | A | I | I | I | C | C | R |
| Reporting-obligation assessment | C | C | C | I | I | A/R | C |
| Restoration approval | C | C | C | R | A | I | I |
| Closure or review | A/R | R | C | C | C | C | C |

Restoration approval is accountable to the business owner because only that role can accept an examination service back. IT Operations is responsible for the technical restore and cannot approve it alone. The reporting-obligation assessment is accountable to legal, because the Incident Manager must not invent a regulatory conclusion.

### Readiness improvements

| Improvement | Owner | Resources | How readiness is checked |
| --- | --- | --- | --- |
| Backup readiness: one offline or immutable copy of FS01 and LMS01, with a separate account from svc_backup, plus a restore test. | IT Operations | Immutable storage or a disconnected disk rotation; a second admin identity; a four-hour test window each quarter. | A written restore of a sample share succeeds in the last 90 days, and the test log is stored off FS01. |
| Incident contact list kept off FS01: printed cards and a phone group held by the Director, the Registrar, and IT. | Incident Manager | Printed cards, one hour to build the list, a deputy for each role. | A quarterly call-down reaches every role without opening FS01 or corporate email. |
| EDR on FS01, LMS01, and BAK01, not only on workstations. | Security Analyst | Server licences, a change window, a week of tuning. | Coverage report shows all three servers reporting. A test alert is acknowledged. |
| Command channel that is not corporate email: a pre-agreed mobile channel for the seven roles, with MFA on personal devices. | Communications | Approved app, enrolment of the seven numbers, a one-page rule for what may be written there. | A monthly test message is acknowledged by all seven roles. |
| Two short exercises a year, one of them this ransomware pattern, and a five-minute staff rule for unexpected payroll attachments. | Incident Manager | Two hours of staff time; the finance team for the phishing scenario. | An after-action note exists, and finance staff can state the attachment rule when asked. |

---

## Task 3 — Response playbook and evidence preservation

### Ordered playbook

The playbook is the plan from 08:45. Collection steps are planned. A completion check is how the step is shown to have happened.

| Time | Action | Owner | Approval | Evidence to retain | Completion check |
| --- | --- | --- | --- | --- | --- |
| 08:45 | Declare a Critical incident. Open the log. Assign the seven roles by phone. | Incident Manager | None. Declaration is pre-authorised. | Incident log with E06, E07, E08. | Log open. Director’s phone answered or a deputy recorded. |
| 08:48 | Call the Director and the Academic Director. State confirmed facts only. | Incident Manager | None for the call. | Call note: time, words used, decision asked. | Both roles informed of encryption and of the 11:00 exam. |
| 08:50 | Isolate FIN-PC07 from the network. Leave it powered on. | IT Operations | Incident Manager. Pre-authorised because encryption is active (E06). | Switch-port or cable record. Photograph of the screen if a note is visible. | Host has no network path and is still powered. |
| 08:53 | Revoke Microsoft 365 sessions for the finance officer and disable svc_backup. Reset both passwords. Block the E02 IP. | Security Analyst with IT Operations | Incident Manager | Planned export of the sign-in and admin logs. Ticket number of the disable action. | Both accounts show no active session. |
| 08:57 | Remove user write access to FS01 shares. Pause backup jobs. Restrict BAK01 so svc_backup cannot connect. | IT Operations | Incident Manager | Backup-console record that jobs are paused. Time of the share change. | No backup job is running. Users cannot write to the shares. |
| 09:02 | Copy proxy, Windows Security, and backup-console logs onto encrypted media that is not FS01. | Forensics lead, or IT staff member 2 until the retainer arrives | None | Media label, source path, time, collector. Hash calculated at copy time and recorded then, not before. | Media is in the IR bag. Paths are listed in the log. |
| 09:08 | Scope hunt: EDR for other `.locked` hosts; Microsoft 365 for other risky sign-ins; LMS01 for new tasks and new accounts. Do not log learners back in. | Security Analyst | None | Query text and result list, including a nil result. | A written host list says in scope, clean, or not yet checked. |
| 09:15 | Decide the examination. Do not mount the 02:00 backup. Recommend postponement. | Incident Manager | Academic Director for the postponement itself. | Decision record citing E03, E05, E06, E07. | Postponement recorded, or a written acceptance of risk if the Director overrides, which this plan advises against. |
| 09:25 | Send the learner notice by SMS and through tutors. Do not use corporate email as the only channel. | Communications | Incident Manager and Academic Director | Approved text and the send log. | Send log stored off FS01. |
| 09:35 | Brief counsel on confirmed facts versus hypotheses. Ask whether any notification clock has started. | Incident Manager | None to consult counsel. | The fact pack and counsel’s answer. | Counsel has said either that the clock has not started, or that it has, with the time. |
| 10:00 | Executive update. Repeat unknowns. Ask for retainer spend if a second host is encrypting. | Incident Manager | Director for external spend. | Briefing text from Task 4 and any new evidence ID. | Update sent on the phone channel. Next update time set. |
| 11:30 | Start the planned forensic image of FIN-PC07 if it is still powered and isolated. If imaging would let encryption finish on a connected disk, image after the disk is detached, and record the volatile data that was lost. | Forensics | Incident Manager | Custody entry. Hash only after the image exists. | Image started, or a written reason why it was deferred. |
| 13:00 | Longer-term containment: review all global administrators, disable legacy authentication if it is on, and block the E04 destination at the proxy. | IT Operations and Security Analyst | Incident Manager | Admin list and the block rule. | E04 destination blocked. Admin list reviewed and signed. |
| 15:00 | Select the Friday 17:00 disconnected backup as the candidate. Mount it read-only in an isolated network. Do not use the 02:00 job. | IT Operations | Incident Manager. Not the business owner yet. | Media ID, mount time, network ID of the isolated segment. | Mount is isolated. Production LMS01 is unchanged. |
| 18:00 | Eradication plan for FIN-PC07: rebuild from a gold image, then check persistence. Do not clean the encrypted disk in place. | IT Operations, checked by Forensics | Incident Manager | Build record and the persistence checklist. | Checklist complete on the rebuilt machine before it is allowed on the network. |
| 21:00 | Compare the isolated restore with the Academic Director’s sample of examination files. No production return on Day 0. | IT Operations | Business owner will be asked only after this check passes. Not on Day 0 unless the check is unexpectedly clean and counsel agrees. | Sample list and pass or fail. | Written statement that production was not changed. |
| 08:45 Day 1 | Monitoring checkpoint. Search again for `.locked` files, new admin logons, and the E04 destination. Schedule the post-incident review. Closure is not claimed at 24 hours. | Security Analyst | Incident Manager | Monitoring note. | Either a nil recurrence, or a new evidence ID and a return to containment. |

### Two difficult decisions

**Containment while keeping evidence.** Immediate isolation is necessary at 08:50 for FIN-PC07 and at 08:57 for write access and backup jobs. E06 is mass file change already under way, and E03 put an administrator account on BAK01. Waiting for a full memory image before pulling the network path would protect volatile evidence and would also give the encryption more of FS01 and possibly BAK01. I therefore isolate first and keep the machine powered so that a later image is still possible. I do not power the machine off unless encryption continues through a local process after the network is gone. If a hard power-off becomes necessary to save BAK01, the log will state the time, the reason, and the volatile evidence given up: memory contents and any live network sessions. That loss is accepted and written down. It is not hidden.

**Examination access while the backups are unsafe.** I will not restore the 02:00 network backup into LMS01 to catch the 11:00 start. E05 only says the job reported success. E03 shows svc_backup on BAK01 twenty-five minutes earlier. The backup is not immutable, and neither route has been restore-tested in 90 days. A fast restore could publish a ransom note through the portal or reintroduce the malware. The continuity alternative is to postpone the examination and, only if assumption A4 is confirmed, to use a clean paper copy under invigilation for a later sitting. The Academic Director owns that postponement. If the Director orders a reopen anyway, the plan requires a written acceptance of the risk, naming the untested backup, and the Incident Manager records the objection.

**Broader-compromise check.** Before any production return I want a nil or explained result for: other hosts with `.locked` files; Microsoft 365 risky sign-ins and mailbox forwarding rules on the finance account; new members of administrator groups; scheduled tasks and new services on FS01, LMS01, and BAK01; proxy traffic to the E04 destination from any host; and LMS01 application errors that began before 08:30, which would mean the portal was in scope earlier than E07. Server EDR is incomplete today, so a nil result on workstations is not a nil result on servers. That gap stays in the log.

### Evidence register and custody

Access to retained material is limited to Forensics and the Incident Manager. Nothing in this register has been collected yet. No hash is claimed before an image or export exists.

| ID | Source | Collection | Collector | Time | Integrity | Storage and custody |
| --- | --- | --- | --- | --- | --- | --- |
| EV-01 | FIN-PC07 disk | Planned forensic image after isolation. Not yet collected. | Forensics | Planned Day 0, WAT, after the 11:30 step | Hash to be taken when the image is made. No hash is claimed now. | Encrypted evidence drive in a locked cabinet. Custody transfer not yet started. |
| EV-02 | Microsoft 365 audit for the finance officer and svc_backup | Planned compliance export, Day −1 00:00 to Day 0 09:30. | Security Analyst | Planned Day 0 morning, WAT | Export hash recorded at creation. Not yet collected. | Same cabinet. Analyst to Incident Manager on creation. Restricted from the finance officer. |
| EV-03 | Proxy record behind E04 | Planned log export for FIN-PC07 around 01:50. | Security Analyst | Planned Day 0, WAT | Hash at export. Not yet collected. | Same cabinet. No broad IT access. |
| EV-04 | FS01 ransom note and a directory listing of `.locked` names | Planned photograph and a read-only listing before files are restored over. | IT Operations under Forensics direction | Planned immediately after share lock, WAT | Photograph file hashed at copy. Listing hashed at copy. | Same cabinet. Original note left in place until imaged. |
| EV-05 | Friday 17:00 backup media and the 02:00 job report | Planned read-only catalogue. The Monday job is retained but not restored to production. | IT Operations | Planned Day 0 15:00, WAT | Media identifier and catalogue checksum recorded at mount. | Friday media stays offline when not mounted. Mounts logged. |

### Eradication, credentials, and acceptance criteria

**Persistence and the likely entry point.** The likely entry is the Day −1 attachment on FIN-PC07 (E01), followed by use of the finance-officer account (E02) and then svc_backup (E03). That chain is a hypothesis until EV-01 and EV-02 exist. Eradication does not depend on waiting for the family name. FIN-PC07 is rebuilt from a known image rather than cleaned in place. After build, Forensics checks for new local accounts, scheduled tasks, services, and startup items, and the Security Analyst checks that the host’s EDR agent reports. The mail gateway rule for unexpected executable attachments is tightened the same day. I do not describe the attachment’s internals here. Identifying and removing it is the forensic retainer’s task, recorded in EV-01.

**Credentials.** The finance-officer password is reset and all sessions are revoked. MFA is re-enrolled with the officer present on a known device. svc_backup is disabled until a new backup account exists, with MFA and no interactive use on FIN-PC07. Global administrators are reviewed at 13:00. Mailbox forwarding rules are checked before the finance mailbox is used again. These are Respond actions under CSF 2.0 (NIST, 2025).

**Restoration order.** Identity first, after the resets above, because every later logon depends on it. BAK01 is then locked to the new backup account. The Friday 17:00 disconnected backup is the candidate data copy. FS01 file data for the examination is restored only inside the isolated network. LMS01 is pointed at that copy only after the file check passes. FIN-PC07 is last, because finance can work manually for a short time and the original disk is evidence. The Monday 02:00 backup is not in this order unless the isolated test shows it is clean and counsel and Forensics accept it.

**Recovery targets and acceptable loss.** The service target is not a ping. It is: a test learner account can open the approved paper and submit a file; an invigilator can retrieve it; a search of the restored share finds no ransom note and no `.locked` extension; EDR on the restored segment is quiet for the observation window. Acceptable data loss, if the Friday copy is the one that validates, is changes made after Friday 17:00. That is about 63 hours before 08:45 on Monday. Weekend edits are lost unless someone still has them in a mailbox outside the encrypted share. The Academic Director must accept that loss in writing. I will not promise a Monday-morning recovery point while E05 is untrusted.

**Selecting and validating the backup.** Selection rule: prefer the newest copy that was disconnected before E03 and that mounts read-only. The Friday 17:00 set meets the disconnection test. The 02:00 set does not. Validation is a restore into a network that cannot route to production or to BAK01. Checks are: the share opens; file counts are compared with the Friday catalogue; ten files named by the Academic Director open and match what the examinations office expects; an antimalware scan of the restored copy completes; the ransom note is absent. The business owner is then asked to accept or reject. Monitoring for recurrence runs for at least 24 hours after any production connection, looking for new `.locked` files and for svc_backup activity. Rollback is to disconnect the restored segment and keep the examination postponed. Production is not updated until that acceptance is signed.

---

## Task 4 — Communication and escalation

### Communication matrix

If the mobile channel fails, the fallback is voice calls on the printed numbers and a runner between the IT room and the examinations office. Messages are not posted on FS01.

| Stakeholder | Trigger | Message owner | Channel | Approver | Update |
| --- | --- | --- | --- | --- | --- |
| Learners | Examination change, or any instruction not to use the portal | Communications | SMS to the mobile number on the admission record, and tutors. Not corporate email alone. | Incident Manager and Academic Director | At decision, then by 15:00, then daily until the new sitting. |
| Director and management | Declaration, severity change, spend, or a notification decision | Incident Manager | Phone +234 809 000 2002. Follow-up note by SMS. | Director approves spend and any external statement. Incident Manager approves the facts. | First brief at 09:00. Next at 10:00. Then every two hours on Day 0. |
| IT and security staff | Declaration and each containment change | Incident Manager | Phone tree and the pre-agreed mobile channel | Incident Manager | On each playbook step, and at the 08:45 Day 1 checkpoint. |
| Microsoft 365 support | E02 unexplained, or a control we cannot set ourselves | Security Analyst | Admin-portal ticket. No personal data in the first ticket beyond account identifiers. | Incident Manager | When the ticket is opened, and when Microsoft replies. |
| Backup vendor | Need to mount Friday media read-only | IT Operations | Vendor support number, from the printed contact card | Incident Manager | At the 15:00 mount, and if the mount fails. |
| Legal and privacy advisers | Possible personal-data breach, law-enforcement question, or a claim of publication | Incident Manager | Phone +234 809 000 2004, then a fact pack from a clean device | Counsel owns the legal conclusion. Director is informed. | Within 30 minutes of declaration, then when a new fact changes the theft hypothesis. |
| Journalists or social media | Any inbound press or a public post about the exam or a leak | Communications | A single approved statement. No other staff reply. | Director, after Communications drafts and Legal reviews if data exposure is alleged | One holding line within 30 minutes, then only when the facts change. |

### Executive briefing

> At 08:45 WAT on Day 0 I declared a Critical suspected ransomware incident. EDR and the helpdesk report mass file changes on FIN-PC07. Shared folders on FS01 contain `.locked` files and a ransom note (E06). The portal login page loads, but learners cannot open materials or submit assessments (E07). An examination for 100 learners is due at 11:00. The contact list on FS01 is inaccessible (E08). Confidentiality impact is not confirmed. A finance officer opened an unexpected payroll attachment on Day minus 1 (E01). The account signed in from an unfamiliar IP and device at 01:15 (E02). FIN-PC07 uploaded 1.2 GB to unfamiliar cloud storage at 01:50; the content is unknown (E04). svc_backup logged on to FS01 and BAK01 off schedule at 01:35 (E03). The 02:00 backup reported success, but cleanliness is unproven (E05). Please approve three decisions: isolate FIN-PC07, the FS01 shares, and both suspect accounts now; postpone the 11:00 examination instead of reopening the portal on an unverified backup; and authorise forensic support and counsel. Corporate email will not be used for command until the sign-in is explained. The next update is at 10:00 WAT, or sooner if encryption spreads or counsel starts a statutory notification period.

### Learner notice

> Northbridge Skills Institute learner notice, issued at 09:15 WAT on Day 0. The online examination scheduled for 11:00 today will not go ahead at that time. Do not log in to the learning portal. It may show a login page, but materials and submission are not reliable. Do not open unexpected attachments, do not pay any message claiming to be from the Institute, and do not share passwords or one-time codes. Your place is held. A new date and time will be sent by SMS to the mobile number on your admission record and through your programme tutor. This delay protects the assessment. It is not a statement that your personal information has been published. If you have an agreed access arrangement, contact the examinations office on +254 123 000 2010. The next update will be by 15:00 WAT today.

### Escalation and notification rules

Operational notices and formal breach notifications are different products. The learner notice above is an operational notice. It tells people what to do about the examination. It does not tell a regulator that a personal-data breach has occurred, and it does not say that data were stolen. A formal breach notification is sent only after counsel has assessed the duty, named the recipient, and named the deadline.

Under assumption A1, counsel will apply section 40 of the Nigeria Data Protection Act 2023. A data controller notifies the Nigeria Data Protection Commission within 72 hours of becoming aware of a breach that is likely to result in a risk to the rights and freedoms of individuals, and communicates with the data subject where the breach is likely to result in a high risk (Nigeria Data Protection Act 2023, s. 40(2) and s. 40(3)). Counsel, not the Incident Manager, decides whether “becoming aware” has happened. On the present facts I will tell counsel that encryption of FS01 is known and that theft is not known. E04 is an unexplained transfer, not a demonstrated disclosure of learner records. If counsel finds that the threshold is not met, no 72-hour clock is started, and the log says so. If counsel finds that it is met, the clock starts at the time counsel records awareness, the Commission is the recipient, and data-subject communication is a further decision under section 40(3). I am not asserting that the clock has already started.

Escalation inside the institute: any new host with `.locked` files returns the incident to the Incident Manager immediately; any counsel opinion that the threshold is met goes to the Director the same hour; any journalist contact goes to Communications and is not answered by IT. Law-enforcement contact is a Director decision. Staff do not confirm or deny a leak in their own words.

---

## Task 5 — Tabletop exercise and improvement plan

This section is a simulation. I walked through it alone, speaking each role in turn. It is not a record of a live meeting and not a claim that these events have already happened.

### Design

**Objectives.** Test whether the team postpones the examination when management asks for an unverified reopen. Test whether a leak claim is treated as an allegation until a file is authenticated. Test whether a failed backup snapshot stops production restore. Test whether public messages stay inside the approved notice.

**Roles played.** Incident Manager, Security Analyst, Forensics, IT Operations, Academic Director, Legal, Communications, and a journalist played by Communications reading the inbound question aloud.

**Agenda, 60 minutes.** Minutes 0 to 5: objectives and the rule that only the communications role speaks publicly. Minutes 5 to 10: scene set at 09:20 with containment under way. Minutes 10, 20, 35, and 45: the four required injects. Minute 52: one added inject. Minutes 58 to 60: score the success criteria.

1. The postponement decision, with an owner, is written within five minutes of the minute-10 inject.
2. No simulated public statement confirms theft. Counsel is tasked within ten minutes of the minute-20 inject.
3. Within five minutes of the minute-35 inject, the failed snapshot is rejected for production and the examination stays postponed.

These three are measurable. A fourth, used in the debrief, is that the journalist receives only the holding line.

### Simulated decision log

| Inject | Decision | Owner | Rationale | Communication | Effect on the plan |
| --- | --- | --- | --- | --- | --- |
| Min 10. Management asks to reopen the portal before 11:00 on the unverified backup. | Refuse production restore. Postpone the examination. Offer a later sitting only after an isolated test, or sooner only if a clean invigilator copy is confirmed. | Academic Director, on the Incident Manager’s recommendation. The Director may not use IT Operations to bypass this. | E05 is a success flag, not a clean restore. E03 and the lack of a 90-day test make a reopen likely to miss the exam and contaminate LMS01. | Director told by phone that the exam is postponed. Learner SMS stands. No portal banner that says the exam is on. | The plan’s 09:15 decision is confirmed. A reopen becomes a plan breach unless a tested copy is accepted in writing. |
| Min 20. Attacker claims all 1,800 learner records and threatens publication in 12 hours. Screenshot authenticity is unknown. | Do not treat the claim as proof. Do not pay. Preserve the screenshot as a new planned exhibit. Ask counsel whether the claim changes the section 40 assessment. | Legal for the notification judgement. Incident Manager for preservation. | A screenshot can be fabricated. E04 still does not name the files. Paying or confirming theft would make an unverified claim into a public fact. | Communications issues no leak statement. Learners are not told that their data are published. Director is informed that counsel is assessing. | The fact pack to counsel gains the claim and the screenshot. The 72-hour question is reopened. It is not answered by the attacker’s clock. |
| Min 35. Test restore of the latest network backup fails with an unreadable snapshot. | Abandon the Monday network backup as a recovery source. Keep the Friday disconnected media as the only candidate. Do not retry the bad snapshot against production. | IT Operations, accountable to the Incident Manager. | An unreadable snapshot cannot meet the acceptance checks. Retrying it in production risks the only remaining service path. | Academic Director told that a same-day portal recovery is no longer the plan. Learners’ 15:00 update will say the exam remains postponed, without technical detail. | Recovery point is Friday 17:00 if that media mounts. Acceptable loss stays at about 63 hours. E05 is closed as unusable. |
| Min 45. A social-media post says the examination is cancelled. A journalist asks whether learner information has been exposed. | Correct the word “cancelled” to “postponed, place held,” using the approved notice. Do not answer the exposure question with yes or no. | Communications. Legal reviews the exposure sentence. Director approves the holding line. | “Cancelled” is stronger than the decision and causes unnecessary harm. The exposure answer is unknown, and saying “no” would repeat the healthcare case’s weakest claim. | One holding line: the exam is postponed, places are held, the next update is 15:00, and questions about personal data are not answered until they are known. | The communication matrix gains a 30-minute rule for public corrections. Staff are reminded not to reply on the post. |
| Min 52, added inject. LAB-PC04 in a teaching lab starts writing `.locked` files. | Isolate the lab VLAN. Expand confirmed scope beyond FIN-PC07 and FS01. Keep rooms closed. Start an EDR hunt of every lab. | Incident Manager. IT Operations performs the isolation. | A second site of encryption means containment at 08:50 was necessary but not sufficient. The examination rooms are inside the new scope. | Director and Academic Director called. No learner message that names the lab malware. | Severity stays Critical. The hunt list in the 09:08 action is widened. Server EDR gap is recorded again, because LAB-PC04 was seen by workstation EDR only. |

### Results against the success criteria

Criterion 1 was met in the simulation: the postponement was restated at minute 10 with the Academic Director as owner. Criterion 2 was met: the screenshot was preserved and counsel was tasked, and no public line confirmed theft. Criterion 3 was met: the unreadable snapshot was dropped within the inject, and the Friday media became the only candidate. The journalist received only the holding line. The simulation also showed a gap the criteria did not originally include: the plan’s first scope hunt was too narrow, because LAB-PC04 was outside FIN-PC07 and FS01. That gap is improvement 1 below.

### Improvements and review

| Priority | Improvement | Owner | Target | Evidence of completion |
| --- | --- | --- | --- | --- |
| 1 — High | Rewrite the first-hour hunt so every teaching lab is in the initial scope, not only FIN-PC07 and FS01. | Security Analyst | 10 days from this exercise | Updated playbook page and a query that lists lab hosts, stored off FS01. |
| 2 — High | Test the Friday disconnected backup by restoring a sample share in isolation. | IT Operations | 30 days | Restore record with date, duration, file-open result, and the Academic Director’s sample check. |
| 3 — High | Move the incident contact list off FS01 and complete one call-down. | Incident Manager | 14 days | Call-down sheet showing each role reached by phone. |
| 4 — Medium | Pre-approve the postponement wording and the holding line for journalists, so minute 45 does not wait on drafting. | Communications | 14 days | Two signed templates in the registrar’s pack and in the Director’s pack. |
| 5 — Medium | Put EDR on FS01, LMS01, and BAK01. | Security Analyst | 60 days | Coverage report at 100 percent of those servers and one test alert. |
| 6 — Medium | Separate the backup administrator from svc_backup’s current shared use, with MFA. | IT Operations | 30 days | Access review showing the old account disabled and a failed interactive logon test. |

The post-incident review is due within ten working days of closure, and in any case within two weeks. The Incident Manager writes the review, chairs the meeting, and closes each action only when the evidence is attached. The Director accepts any action that will not be done, in writing, with the residual risk named. Legal closes the notification question separately, so a closed incident review cannot accidentally close an open statutory duty.

---

## Task 6 — Case study analysis and critical reflection

Slide 16 describes ransomware at a healthcare organisation of about 2,500 employees. A phishing attachment led to Ryuk, which moved laterally for about 72 hours before encryption. On day 4, endpoint detection alerted. The team isolated segments and verified that offline backups were intact. Critical systems returned within 72 hours, and full restoration took seven days. No ransom was paid. Patient-data theft was not confirmed. Notifications were completed, and a lessons-learned review was held on day 14 (ICDFA, Week 20, slide 16).

Three strengths stand out. Recovery did not depend on payment, because an offline backup had been checked before use. Isolation started when encryption was detected, which limited spread after a long dwell time. An existing team and playbook meant the day-14 review could update the plan and the training (CISA, n.d.).

Two limitations remain. Detection missed about 72 hours of lateral movement, so a strong recovery followed a failed detect stage. The slide also says exfiltration was not confirmed. That is not proof that patient data stayed inside the network, because no egress log or forensic result is given.

Northbridge should take two lessons from the case. First, restore only a copy that has been opened on a network separated from production. The Friday backup is disconnected, but it has not been restore-tested in 90 days, and the Monday 02:00 job followed the svc_backup logons and is not immutable (E03, E05). Second, separate the critical-service clock from the full-estate clock. Restoring critical systems within 72 hours is not the same result as completing restoration in seven days. The first protects the service that cannot wait. The second includes remaining systems, hardening, and a check that the attacker is gone.

A claim that no patient data was stolen would need proxy records, a check for new mailbox rules and bulk downloads, and a forensic finding that no staging archive existed. Silence in a ransom note is not that evidence. E04 does not provide it for Northbridge either.

After the tabletop I revised one decision. Management pressure to reopen the portal before 11:00, and the unreadable snapshot at minute 35, showed that an untested restore can fail the examination. The rule is now to postpone once FS01 shows encryption, and to reopen only after the business owner accepts a tested copy.

---

## References

Cybersecurity and Infrastructure Security Agency. (n.d.). #StopRansomware guide. https://www.cisa.gov/stopransomware/ransomware-guide

International Cybersecurity and Digital Forensics Academy. (Week 20). Incident response planning [PowerPoint slides]. GRC105, slides 1–18.

International Cybersecurity and Digital Forensics Academy. (Week 21). Business continuity and disaster recovery [PowerPoint slides]. GRC205, slide 14.

National Institute of Standards and Technology. (2025). Incident response recommendations and considerations for cybersecurity risk management: A CSF 2.0 community profile (NIST Special Publication 800-61 Revision 3). https://doi.org/10.6028/NIST.SP.800-61r3

Nigeria Data Protection Act, 2023, s. 40. https://www.ndpc.gov.ng/ndp-act-2023/
