# Security Awareness Program for Northbridge Logistics

**GRC 206 · Designing a Security Awareness Program**  
International Cybersecurity and Digital Forensics Academy

Fictional training scenario. Northbridge Logistics is not a real organisation.

| | |
| --- | --- |
| Student | Edna Moraa Maburi |
| Student ID | 2025/GCR/12688 |
| Official email | [gcr2512688@students.icdfa.ed.ng](mailto:gcr2512688@students.icdfa.ed.ng) |
| Programme | Governance, Compliance, and Risk Engineering |
| Course | GRC 206 – Designing a Security Awareness Program |

## Contents

1. [Scenario facts and assumptions](#scenario-facts-and-assumptions)
2. [Assess](#task-1--assess)
3. [Plan](#task-2--plan)
4. [Deliver](#task-3--deliver)
5. [Measure](#task-4--measure)
6. [Improve and sustain](#task-5--improve-and-sustain)
7. [12-month program calendar](#12-month-program-calendar)
8. [Reflection](#reflection)
9. [References](#references)

---

## Scenario facts and assumptions

Northbridge Logistics has 800 staff across head office, warehouses and remote sales. The brief says recent invoice-fraud attempts have raised concern, two staff reused passwords after a breach and the company handles regulated customer data. A small security team and a modest training budget must keep the program going and leadership expects fewer incidents within 12 months. The design has to work on the warehouse floor as well as in the office and on the road. Those are the facts. The brief does not give a loss figure, a click rate, a headcount by role or a team chart.

Everything else below is a planning assumption, labelled as such. Email and sign-in are on Microsoft 365 and Entra ID. Head-office staff and remote sales have their own mailboxes. Most warehouse staff do not: they use shared floor terminals and get messages through the shift supervisor. Finance can change a supplier's bank details in its system, and a recorded callback to a number already on file is not yet something the company can evidence. Security is one Security Manager plus a part-time analyst. The IT service desk is separate. There is no trainer and no developer team of any size. Privileged IT accounts are handled with finance under the credential objective.

"Regulated customer data" is treated as personal data of customers and recipients. The brief does not name the law, so the behaviours stay practical rather than legalistic. Headcount splits are workload guesses only: about 450 warehouse and operations, 120 remote sales, 40 finance, 40 executives and people managers, and the rest in head office. Metrics will use HR's real lists, not these guesses. The awareness budget is a planning figure of USD 12,000, for a simulation tool if the current licence does not include one, for warehouse print, and for small recognition vouchers. Manager time is the real cost. Every target in this paper is a proposal, not a result Northbridge has already achieved. Month 1 is the month after approval.

The structure follows the course lifecycle: assess, plan, deliver, measure, improve. NIST SP 800-50r1 describes the same idea as a learning program aimed at behaviour, with measures and regular revision (Merritt et al., 2024). ISO/IEC 27001:2022 control 6.3 asks for awareness and training that fit the job and that are kept up to date.

## Task 1 — Assess

### Three human-driven risks

**Risk 1 — paying the wrong account after an invoice lure.** The unsafe action is changing a supplier's bank details, or releasing a payment, because an email looked familiar or urgent. The brief's invoice-fraud attempts make this the priority, not a general phishing topic. One success can cost more than the training budget. Finance is the main audience. Remote sales are a second path, because they speak to customers and suppliers and may pass on a "please use this new account" message. Warehouse staff are a thinner path, through fake reroute or carrier messages. They are included so that path is not ignored.

**Risk 2 — account takeover from reused passwords.** The brief states that two staff reused passwords after a breach. The unsafe action is keeping a password that has already leaked, skipping a second factor, or leaving a shared warehouse password in the open. A taken mailbox is how an invoice lure becomes believable, and a taken account can expose customer data. The two known cases show the behaviour has already happened. In Month 1 those accounts are reset, checked for misuse, and the people are coached, with MFA turned on. Reuse is not treated as misconduct.

**Risk 3 — customer data leaving the work channel.** The unsafe action is putting delivery details into personal email or a chat app, leaving paperwork in the open, or working on customer information from an unlocked device in a public place. The data moves through office, road, and warehouse, and a small security team cannot watch every phone. The control is a habit, plus a safe way to say "I sent that to the wrong place" before it spreads.

The three risks connect. A reused password makes invoice fraud easier, and both can end with customer data exposed. They are still taught as separate actions, because the safer behaviour is different each time: call back before you pay, use MFA and a unique password, and keep customer data in work systems.

### Audiences and what each one needs

- **Finance and accounts payable.** They can change who gets paid, they are under pressure at month-end, and they have their own mailboxes. A video about phishing will not change a payment run. They need a short drill on the callback, and a rule that an urgent email from a senior person does not replace it. This is Risk 1.
- **Remote sales.** They work between visits, often on a phone, they hold customer data away from the office, and they can be pulled into a bank-detail conversation. They will not attend a classroom session. Modules stay under ten minutes, and their manager repeats one point on the monthly call. Risks 1, 2, and 3.
- **Warehouse and operations, including night and weekend shifts.** This is the largest group and the easiest to miss. Shared terminals, noise, and rotating supervisors make e-learning a poor fit. They need a ten-minute talk at the start of the shift, a poster at clock-in, and a way to report without a mailbox. Topics are shared passwords, unverified reroute or shift-change messages, and customer paperwork. Risks 2 and 3, and the warehouse route into Risk 1.
- **Executives and people managers.** They need less content and more modelling. They are targets for rushed payment requests, and their reaction decides whether anyone reports a mistake. They need a short briefing: do not pay from an email, thank the reporter, and do not ask who clicked in front of the team. They also protect the ten minutes a shift needs. Risk 1, and the culture Objective 3 depends on.
- **Other head-office staff, including the IT service desk.** They still need the report button, password hygiene, and the rule on customer data. The service desk has an extra need: if a reset or a reported click is met with irritation, the no-blame route is only a poster. Risks 2 and 3.

### Baseline to capture before teaching starts

Four pictures are taken in Month 1, before the teaching campaigns. Each has a source.

- **Email behaviour.** Email users receive one low-difficulty simulation. Click rate and report rate are stored from the simulation tool. Staff with no mailbox are not in this number. Including them would invent a result.
- **Credentials.** IT exports the share of individual accounts with MFA and checks finance, executive, and admin passwords against a breach list, including the two known reuse cases. The source is Entra ID.
- **Payee changes.** The Finance Manager samples supplier bank-detail changes from the past 90 days and counts how many have a recorded callback to a number already on file. The source is the finance system. If nothing was logged, the baseline is written as "no reliable evidence". That is a finding, not a zero.
- **Harm and the reporting route.** The Security Manager counts confirmed invoice-fraud losses, account takeovers, and customer-data mishandling events over the past 12 months from the incident log. Supervisors and a short head-office form ask about 30 people, across shifts, whether they can name how to report a suspicious message.

## Task 2 — Plan

### Three objectives

The targets below are planning choices. They are not current Northbridge results.

**Objective 1 — payments, by the end of Month 6.** Every supplier bank-detail change in the month has a recorded callback to a known number before payment. The same result holds in each month of Quarter 3. The Finance Manager owns the control. The Security Manager owns the teaching. Evidence is the finance checklist. This is the safer action for Risk 1.

**Objective 2 — credentials, by the end of Month 4 and Month 9.** By the end of Month 4, MFA is enforced on at least 98% of individual accounts. The rest must be named exceptions, such as a break-glass account. By the end of Month 9, fewer than 8% of email users click a credential-harvest simulation, set against the Month 1 baseline, and no finance or admin password appears in a current breach list. IT owns MFA. The Security Manager owns the simulation figure. This is the safer action for Risk 2. Shared warehouse passwords sit outside the 98% and are a yes/no check by the Operations Manager each quarter.

**Objective 3 — reporting and harm, by the end of Month 12.** At least 40% of simulation emails are reported by the recipient and confirmed incidents that caused a payment loss or exposed customer data are at least 30% below the Month 1 twelve-month count. The Security Manager owns the figures. The COO is accountable for the outcome. A rise in reports counts as success. Only confirmed loss and confirmed data exposure count toward the 30%, so the program is not punished for people speaking up. This is how the request to reduce incidents within 12 months becomes something a third person can judge.

### Sponsor and owners

The sponsor is the Chief Operating Officer. The risks cross finance, warehouses, and sales, and only someone at that level can require shift time and stand behind a no-blame rule when a real loss is embarrassing. The Finance Director co-sponsors the payment control, because a successful fraud would sit in finance.

The Security Manager owns the program: baseline, materials, simulations, the quarterly pack, and the decision log. The Finance Manager owns the callback procedure and the payee measure. The Warehouse Operations Manager owns delivery of the shift talk on every shift, from a script security writes once. A sales manager owns the two-minute repeat with remote sales. HR owns the week-one slot, the joiner list, and the policy sentence that reporting a mistake is not a disciplinary matter. The IT Service Desk Lead owns MFA, the report button, and the tone of coaching.

### Topics for each audience

Finance learns the clues in an invoice or bank-change email, the callback steps, and the limit on an urgent message from a senior person. That is Risk 1, plus practice at reporting. Remote sales learn bank-detail requests, MFA and password reuse, and keeping customer details out of personal chat and personal email. That is Risks 1, 2, and 3. Warehouse staff learn not to leave a shared password in the open, not to act on a reroute or shift-change message until a supervisor confirms it, where paperwork stays, and how to report with no mailbox. Leaders learn what to do with a rushed payment request, how to thank a report, and why names of people who clicked are not published. Other office staff and the service desk learn the report button, password reuse, and customer data in work systems only. The desk also learns the coaching script, so a click never becomes a telling-off.

## Task 3 — Deliver

### Formats, and why they fit

Finance gets one 30-minute live drill in Month 2. They work a fake bank-change email and complete the callback. In Months 6, 9, and 12 a five-minute scenario is added to the month-end meeting they already hold. A separate course would be dropped at month-end. The drill is the action Objective 1 measures.

Remote sales get two modules of about eight minutes, in Month 3 and Month 7, built for a phone. Their manager spends two minutes on the same point in the monthly sales call (Months 5, 10, and 11). They will not come in for a workshop.

Warehouse staff get a ten-minute toolbox talk in Months 3, 6, 8, and 11, led by the shift supervisor from a one-page script, on every shift including nights. Month 5 is only a catch-up for any shift that missed Month 3. A poster stays at clock-in and at goods-in. There is no e-learning assignment for this group. Security writes the script once. Supervisors deliver it. That is how several hundred people are reached without a training department.

Leaders get 30 minutes in Month 3 and 20 minutes in Month 9. They will not work through a module. They need a script for their own behaviour and a decision on shift time. Other office staff get one 10-minute module in Month 3 and a short customer-data reminder in Month 7. The service desk gets a 20-minute huddle in Month 2, before teaching simulations start, so the first person who reports is handled well.

### Onboarding

From Month 2, HR places a 15-minute induction in week one, before access to customer data or the finance system. In that same week IT enrolls MFA, or the account stays limited. Within 30 days the joiner does the activity for their audience: the finance material, the sales module, the next toolbox talk or the office module. The manager confirms it. The Security Manager does not teach each arrival. Turnover is otherwise how a small program quietly stops.

### Phishing simulations and supportive follow-up

The report button is switched on, and people have been shown it, before any teaching simulation. Month 1 is a baseline only. The same day, each recipient gets a short note: this was a test and this is how to report. It is not a score against anyone.

After that, the Security Manager runs a campaign about every other month: Months 3, 4, 6, 8, and 11, six touches in the year including the baseline. Themes follow the risks: an invoice bank-change, a password reset, an executive demanding a payment, and a customer or delivery lure for sales. Difficulty stays modest until the report rate is moving. A harder template in a quiet month would look like failure when it is only a harder test. Staff with no mailbox are not loaded into the tool. Their practice is a verbal case in the toolbox talk.

A click opens a short explanation and the report step. Names are not circulated. After two clicks in the year, the manager or the service desk spends ten minutes on the clue that was missed and on how to report next time. There is no disciplinary consequence for a click or for an honest mistake. Deliberate misuse stays an HR matter and sits outside this program. The COO sees trends, not a list of names.

### A no-blame way to report

Email users use the report button in Outlook, watched by the service desk and the Security Manager. Warehouse staff, and anyone without email, tell the shift supervisor or scan the QR code at clock-in. The form asks what they saw, when, and which site. A supervisor phone extension is the backup if the code fails.

The reporter is thanked the same day. When the outcome is known, they get one line: it was malicious and it was blocked, or it was genuine and no action was needed. The same route covers mistakes, including a customer list sent to the wrong place or a click on a real message. Supervisors use one sentence: "Thank you for telling us. You are not in trouble for reporting." They do not ask the person to explain it in front of the shift. The Security Manager records the report, works with IT or Finance to contain it, and removes names before anything goes to leadership.

## Task 4 — Measure

Five measures cover reach, behaviour and outcomes. Each one is read against the Month 1 baseline. A change is only meaningful if we say what it does and does not, prove.

**1. Reach — role learning completed.** Calculation: people who finished the in-role activity, divided by people in that audience that quarter. For joiners: week-one inductions completed, divided by joiners that month. Source: attendance sheets, or the learning record for the short modules; HR supplies joiner numbers. Owner: HR for onboarding, the Security Manager for the roll-up. Review: monthly for joiners, quarterly otherwise. A high figure means people were offered the learning. It does not mean they work more safely. A low figure on one shift means the talk did not happen, and behaviour numbers for that site should not be over-read.

**2. Behaviour — click rate and report rate.** Calculation: users who clicked, divided by users who received the email, and users who reported, divided by users who received it. The two rates are never combined. Source: the simulation tool. Owner: Security Manager. Review: after each campaign, and at the quarterly meeting. A lower click rate suggests more people paused, but only if the template was not simply easier than last time. A higher report rate is the safer action, because early reports cut the loss. These rates are email users only. They are not a warehouse result.

**3. Behaviour — verified bank-detail changes.** Calculation: supplier bank-detail changes with a callback log to a known number, divided by all such changes that month. Source: the finance checklist on the payment record. Owner: Finance Manager, with the figure passed into the quarterly pack. Review: monthly in finance, quarterly with the COO. This is whether Objective 1 is happening. A dip usually means someone paid from an email under time pressure, and the next five-minute scenario should use that exact case. The measure says nothing about the warehouse.

**4. Behaviour — MFA on individual accounts.** Calculation: individual accounts with MFA enforced, divided by individual accounts, excluding documented break-glass accounts. Source: Entra ID. Owner: IT Service Desk Lead. Review: monthly until Month 4, then quarterly. A rise means a stolen password is less likely to open a mailbox. It does not prove people stopped reusing passwords, which is why finance and admin passwords are checked against breach data at Month 1 and Month 9. Shared warehouse logins are the separate quarterly yes/no from Operations.

**5. Outcome — confirmed harmful incidents.** Calculation: the count, and the estimated money lost, of incidents in the quarter where someone was persuaded into a payment, an account was taken over, or regulated customer data left the company. At Month 12 the four-quarter total is set against the twelve-month baseline. Median time from the message to the first report is tracked beside it. Source: the incident log, with Finance confirming any payment loss. Owner: Security Manager. Review: quarterly. Fewer losses and faster reports mean less harm. More reports with no loss mean people are speaking up, and they are not presented to the COO as a worse year. Counts will be small, so one fraud would dominate the story. The note separates the number of events from the value.

### What leadership receives

Each quarter the Security Manager gives the COO a short pack and walks through it in the review. The pack shows the five measures against the baseline, what that means for money and for customer data, and the limits: email rates leave out the warehouse, a higher report count is not more harm, and a harder simulation is not automatically a failure. It ends with the decisions asked of the COO. Typical decisions are making the callback mandatory, enforcing MFA, protecting ten minutes on each shift and whether shared warehouse logins need a funded change or a code held by the supervisor. Decisions are written in the log that week. An item with no owner is not treated as agreed.

## Task 5 — Improve and sustain

### Quarterly review

The review is at the end of Months 3, 6, 9, and 12, for 45 minutes. The COO, Security Manager, Finance Manager, Warehouse Operations Manager, a sales manager, the HR Business Partner, and the IT Service Desk Lead attend. They look at the five measures against the baseline, incidents since the last meeting, which shifts missed a talk, open actions, and what learners said. The Security Manager writes a decision log: the decision, the owner, the date, and which measure will show whether it worked. The log is kept with the incident records.

Content is updated between reviews, not only at them, when a real invoice-fraud attempt or a new scam does not match the materials, when a process changes (a new finance system, a new carrier, or a new customer form), when the same lure works twice, or when people say the advice cannot be done in the job. A callback number that was never on file is a process fix, not a new poster.

### Listening to different roles and shifts

Every toolbox talk, finance drill, leader briefing, and short module ends with three questions: Did this match your work? What got in the way of the safe action? What should we stop? Warehouse answers go on the script sheet, and the night shift is asked separately, so the day shift does not speak for the site. Sales are asked the same questions for two minutes on the monthly call. Office staff get a three-line form. Answers can be unsigned. The Security Manager reads them before each quarterly review, changes one concrete thing, and says at the next talk what changed. Feedback that never changes the program will stop arriving.

### Two ways to recognize safer behaviour

First, team acknowledgement. When a shift, a sales team, or finance reports a message that is confirmed malicious, the COO names the team in a short note that month. The note does not rank individuals, and it repeats that a click or an honest mistake is not misconduct.

Second, a manager "good catch" within a week of a useful report, plus entry into a quarterly voucher draw. The draw is among people who reported. It is never a prize for people who simply did not click. People who click a simulation are not named in either method. Both methods reward speaking up early, which is the action Objective 3 is trying to grow.

### How the year stays tied to the risks

Risk 1 runs from finance and sales, through Objective 1, the callback drill, and the invoice simulations, to the payee-change measure, and into the quarterly log if that measure slips. Risk 2 runs from individual account holders, with extra weight on finance and IT, through Objective 2, MFA, and the credential simulations, to the MFA rate and the click rate. Risk 3 runs from warehouse, sales, and office staff, through the talks and the no-blame route, to the report rate, shift coverage, and the harm count. The calendar uses the same owners. The security team writes and measures. Managers deliver. That is a load a small team can still be doing in Month 12.

## 12-month program calendar

One proposed year for Northbridge Logistics. Activities, audiences and owners match the report. Targets are planning choices, not current results. The Security Manager prepares scripts and runs simulations. Managers deliver the talks.

| Month | Activities and audience | Owner | What is checked |
| --- | --- | --- | --- |
| 1 | Baseline simulation for email users. MFA and breach-password extract, including the two known reuse cases (reset, misuse check, coaching). 90-day payee sample. 12-month incident count. Reporting-route pulse across shifts. Report button and QR switched on. COO kickoff. | Security, Finance, IT, COO | All four baselines filed, including "no evidence" if payee changes were not logged. |
| 2 | Reporting route explained: email, poster, supervisor line. Finance 30-minute callback drill. Service-desk coaching huddle. Week-one induction and MFA gate start for every new joiner. | Security, Finance, HR, IT | Drill attendance. Induction completions. Button and QR tested. |
| 3 | Warehouse talk 1, all shifts: risks and how to report. Sales module 1 (phone). Office 10-minute module. Leader briefing (30 min). Simulation: fake invoice. Quarter 1 review. | Operations, Sales, Security, COO | Talk coverage by shift. Click and report rates. Q1 decisions logged. |
| 4 | MFA enforced on individual accounts. Proposed target: 98%, with named exceptions. Simulation: fake password reset. Ten-minute coaching for anyone with two clicks. | IT, Security, managers | MFA rate. Coaching done with no disciplinary note. |
| 5 | Catch-up only for shifts that missed Month 3. Two-minute prompt on the sales call. A new poster only if a live scam appeared. No new module. | Operations, Sales | Missed shifts now covered. |
| 6 | Finance five-minute scenario in the month-end meeting. Warehouse talk 2, all shifts. Simulation: urgent payment from an executive. Quarter 2 review. Payee-change target is due. | Finance, Operations, Security, COO | Payee-change rate against the proposed 100%. Shift coverage. Q2 log. |
| 7 | Materials updated from Quarter 2 feedback and any live lure. Sales module 2 on customer data. Short office reminder on the same topic. | Security, Sales | One feedback item from Q2 closed and told back to staff. |
| 8 | Warehouse talk 3, all shifts: paperwork, shared logins, reroute messages. Simulation: credential harvest. | Operations, Security | Shift coverage. Shared-password yes/no. Click and report rates. |
| 9 | Leader refresh (20 min). Finance month-end scenario. Recheck finance and admin passwords against breach data. Quarter 3 review. Credential target is due. | IT, Finance, Security, COO | Click rate against the proposed 8%. Breach recheck. Q3 log. |
| 10 | Two-minute sales prompt, using any real lure from the quarter. Coaching for repeat clickers. Lighter month for the security team. | Sales, IT, managers | Monthly payee-change rate continues. Coaching log. |
| 11 | Warehouse talk 4: what changed this year, all shifts. Harder mixed simulation. Manager good-catch notes and the voucher draw among reporters. Same five-minute close on the sales call. | Operations, Security, managers, COO | Report rate. Recognition went to reporters, not to a "did not click" list. |
| 12 | Finance month-end scenario. Year set against the Month 1 baseline. Next-year calendar drafted from the decision log. Quarter 4 review with the COO. | Security, Finance, COO | Harm count and value versus baseline, with the detection limit stated in the pack. |

Runs every month, so it is not repeated in the grid. From Month 2, every new joiner does the 15-minute induction and MFA in week one, then the audience activity within 30 days (HR). The Finance Manager checks the payee-change rate every month from Month 2. IT checks MFA monthly until Month 4, then quarterly. The three learner questions are asked at every talk, drill and module. Quarter reviews are Months 3, 6, 9 and 12.

## Reflection

The hardest part was measuring warehouse staff without pretending email statistics speak for them. Click rates are easy to export and they would make the program look stronger than it is, because most of the 800 people do not work from a mailbox. Simulation results in this design are labelled as email-user results. Warehouse evidence is shift attendance, the QR reports and a quarterly yes or no on shared passwords.

The trade-off was capacity. A small security team and a budget of this size cannot teach everyone in person each month. Managers and shift supervisors deliver from scripts the security team writes once. That is what makes twelve months realistic. It is also the weak point: a busy night shift can skip the talk. The quarterly review therefore checks which shifts were missed, not only the percentages. I would rather repair a missed talk than add modules that nobody has time to run.

## References

GRC206 self-study deck. *Designing a Successful Security Awareness Program.* The five stages, and the split between reach, behaviour, and outcomes, follow that deck.

International Organization for Standardization. (2022). *ISO/IEC 27001:2022. Information security, cybersecurity and privacy protection — Information security management systems — Requirements.* Annex A, control 6.3, Information security awareness, education and training.

Merritt, M., Hansche, S., Ellis, B., Sanchez-Cherry, K., Nethery Snyder, J., & Walden, D. (2024). *Building a cybersecurity and privacy learning program* (NIST SP 800-50r1). National Institute of Standards and Technology. [https://doi.org/10.6028/NIST.SP.800-50r1](https://doi.org/10.6028/NIST.SP.800-50r1)
