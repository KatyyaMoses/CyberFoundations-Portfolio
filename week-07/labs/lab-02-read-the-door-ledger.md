# Week 7 Lab 02 — Read the Door Ledger

**Student Name:** Kay Moses

**Date Completed:** 8/29/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-02-read-the-door-ledger.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Learn to read inbound and outbound rule ledgers in evaluation order. Translate a rule from field values into plain English, then predict which matching rule makes the decision.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights → Security Rules |
| Change level | Read-only reasoning |
| Evaluation | Lower priority number first; first match wins |
| Time | 25–30 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

The priorities **250**, **300**, and **350** used in this lab are **hypothetical examples on paper only — do not create them**. This lab is prediction-only: do not add, edit, or delete rules, and do not run **Test My Rule** unless your instructor tells you to.

Remember the live baseline also contains the protected priority **1000** `deny-tcp8080-student-subnet` fallback (Inbound Deny TCP from `10.60.6.0/26` to port 8080), which is reached only when no earlier rule matches.

Two inbound rules match TCP 8080 from the same source: priority 250 is **Deny** and priority 300 is **Allow**. Predict the verdict before reading further.

```text
It will deny it. Both rules match the same TCP 8080 traffic, but Azure reads them lowest number first. Priority 250 (Deny) comes before priority 300 (Allow), so it matches first and wins. The Allow at 300 never gets reached, so the traffic is denied.
```

## Guided Steps

### Step 1 — Separate the Ledgers

Scroll below the yellow protected-rules summary to the detailed lists headed **INBOUND — EVALUATION ORDER** and **OUTBOUND — EVALUATION ORDER**. Read the inbound list, then the outbound list. Record one sentence explaining why an inbound allow does not automatically create an outbound allow.

```text
Inbound and outbound are evaluated as two separate lists, so a rule that allows traffic coming in doesn't say anything about traffic going out — each direction has its own rules that are checked on their own.
```

### Step 2 — Translate a Rule

Choose one visible protected rule and translate it using this form:

> Read in the [direction] ledger at priority [number], [allow/deny] [protocol] traffic from [source]:[source port] to [destination]:[destination port].

```text
Read in the Inbound ledger at priority 120, deny TCP traffic from 10.60.6.0/26:any to any:22.
```

### Step 3 — Evaluate in Order

For each hypothetical scenario below, list the rules in evaluation order, identify the first matching rule, and state the verdict. These rules are imaginary — do not create them.

1. Priority 250 Deny TCP from `10.60.6.4` to port 8080; priority 300 Allow the same traffic.
2. Priority 300 Allow TCP from `10.60.6.4` to port 8080; priority 350 Deny TCP from any source to port 8080.
3. An inbound Allow exists, but the traffic being evaluated is outbound.

```text
Scenario 1: Priority 250 (Deny) is read before priority 300 (Allow). Both match, but 250 comes first, so it wins. Verdict: DENIED.

Scenario 2: Priority 300 (Allow) is read before priority 350 (Deny). Both match, but 300 comes first, so it wins. Verdict: ALLOWED.

Scenario 3: The Allow is an inbound rule, but this traffic is outbound. Inbound and outbound are separate ledgers, so the inbound Allow never applies. With no outbound rule matching, it falls to Azure's default. Verdict: not allowed by that rule (the inbound Allow doesn't count for outbound traffic).
```

## Stop & Check

If you find yourself reading the Allow you want and ignoring an earlier matching Deny, restart at the lowest priority number. The ledger stops at the first match.

## Test

Use the displayed rule list to predict whether Grid Beacon TCP 8080 would currently be explicitly allowed by a student rule. Do not press **Test My Rule** yet unless your instructor directs you; the service may not be listening.

## Capture Evidence

Capture the detailed **INBOUND — EVALUATION ORDER** view (and **OUTBOUND — EVALUATION ORDER** if your evidence needs it), then name the first matching hypothetical rule and the resulting verdict for each scenario in your worksheet.

![Rule view in evaluation order — week07-lab02-evaluation-order.png](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab02-evaluation-order.png)

## Explain

Write a five-sentence explanation of first-match-wins that a classmate could use without memorizing Azure terminology.

```text
Think of the rules as a checklist you read from top to bottom, starting with the smallest number. You go down the list until you hit the first rule that fits the traffic you're looking at. As soon as one fits, that's the answer. You stop reading right there. Anything below that rule never even gets looked at. So the winner isn't the strictest rule or the newest one; it's just whichever one comes first in line.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab02-evaluation-order.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** If a Deny at priority 300 and an Allow at priority 400 both match, which wins and why? (Minimum 3 sentences.)

```text
The Deny at 300 wins. Rules are read from the lowest number up, so 300 is checked before 400. Since it matches the traffic first, the check stops right there, and the Allow at 400 is never reached. So the traffic ends up denied.
```

**Analysis Question 2.** Why do inbound and outbound rules have to be reasoned about separately? (Minimum 3 sentences.)

```text
Inbound and outbound are two separate lists that Azure checks on their own. A rule that lets traffic come in doesn't do anything for traffic going out, and the other way around too. So you can't assume that opening one direction opens the other. You have to look at each ledger by itself to know what actually happens.
```

**Analysis Question 3.** How can an Allow rule be correct by itself but ineffective in the full ledger? (Minimum 3 sentences.)

```text
An Allow rule can have all the right settings and still never do anything if a Deny with a lower number matches the same traffic first. Since the check stops at the first match, that earlier Deny wins and the Allow never gets reached. So the rule looks correct on its own, but its position in the list makes it useless. It's not the rule that's wrong; it's where it sits in the order.
```

## Submission Checklist

- [x] Three scenarios evaluated in order

- [x] One live rule translated to plain English

- [x] Inbound and outbound ledgers distinguished

- [x] `week07-lab02-evaluation-order.png` captured

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-02-read-the-door-ledger.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 02: Read the Door Ledger** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
