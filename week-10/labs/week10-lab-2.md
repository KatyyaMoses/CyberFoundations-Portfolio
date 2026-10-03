# Week 10 — Lab 2: Prioritize Risks and Recommend Controls

Learner: Katyya Moses
Case: Cloud Heights Family Clinic — Risk & Threat Investigation
Scenario date: Friday 13 March 2026, 09:00 (clinic local time)
Report generated: 2026-10-03T11:19:13.955Z
Study mode: Guided (hints available)

Completion checklist: all required work for Lab 2 is present.

## Risk ratings

| ID | Asset | Likelihood | Why | Impact | Why | Score (L x I) | Classroom band |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SC-01 | Appointment scheduling account | 3 | Fake emails like this reach the reception mailbox routinely \(the third in three weeks\), staff just delete them, and nothing is reported. The account uses only a password with no second step, so nothing would reliably stop a staff member who is fooled. | 2 | An attacker could view, change or cancel appointments for the whole clinic. Part of the clinic's day would be disrupted, but patient records are in a separate system and the account can be recovered. | 6 | High (6–9) |
| SC-02 | Patient records application | 3 | All four reception staff share the "frontdesk" login. The password is written on a card in the desk drawer, was last changed 8 months ago, and is not changed when staff leave \(two have left in the past year\). Nothing in the evidence would stop someone who knows it. | 3 | The application holds clinical information about patients, so exposure would be a serious privacy breach. The log only shows "frontdesk", so the clinic could not tell who opened or changed a record, which makes investigation and recovery slow and uncertain. | 9 | High (6–9) |
| SC-03 | Staff laptops | 2 | There is real exposure: a laptop was found signed in and unattended in a room that opens onto the patient corridor, and 2 of 9 laptops have had no security updates for 90 days. But 7 of 9 are up to date, and no alert, malware or incident has been reported. | 2 | A single laptop gives access to email and the records application, so limited sensitive data is involved. Recovery would take real effort, but the damage would be contained to one or two devices unless malware spread. | 4 | Medium (3–4) |
| SC-04 | Public information website | 3 | The certificate expires on 20 March 2026, 7 days after the scenario date. Renewal is manual with no reminder and no named owner, and the last two renewals happened only after visitors saw warnings. | 1 | The site is only public information \(address, hours, services\) with no sign\-in, no patient portal and no link to the records application. Visitors would see a browser warning, which is a nuisance, and renewing the certificate fixes it quickly. | 3 | Medium (3–4) |
| SC-05 | Backup archive | 2 | The only backup is one USB drive permanently attached to the reception workstation, so anything that damages that computer can damage the drive too. The last 6 jobs finished with errors. But no ransomware or failure has actually been reported, so I rate it medium. | 3 | The clinic would lose at least 14 days of records, possibly all of them. There is no offsite or cloud copy, and no restore has ever been tested, so recovery would be slow and uncertain. | 6 | High (6–9) |

Bands (1–2 low, 3–4 medium, 6–9 high) are a classroom teaching aid, not a compliance standard.

## Priority risks
- SC-02 — Patient records application
- SC-01 — Appointment scheduling account

**Why these:** Risk 2 has the highest score \(9\) and Risk 1 is next at 6. Both come from the same weakness: shared logins with weak password handling. Patient records hold the most sensitive data, and the scheduling account is open to a phishing email that arrives every few weeks. Fixing how people sign in reduces both risks at once. Backups also scored 6, but the shared logins are easier to fix quickly and are being hit more often, so I put them first.

## Recommended controls
### SC-02
- **Control:** Give each staff member their own named login for the records application, remove the shared "frontdesk" account, and disable an account on the day someone leaves.
- **How it helps:** It closes the shared-password weakness. The old password on the desk-drawer card stops working, leavers lose access automatically, and the log shows which person opened each record.
- **Risk remaining afterwards:** A staff member's own password could still be stolen or guessed, and staff could still open records they shouldn't. The clinic would now at least be able to see who did it. Whether former staff could reach the system from outside is still unknown.

### SC-01
- **Control:** Turn on a second sign-in step \(such as a code from a phone app\) for the scheduling account, and give each reception person their own login. Also make reporting suspicious emails to the IT contractor a standing rule.
- **How it helps:** A stolen password alone would no longer let an attacker in, so the fake "Clinic IT Support" email loses most of its power. Reporting means the IT contractor hears about each phishing attempt instead of staff deleting them.
- **Risk remaining afterwards:** Staff could still be fooled by a phishing email and approve a sign-in or enter a code, and a few messages might not get reported. The chance of takeover drops a lot, but not to zero.

## Owner briefing
Word count: 121 (guide: 100–150)

I looked at how the clinic keeps its systems safe and found two big problems. First, all four front desk staff use the same login for patient records, and the password is on a card in a drawer. When someone leaves, the password doesn't change, so we can't tell who opened a record. Second, the appointment account only needs a password. Three fake "IT Support" emails showed up in three weeks, and nobody reported them. I recommend giving everyone their own login and adding a second step when signing in to the appointment account. This won't fix everything, and I don't have proof that anyone has been attacked yet. But it would make problems harder to cause and easier to trace.
