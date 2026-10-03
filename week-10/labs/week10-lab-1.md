# Week 10 — Lab 1: Investigate What Needs Protection

Learner: Katyya Moses
Case: Cloud Heights Family Clinic — Risk & Threat Investigation
Scenario date: Friday 13 March 2026, 09:00 (clinic local time)
Report generated: 2026-10-03T11:08:11.390Z
Study mode: Guided (hints available)

Completion checklist: all required work for Lab 1 is present.

## Evidence added to my findings
- EV-BAK-01 — Backup job history
- EV-BAK-02 — Restore testing statement
- EV-BAK-03 — Backup drive photo
- EV-REC-01 — Email received at the reception mailbox
- EV-REC-02 — Sender address comparison card
- EV-REC-03 — Reception desk log note
- EV-REC-OFF-02 — Records access log extract
- EV-REC-OFF-01 — Records application account list
- EV-REC-OFF-03 — Practice manager statement — leavers
- EV-WKS-01 — Update status report (IT contractor, 13 March 2026)
- EV-WKS-02 — IT contractor statement
- EV-WKS-03 — Desk photo — signed-in laptop
- EV-WEB-03 — What the public website actually holds
- EV-WEB-02 — Renewal process note
- EV-WEB-01 — Website certificate details (captured 13 March 2026)

## My investigation notebook
RECEPTION
Worth protecting: appointment scheduling account \(AS-01\)
- EV-REC-01: Fake "Clinic IT Support" email from clinic-support.example asking for the password within 2 hours. Sender domain matches neither the real vendor nor the clinic.
- EV-REC-02: Real vendor is appointments-vendor.example; the clinic's domain is cloudheights-clinic.example. The display name proves nothing.
- EV-REC-03: Third message like this in three weeks, staff just delete them. Sign-in is email and password only.
- Cost if it goes wrong: an attacker could view, change or cancel all appointments.
- UNKNOWN: whether anyone replied or entered a password. No record of these ever being reported to the IT contractor.

RECORDS OFFICE
Worth protecting: patient records application \(AS-02\)
- EV-REC-OFF-01: All four reception staff sign in as one shared account, "frontdesk". The password is on a card in the desk drawer and was last changed 8 months ago.
- EV-REC-OFF-02: The log only shows "frontdesk", so it cannot show which person opened a record.
- EV-REC-OFF-03: The password is not changed when staff leave. Two reception staff left in the past year.
- Cost if it goes wrong: patient privacy breach, and no way to tell who did it.
- UNKNOWN: whether a former employee can still reach the application from outside the clinic, and whether past record openings were appropriate.

STAFF WORKSPACE
Worth protecting: staff laptops \(AS-03\)
- EV-WKS-01: 2 of 9 laptops \(CHC-04, CHC-07\) have had no security updates for 90 days. Both are used daily for email and records.
- EV-WKS-02: Staff keep postponing the restart and nobody owns the job.
- EV-WKS-03: A laptop sat signed in to the records application, unlocked and unattended, in a room that opens onto the patient corridor.
- Cost if it goes wrong: exposed patient records, or an infected laptop reaching other systems.
- UNKNOWN: no alert, malware or incident has been reported. Missing updates are a weakness, not a confirmed attack.

WEBSITE STATION
Worth protecting: public information website \(AS-04\)
- EV-WEB-01: The certificate expires 20 March 2026, 7 days after the scenario date.
- EV-WEB-02: Renewal is done by hand with no reminder and no owner. The last two renewals happened only after visitors saw warnings.
- EV-WEB-03: The site has no sign-in, no patient portal and no link to the records application.
- Cost if it goes wrong: visitors see a browser warning and lose trust. Low impact, because no patient data is on the site.
- UNKNOWN: an expired certificate does not by itself expose patient records.

## Risk scenarios

