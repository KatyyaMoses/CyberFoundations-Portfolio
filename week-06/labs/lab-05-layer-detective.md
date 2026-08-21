# Week 6 Lab 05 — Layer Detective

**Student Name:** Kay Moses

**Date Completed:** 8/21/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-05-layer-detective.md`

---

## Overview

**This is a SHORT lab — 20 to 30 minutes — and it needs no VM.** No Cloud Heights session, no simulator, no screenshot. This is a thinking lab: you take the evidence you have already collected in Weeks 5 and 6 and sort it into layers.

This is an **independent** lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | This worksheet only — nothing to start, nothing to connect to |
| Prerequisite | Week 5 labs and Week 6 Labs 01–04 |
| Screenshot | None required |

---

## Part A — The Seven-Row Table

Fill in every row. For the last column, name one **real thing you personally saw** in Weeks 5–6 that belongs at that layer.

| # | Layer name | One-line job | Real thing from Weeks 5–6 |
| --- | --- | --- | --- |
| 7 | Application | allowing programs and users to access network. closest to the user | logging into Lab Portal |
| 6 | Presentation | Translating, formatting, and encrypting data so devices can understand it | png screenshots of lab content, tanslate data from the VM to Github |
| 5 | Sessions |  manages dialogue b/t computers. Start, maintain, and end  convservation between devies | setting session between VM, SSH Session |
| 4 | Transport | Ports & deliveries making sure that data gets delivered correctly and in the right order using TCP or UDP | network connection, streaming, TCP, 3 way handshake |
| 3 | Network | Routing and addressing. Moving data from one network to another using IP addresses | IPv4 and routers |
| 2 | Data Link | local delivery messages. local hops | MAC address and local hops via Class A IP address |
| 1 | Physical  | Signals and media getting bits across the wire | Cables, keyboards |

---

## Part B — Case Files

For each case, name the layer where the problem lives, and name the evidence proving the layers **below** it were already working.

### Case File 1 — The Name That Went Nowhere

A hostname lookup fails, but pinging the machine's IP address directly succeeds.

Layer:

```
Application
```

Evidence that the layers below were working:

```
Pinging the machine's IP address directly succeeded, which means the connection reached the machine. That proves the Physical, Datalink, Network, Transport, and Presentation layers were all working. Only the name lookup failed, which is a DNS problem at the Application layer.
```

### Case File 2 — Permission Denied

`ssh` to a host returns `Permission denied` after a password prompt.

Layer:

```
Application
```

Evidence that the layers below were working:

```
I got a password prompt, which means the connection reached the SSH service and was fully established. That proves the Physical, Datalink, Network, and Transport layers were all working, since the TCP handshake had to complete for me to be asked for a password. Only the login itself failed, which is authentication at the Application layer.
```

### Case File 3 — The Cable Story

A machine reports no link on its interface and has no address at all.

Layer:

```
Physcial
```

Evidence and reasoning:

```
Since there are no links on the interface and no address. We will start at the bottom at layer 1. We would have to see if the cables are connected. This will give us some information about the machine. 
```

### Case File 4 — Ping Works, The Page Does Not

`ping` to a server succeeds, but `curl http://<that server>` returns nothing useful.

Layer:

```
Application
```

Evidence that the layers below were working:

```
So, the ping to the server is working, as it reached the website. However, the HTTP server did not work. This means the Physical, Data Link, Network, and Transport layers are working since it pinged the server. 
```

### Case File 5 — Wrong Neighbourhood

A machine has an address, but its default route points somewhere that cannot forward its traffic.

Layer:

```
Network
```

Evidence and reasoning:

```
The machine is able to communicate on its local network using its MAC address, which shows the Physical and Data Link layers are working. The problem is that its default route points to a gateway that cannot forward its traffic, and routing is a Network layer job.
```

---

## Part C — The Silent Gateway Case

In Lab 03 the Azure default gateway did not answer your ping, yet name resolution, your beacon session, and the beacon's HTTP response all worked.

### Step 1 — Rule on the Case

Is this a layer failure? Answer, and justify using the other evidence you had:

```
No, this is not a layer failure. The gateway not answering ping does not mean anything is broken, because the platform gateway is configured not to reply to ping on purpose. I can prove it with the other evidence: name resolution worked, my beacon session worked, and the beacon's HTTP response came back.
```

### Step 2 — Name the Correct Conclusion

State the rule you would give a junior colleague about probes that go unanswered by a cloud platform:

```
An unanswered probe is not proof of failure. A cloud platform may be configured not to reply to ping as silence. That does not mean anything is broken. We prove it in a different way to determine if it has failures. Check whether other traffic reaches out and comes back, like a name lookup or a known-good reference point. If that works, the path is fine. We will the evidence to prove that the VM is configured that way. 
```

---

## Part D — Two Models, One Job

The OSI model has seven layers. The practical TCP/IP model most engineers speak day to day has four or five.

### Step 1 — Map Them

Briefly show how the seven OSI layers collapse into the practical model:

```
The seven OSI layers collapse like this:

Application, Presentation, and Session (OSI layers 7, 6, 5) all become one Application layer.
Transport (OSI layer 4) stays as Transport.
Network (OSI layer 3) becomes the Internet layer.
Data Link and Physical (OSI layers 2 and 1) combine into one Network Access (or Link) layer.

So the top three fold into one, the bottom two fold into one, and the two in the middle stay as they are, giving four layers instead of seven.

```

### Step 2 — When Each Is Useful

Explain when the seven-layer vocabulary helps and when the practical model is the better tool:

```
The seven-layer OSI model is more useful when explaining or teaching because its extra detail lets you point to exactly where something happens.

The practical four-layer TCP/IP model is the better tool when actually investigating a problem, because it matches how traffic really moves and has fewer layers to work through.
```

---

## Analysis Questions

**Analysis Question 1.** Explain the Ladder Rule using layer language. What does "test the near thing first" mean when the rungs are layers? *(Minimum 3 sentences.)*

```
The Ladder Rule means you start with the nearest thing and work your way out. In layer language, that means you start at the bottom layers and move up. The near stuff is the bottom of the stack, like the physical connection, then the address and routing, then the services on top. You go one layer at a time so you don't skip anything. And a lower layer breaking will break everything above it, so the bottom is where you look first.
```

**Analysis Question 2.** Why is "which layer is this?" a faster question than "what is broken?" when you are under pressure? *(Minimum 3 sentences.)*

```
Asking which layer it is gives you a place to start. The layers are stacked, and the top ones depend on the ones under them. So you start at the bottom and go up, and each layer you check rules out the ones below it. Asking what is broken is too broad. It could be anything, so under pressure you end up guessing. Which layer just points you to the problem faster.
```

**Analysis Question 3.** Pick one case file from Part B and describe the very next command you would run to confirm your ruling, and what result would change your mind. *(Minimum 2 sentences.)*

```
For Case File 5, I said it was a Network layer problem because the routing was off. The next thing I would run is ip route to see where the machine is sending its traffic. If ip route showed a working default gateway, then the routing is fine, and the problem is somewhere else.
```

---

## Submission Checklist

- [x] All seven rows of the OSI table completed with a real Week 5–6 anchor each (Part A)

- [x] All five case files given a layer and supporting evidence (Part B)

- [x] Silent gateway case ruled on correctly (Part C)

- [x] OSI vs. practical TCP/IP model compared (Part D)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] No screenshot required for this lab

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-05-layer-detective.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 05: Layer Detective** in the Lab Portal.
2. Fill in the worksheet fields.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-05-layer-detective.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
