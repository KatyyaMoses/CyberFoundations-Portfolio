# Week 6 Lab 04 — Reading the Blueprints

**Student Name:** Kay Moses

**Date Completed:** 8/21/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-04-reading-the-blueprints.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

**This is a SHORT lab — 15 to 20 minutes.** It is deliberately small. You already have the commands; this lab is about matching a drawing to reality.

The **Cloud Heights Network Blueprint** is displayed at the top of this lab page in the portal. Everything you write about the network's architecture comes from that blueprint or from your own machine — never from a guess.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Source of truth | The Cloud Heights Network Blueprint shown at the top of this lab page |
| Commands used | `ip addr`, `ip route` |
| Known value | Student subnet: **`10.60.6.0/26`** |

---

## Part A — Read the Drawing

### Step 1 — Record the Architecture Values

From the blueprint at the top of this page, record each value **exactly as drawn**. If a value is not shown on the blueprint, write "not shown on blueprint" — do not guess.

| Item | Value from the blueprint |
| --- | --- |
| VNet name | vnet-ct-labs |
| VNet address space | 10.60.6.0/24 |
| Student subnet range | 10.60.6.0/26 |

---

## Part B — Verify Against Your Own Machine

### Step 1 — Confirm Your Address Lives in the Subnet

Run `ip addr` and find your private IPv4 address.

Command and output:

```
analyst@cf-student-15:~$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 60:45:bd:4a:4a:6a brd ff:ff:ff:ff:ff:ff
    inet 10.60.6.34/26 metric 100 brd 10.60.6.63 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::6245:bdff:fe4a:4a6a/64 scope link 
       valid_lft forever preferred_lft forever
3: enP1018s1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc mq master eth0 state UP 
group default qlen 1000
    link/ether 60:45:bd:4a:4a:6a brd ff:ff:ff:ff:ff:ff
    altname enP1018p0s2
```

Your private IP:

```
10.60.6.34/26
```

Explain how you know your address falls inside `10.60.6.0/26` — what range does that prefix actually cover:

```
Based on the data, we are on the same street. This is a Class A private address. Both first three octets are the same, 10.60.6, which means we are on the same network. When it comes to the prefix notation, that deals with the number of borrowed bits. Since this is IPv4, there are 32 bits total. Therefore, I need to subtract 32 from the CIDR 26 and that leaves 6 host bits. Then, I us the power of 2 with 6 and that is 64. The subnet covers 64 addresses, running from 10.60.6.0 up to 10.60.6.63 (which is the inside global address, and it is the scope global eth0 in the vm). I want taught that we have to subtract one for the Broadcast ID. (So that is why it is 10.60.6.63 and not 64). I can confirm the top of that range in my own ip addr output, where the eth0 line shows brd 10.60.6.63. My address, 10.60.6.34, falls between 0 and 63, so it sits inside the subnet.
```

### Step 2 — Confirm Route Behaviour

Run `ip route`.

Command and output:

```
analyst@cf-student-15:~$ ip route
default via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.34 metric 100 
10.60.6.0/26 dev eth0 proto kernel scope link src 10.60.6.34 metric 100 
10.60.6.1 dev eth0 proto dhcp scope link src 10.60.6.34 metric 100 
168.63.129.16 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.34 metric 100 
169.254.169.254 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.34 metric 100 
```

What the default route tells you about traffic that is not destined for your own subnet:

```
It proves that the path reached the target. It reached it and responded, which is lock 1. Anything headed off my floor goes to the gateway at 10.60.6.1, which is my way out. So the default route is the exit for all traffic that isn't local to my own subnet.
```

### Step 3 — Capture Your Evidence

**Required filename:** `blueprint-verified.png`

This must be **your own `ip addr` and `ip route` output** — not a re-screenshot of the blueprint. Crop out the address bar and any login information.

![Blueprint verified — my address inside the student subnet](https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week-06/blueprint-verified.png?raw=true)

---

## Part C — How Traffic Actually Moves

### Step 1 — No Public IP

Your VM has a private address and **no public IP**. Explain what that means for who can reach it directly from the internet:

```
My VM has a private address, Class A and no public one. A private address only has meaning inside my own network, so nobody on the internet can reach my machine directly. I can still talk to other machines inside my own network since the MAC addresses, but for anything outside, my traffic has to go out through the gateway.
```

### Step 2 — Outbound vs. Inbound

Outbound internet traffic from your VM leaves through address **translation (NAT)**. Inbound access for you arrives through **Azure Bastion**, not through a public address on the VM.

Explain both directions in your own words:

```
Outbound: my VM has a private address, which means nothing out on the internet. So when my traffic leaves, NAT (network address translation) swaps my private address for a public one that the internet can use, and swaps it back when the reply comes home. 

Inbound: Nothing comes in through a public address. The only way in is through the Azure Bastion link in my browser, which is the one guarded entrance. So I guess the traffic goes out through NAT and comes in through Bastion.
```

### Step 3 — The Guard Post You Do Not Touch Yet

Each student machine sits behind its own **network security group** — a per-student guard post that decides what traffic is allowed in.

**In Week 6 you do not configure it.** Week 7 is when you take control of those rules.

Write one sentence naming what the guard post does and one sentence stating what you are *not* doing with it this week:

```
The network security group is a per-student guard post that decides what traffic is allowed into my machine. This week, I am not configuring it or setting any of those rules that comes in Week 7.
```

---

## Analysis Questions

**Analysis Question 1.** Why would an organization put every student machine in one small subnet instead of giving each machine a public address? *(Minimum 3 sentences.)*

```
An organization puts every machine in one small private subnet instead of giving each a public address because a public address would expose each machine directly to the internet, which is a security risk. A private subnet means none of the machines can be reached from outside, so there is no open door for an attacker to aim at. It also keeps all the student machines grouped and contained in one place. That makes it easier to manage and protect.
```

**Analysis Question 2.** Segmentation means separating a network into parts that cannot freely reach each other. Give one concrete benefit of segmentation during a security incident. *(Minimum 3 sentences.)*

```
Segmentation means separating a network into parts that cannot freely reach each other. The concrete benefit during a security incident is that an attack is contained. If someone breaks into one part, they are stuck in that small segment and cannot spread to everything else. Instead of the whole system being at risk, only that one piece is. This minimizes the damage.
```

**Analysis Question 3.** A diagram and a live machine disagree about an address range. Which do you trust, what do you do next, and why? *(Minimum 2 sentences.)*

```
I would trust the live machine because it shows the actual data of what is really going one. The diagram is only someone's drawing of what should be true, but things could have changed. What I would do next is not just assume the diagram is wrong, but treat the mismatch as a finding. I would write down what the diagram says, what the machine says, where they disagree, and then report it. Because a mismatch can mean something is misconfigured and needs looking into.
```

---

## Submission Checklist

- [x] VNet name, address space, and subnet range recorded from the blueprint (Part A)

- [x] `ip addr` run and own private IP confirmed inside `10.60.6.0/26` (Part B, Step 1)

- [x] `ip route` run and default route behaviour explained (Part B, Step 2)

- [x] `blueprint-verified.png` captured from your own terminal, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 3)

- [x] Private address / NAT / Bastion explained (Part C, Steps 1–2)

- [x] Per-student guard post identified — and explicitly not configured this week (Part C, Step 3)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-04-reading-the-blueprints.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 04: Reading the Blueprints** in the Lab Portal.
2. Fill in the worksheet fields and upload `blueprint-verified.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-04-reading-the-blueprints.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
