# Week 6 Stretch Lab — Night Watch (Optional)

**Student Name:** Kay Moses

**Date Completed:** 8/22/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-06-stretch-night-watch.md`

---

## Overview

**This lab is optional.** Skipping it costs you nothing. Your Week 6 submission is complete and full-credit with Labs 01–05 alone, and choosing not to do this does not make your week's work lesser in any way. It is here for students who want to connect Cloud Heights to the network they actually live on. Expect about 30 minutes if you take it on.

**Built-in tools only.** Use commands that already ship with your own computer. **Do not install any software** for this lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Your own personal computer — not Cloud Heights |
| Tools | Built-in only: `ipconfig` / `Get-NetIPConfiguration` (Windows) or `ifconfig` / `ip addr` / `netstat -rn` (macOS/Linux), plus `ping` |
| Installs | None. If it needs installing, it is out of scope. |
| Screenshot | Required **only if you submit**: `stretch-real-network.png`, redacted |

> ### 🔒 Redaction Rule
> If you submit this lab, you must redact before uploading. Never publish your home network identity.

---

## Part A — Your Own Address

### Step 1 — Read Your Machine's Network Configuration

Use the built-in command for your operating system to show your address, subnet mask/prefix, and default gateway.

Command you ran:

```
C:\Users\yourname>ipconfig
```

Your private IP and prefix/mask (this is a private address — safe to record):

```
192.168.0.190/24
```

Your default gateway (private address — safe to record):

```
192.168.0.1
```

**Do not record your public IP address anywhere in this file.**

### Step 2 — Compare to Cloud Heights

Compare your home addressing to your Cloud Heights addressing — address range, prefix size, and how many machines each network could hold:

```
My Cloud Heights network used the range 10.60.6.0/26. The /26 is a smaller block, holding 64 addresses total, with about 62 usable for machines. My home network is 192.168.0.0/24, with my machine at 192.168.0.190 and my gateway at 192.168.0.1. The /24 is a bigger block, holding 256 addresses, with about 254 usable. So my home network is larger than my Cloud Heights subnet, because a smaller slash number means a bigger range. Both use private addresses, just different sizes: Cloud Heights was carved into a smaller /26 floor, while my home router hands out a full /24.
```

---

## Part B — Two Gateways, Two Behaviours

### Step 1 — Ping Your Home Gateway

Ping your own default gateway.

Output:

```
Pinging 192.168.0.1 with 32 bytes of data:
Reply from 192.168.0.1: bytes=32 time=2ms TTL=64
Reply from 192.168.0.1: bytes=32 time=2ms TTL=64
Reply from 192.168.0.1: bytes=32 time=6ms TTL=64
Reply from 192.168.0.1: bytes=32 time=6ms TTL=64

Ping statistics for 192.168.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 2ms, Maximum = 6ms, Average = 4ms
```

### Step 2 — Compare to Azure's Silent Gateway

Your home router almost certainly answered. The Azure gateway in Lab 03 did not.

Explain why both behaviours are normal, and what that teaches you about assuming a non-answer means failure:

```
Both behaviors are normal. At home, a regular router usually answers ping, because most home setups aren't locked down and there's no reason to block it. In the Azure environment, the default gateway is set on purpose not to answer ping. That's a choice the platform makes, not a sign anything is broken. So one answered and one stayed silent, and both are working exactly as they're supposed to.

What this teaches me is that a non-answer does not mean failure. The Azure gateway going quiet was expected behavior, not a fault. Before assuming something is broken, I have to check with other evidence, because silence can just mean a device chose not to reply.
```

---

## Part C — Redaction (Required Only If You Submit)

**Required filename:** `stretch-real-network.png`

Redact **before** uploading. Redaction targets:

- your **public IP address**
- your computer's **hostname**
- your **shell username**
- any **ISP-identifying names** (router model strings, provider names, SSIDs)

**Method:** crop the image, or cover the text with **solid opaque boxes**. **Do not use blur or pixelation** — both can be reversed.

![Home network configuration — redacted](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/869f4602c60f0fd974528cb4491d5aa0aa55bc24/assets/screenshots/week-06/stretch-real-network.png)

List what you redacted and the method you used:

```
PC name and router name
```

---

## Analysis Questions

**Analysis Question 1.** What is genuinely different between the network you sit on at home and the one Cloud Heights sits on, and what is essentially the same? *(Minimum 3 sentences.)*

```
Both use private addresses, both have a default gateway at .1, and my machine sits inside a subnet on both. My home is 192.168.0.0/24 and Cloud Heights was 10.60.6.0/26, but they work the same way. Home is a bigger /24 for personal use, while Cloud Heights was a smaller /26 carved out inside a larger cloud environment with other machines and stricter setup.
```

**Analysis Question 2.** Why is publishing your public IP, hostname, and username together riskier than publishing any one of them alone? *(Minimum 3 sentences.)*

```
Publishing all three together is riskier than any one alone because each piece gives an attacker part of the picture, and together they complete it. The IP tells them where to find you, the hostname tells them what the machine is, and the username gives them a real login name to try. Alone, each one is fairly limited, but combined they hand an attacker most of what they need to target you directly. It increases the attack surface and makes it much easier for them to focus an attack instead of guessing.
```

---

## Submission Checklist (Only If You Choose to Submit)

- [x] Home address, mask, and gateway recorded — **public IP not recorded**

- [x] Home network compared to Cloud Heights (Part A, Step 2)

- [x] Home gateway pinged and compared to Azure's silent gateway (Part B)

- [x] `stretch-real-network.png` redacted with crop or solid boxes (no blur), uploaded to `assets/screenshots/week-06/`

- [x] Redaction list recorded (Part C)

- [x] Both Analysis Questions answered

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-06-stretch-night-watch.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Stretch Lab: Night Watch** in the Lab Portal.
2. Fill in the worksheet fields and upload your redacted screenshot to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-06-stretch-night-watch.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