| ID | Asset | Evidence | Threat / event | Vulnerability | Consequence | CIA | Unknown / question |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SC-01 | Appointment scheduling account | EV\-REC\-01 \(Email received at the reception mailbox\); EV\-REC\-03 \(Reception desk log note\); EV\-REC\-02 \(Sender address comparison card\) | An attacker sends a fake "Clinic IT Support" email, a reception staff member enters the shared scheduling password on the fake page, and the attacker signs in to the account. | Reception staff sign in with an email and password only, with no second step. Fake emails like this are common \(the third in three weeks\), staff just delete them, and nothing is reported to the IT contractor. | Nobody has recorded whether anyone replied or entered a password. | Confidentiality, Integrity, Availability | Nobody has recorded whether anyone replied or entered a password. |
| SC-02 | Patient records application | EV\-REC\-01 \(Email received at the reception mailbox\); EV\-REC\-02 \(Sender address comparison card\); EV\-REC\-03 \(Reception desk log note\) | Threat: Someone who knows the shared "frontdesk" password, such as a former reception employee or anyone who finds the card in the desk drawer, signs in and opens or changes patient records. | All four reception staff share one login. Its password is written on a card in the desk drawer and was last changed 8 months ago. It is not changed when staff leave, and two have left in the past year. | Patient information could be seen or altered, and the clinic could not tell who did it, because the log only shows "frontdesk". | Confidentiality, Integrity | Whether former staff can reach the records application from outside the clinic |
| SC-03 | Staff laptops | EV\-REC\-01 \(Email received at the reception mailbox\); EV\-REC\-02 \(Sender address comparison card\); EV\-REC\-03 \(Reception desk log note\) | A person walking in from the patient corridor uses an unlocked, signed\-in laptop to read records, or malware exploits one of the two laptops \(CHC\-04, CHC\-07\) that have had no security updates for 90 days. | A laptop was left signed in to the records application with no one seated, in a room that opens onto the patient corridor. Two laptops used daily for email and records are 90 days behind on updates because staff keep postponing the restart and nobody owns it. | Patient records could be exposed, or an infected laptop could become a way into other clinic systems. | Confidentiality, Integrity | No alert, malware detection or incident has been reported. Missing updates are a weakness, not a confirmed attack. |
| SC-04 | Public information website | EV\-REC\-01 \(Email received at the reception mailbox\); EV\-REC\-02 \(Sender address comparison card\); EV\-REC\-03 \(Reception desk log note\) | The website's security certificate expires on 20 March 2026, and visitors' browsers show a warning before the page loads. | Renewal is done by hand, with no calendar reminder and no named owner. The last two renewals happened only after the site started warning visitors. | Patients may distrust the site or not reach opening hours and registration information. The impact is limited, because the site holds no patient data. | Availability |  |
| SC-05 | Backup archive | EV\-REC\-01 \(Email received at the reception mailbox\); EV\-REC\-02 \(Sender address comparison card\); EV\-REC\-03 \(Reception desk log note\) | Ransomware or a hardware failure hits the reception workstation and also damages the attached backup drive. | The only backup is one USB drive permanently attached to the reception workstation, with no offsite or cloud copy. The last good backup was 27 February 2026, the 6 jobs since then finished with errors, and a restore has never been tested. | The clinic could lose its patient records, or at least 14 days of them, and be unable to book or treat patients until the data is rebuilt. | Integrity, Availability | Whether the backups would restore, and why the last 6 jobs finished with errors |

## Email analysis
1. The sender address doesn't match. The email came from clinic-support.example, but the real vendor writes from appointments-vendor.example and the clinic's own domain is cloudheights-clinic.example. The display name "Clinic IT Support" is typed by the sender and proves nothing.
2. It creates urgency and a threat. The subject says "URGENT", it gives a 2-hour deadline, and it says the whole clinic's scheduling account will be locked.
3. It asks for the current password through a link. The link goes to account-verify.clinic-support.example, which belongs to neither the vendor nor the clinic. A real service would not ask you to confirm your password by email.

**Safe response / reporting step:** Do not click the link, reply, or enter a password. Report the email to the IT contractor \(Dev Marchetti\) and the practice manager \(Renata Coyle\), and check with the scheduling vendor using a contact I already know, not one from the email. Then delete the email.

**Suspicious vs proven:** This email looks like phishing, but looking suspicious isn't the same as being proven. The sender's address doesn't match the real vendor, it's pushing me to act fast, and it wants my password. Those things make me distrust it. But nothing in the evidence shows that anyone actually replied or typed a password in, so I can't say the account was broken into. To really know, I'd ask the front desk if anyone clicked the link, check with the vendor to see whether they sent it, and look through the scheduling account's sign-in history for anything unusual.
