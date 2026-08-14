# Week 5 Lab 03 — Reading the Grid's Mail (Packet Inspector)

**Student Name:** Katyya Moses

**Date Completed:** 8/13/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 5  
**Submission Path:** `week-05/labs/lab-03-reading-the-grids-mail.md`

---

## Overview

Lesson 4 showed you the three panes of a packet analyzer — the packet list, the detail pane, and the filter bar — and told you that professionals live in this tool. This lab is where you stop watching and start driving. You'll open a recorded minute of traffic on The Grid and read it packet by packet: a name lookup (Part B), a handshake and the message that followed it (Part C), and one conversation that refuses to be read at all.

**The capture is the same for every student.** It's a recording, not a live network — fifteen packets, captured once, replayed identically for everyone. That means your numbers should match your classmates' exactly. If yours don't, you've filtered something out; clear the filter and look again.

**Nothing here can break anything.** You are reading a recording. There is no traffic to disturb and nothing to configure.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | CyberFoundations **Packet Inspector** — browser-based, inside the Lab Portal. Nothing to install. You will never install Wireshark for this course |
| Where to find it | Lab Portal main navigation, alongside the **CLI Simulator** |
| Capture | "The Grid — Workstation Capture." Canned and identical for every student |
| Prerequisite | Week 5, Lesson 3 (ports, the handshake) and Lesson 4 (the three panes) completed. Lab 01 recommended first — Part B builds directly on it |
| Filters available | `dns` · `icmp` · `tcp` · `http` · `ip.addr == x.x.x.x` · `tcp.port == N` |
| Time | Plan for 30–40 minutes, including this worksheet |

**Before you start:** log into the Lab Portal, open **Week 5 → Packet Inspector**, and load the capture named **"The Grid — Workstation Capture."** You should see a list of packets across the top, an empty detail pane below it, and a filter bar above everything. Keep this worksheet open in a second browser tab so you can record answers as you go.

**Get your screenshot tool ready now.** **Windows:** `Win + Shift + S` · **Mac:** `Cmd + Shift + 4`, then drag to capture. You'll need it twice in this lab, and both screenshots are required.

**A note on filters:** a filter never deletes packets. It hides the ones that don't match so you can concentrate. Clearing the filter box brings everything back. This matters in Part A — count *before* you filter anything.

---

## Part A — Orientation

### Step 1 — Count the Whole Capture

With the filter bar **empty**, look at the packet list and count how many packets the capture contains. Every packet has a number in the first column, so the last row tells you the answer without counting by hand.

The total number of packets in the capture:

```
15
```

### Step 2 — Inventory the Protocols

Scan the **Protocol** column from top to bottom and write down every distinct protocol name you see. There are five. You met all five in Lessons 2, 3 and 4 — this is the first time you're seeing them as labelled traffic rather than as ideas.

The protocols that appear in this capture:

```
DNS, ICMP, TCP, TLS, HTTP
```

### Step 3 — Read the Columns

Four columns do most of the work in a packet analyzer. In your own words — not the Lesson 4 wording — write one plain sentence for each: what does it tell you, and what question does it answer?

What the Source column tells you:

```
The source column tells the sender's IP address. It is where the packet is coming from. 
```

What the Destination column tells you:

```
It tells where the packet is headed.
```

What the Protocol column tells you:

```
The protocol is the agreed-upon rule that the packet must follow.
```

What the Info column tells you:

```
It is the steps and ports line by line of the network traffic. It shows how the packets were sent and received.
```

### Step 4 — Spot Your Own Machine

One address appears more often than any other, and it's the one sending most of the requests. That's the workstation the capture was taken from — the same address you recorded in Lab 01.

The workstation's IP address, and how you worked out it was the workstation:

```
The workstation's IP address is 10.20.5.42. I ran the command ip addr to get the workstation's IP address. 
```

---

## Part B — Follow a DNS Lookup

In Lab 01 you ran `dig foundry-archive.grid.local` and read a tidy answer: a name went in, an IP address came out. You saw the *result*. Now you're going to see the actual conversation that produced it — the question travelling across the wire and the answer coming back.

### Step 1 — Filter for DNS

Type `dns` into the filter bar and apply it. The packet list shrinks to the name-lookup traffic only.

The packet numbers that remain after the `dns` filter:

```
1 and 2
```

### Step 2 — Read the Question

Click the first of your two DNS packets and read its Info column and detail pane. This is the query — your workstation asking the Directory Board a question.

The packet number of the query, and the name being asked about:

```
Packet 1 - foundry-archive.grid.local 
```

The source and destination addresses of the query — who asked, and who was asked:

```
Source 10.20.5.42 (the workstation - who asked) and destination 10.20.5.10 (Directory Board - who was asked).
```

### Step 3 — Read the Answer

Now click the second DNS packet. This is the response coming back the other way.

The packet number of the response, and the IP address it returned:

```
Packet 2 - 10.20.5.20
```

### Step 4 — Find the Door

DNS doesn't just travel to an address — it travels to a specific **port** on that address, the way Lesson 3 described a numbered door on a building. Open the detail pane on either DNS packet and find the port number the lookup used.

The port number DNS used here:

```
53
```

### Step 5 — Tie It Back to Lab 01

This is the same lookup you ran with `dig` in Lab 01 — same name, same answer, same DNS server. The difference is the altitude you're viewing it from. `dig` handed you the conclusion; the Packet Inspector shows you the two messages that produced it.

In two or three sentences: what did the packet view show you that `dig` did not?

```
dig just handed me the final answer as it provided the address to 10.20.5.20. The packet showed when my machine asked a question and the other was the server response.
```

### Step 6 — Capture Screenshot 1 (REQUIRED)

With the `dns` filter still applied and one of the two DNS packets selected, take a screenshot showing the filter bar, the filtered packet list, and the detail pane. Name it exactly **`packet-dns-query.png`**. Upload instructions are in the GitHub Commit section.

---

## Part C — Doors and the Handshake

Lesson 3 told the TCP handshake as a knock at a door: **SYN** ("knock"), **SYN-ACK** ("who's there — come in"), **ACK** ("thanks"). You're about to watch one happen.

### Step 1 — Filter for Port 443

Clear the `dns` filter and enter `tcp.port == 443` instead. Port 443 is HTTPS — one of the well-known doors from Lesson 3.

The packet numbers that remain after the `tcp.port == 443` filter:

```
Packets 7, 8, and 9
```

### Step 2 — Identify the Three-Step Handshake

Three of those packets are the handshake itself. Find each one by reading the flags in the Info column, and record its number, its direction (which address sent it to which), and which step of the handshake it is.

The SYN — packet number and direction:

```
Packet 7 from the workstation chose an ephemeral port number 51514 (random number) to the server well-known port 443 (HTTPS). Pointing to the server.→
```

The SYN-ACK — packet number and direction:

```
Packet 8 From port 443 (HTTPS) on the server to port 51514 (random number). Pointing from the server back to the workstation  ←
```

The ACK — packet number and direction:

```
Packet 9 From workstation Port 51514 (random number) to the server port 443 (HTTPS). Pointing to the server.→
```

### Step 3 — Notice the Two Port Numbers

Look at the Info column on the SYN packet. Two port numbers appear: a high, odd-looking one on your workstation's side, and 443 on the server's side. Only one of them is a "well-known door" — the other is a temporary one your machine picked for this conversation.

The two port numbers, and which belongs to the server:

```
51514 and 443, the server provides the port 443

```

### Step 4 — Switch to the Plain Conversation

Clear the filter and enter `http` instead. This is a different conversation to a different machine on The Grid — the notice board — on port 80.

The packet numbers that remain after the `http` filter:

```
Packets 14 and 15
```

### Step 5 — Open the Request and Read It

Click **packet 14** and expand its detail pane all the way. Unlike everything you've read so far, this packet's contents are ordinary text — you can simply read them, the way you'd read a note left on a desk.

The request line at the top of packet 14 (the method, the page, and the version):

```
GET /shift-notice.html HTTP/1.1
```

The Host line — which machine the request was addressed to:

```
Host: grid-notice.grid.local
```

Every other readable line in packet 14's detail pane:

```
User-Agent: GridBrowser/2.4 
Staff-Code: FOUNDRY-2026-STOREROOM
```

### Step 6 — Notice What You Just Read

One of those lines is not like the others. It carries a value that looks like an internal code rather than a technical setting.

The name of that header and the exact value it carries:

```
Staff-Code: FOUNDRY-2026-STOREROOM
```

In one or two sentences: if you were sitting on this network with a packet analyzer open, what would you now know that you were never meant to know?

```
Based on a review of the packet analyzer, I know what the message that was sent. Since the message was sent over HTTP, anyone can review the message as it is not encrpyted.
```

### Step 7 — Capture Screenshot 2 (REQUIRED)

With the `http` filter applied and **packet 14** selected and expanded so the readable lines are visible, take a screenshot. Name it exactly **`packet-http-plaintext.png`**.

### Step 8 — Now Try to Read the Other One

Clear the filter and click **packet 10** — the TLS Client Hello from the port 443 conversation you examined in Steps 1–3. Expand its detail pane and try to read it the same way you just read packet 14.

What packet 10's detail pane shows you, described in your own words:

```
On packet 10, I can see the details layer by layer. On the Ethernet level, I can see the MAC addresses of the workstation and the server (foundry-archive), and I can see we're still using IPv4. On the Transmission Control Protocol level, I can see the handshake. My machine's ephemeral port and the destination port 443 (HTTPS), which means it's encrypted. The flags show PSH and ACK, so it was pushed and acknowledged. But there's a new layer, Application Data, that says "not readable in this capture." The lock is closed, and the contents are just scrambled, encrypted characters I can't read.
```

Compare it to packet 14 — what can you still tell about packet 10 from the packet list, even though you can't read its contents?

```
Even though I can't read packet 10's contents, I can still tell a lot from the outside: the MAC addresses, both IP addresses (10.20.5.42 to 10.20.5.20), that it's TCP going to port 443, and that it's HTTPS. So I know who is talking to whom and that it's encrypted web traffic — I just can't see what they said, unlike packet 14 where I could read the whole message. Since the message is not encrypted, the staff code was shared with anyone that wanted to read it. 
```

**Don't chase this yet.** You've just found something real, and the explanation is a later part of this course. For now, sit with the observation: two conversations, one legible and one not. Analysis Question 4 asks what you make of it.

---

## Analysis Questions

**Analysis Question 1.** In Part A you counted the packets before applying any filter. Explain why a filter is a *view* rather than a deletion, and describe a situation where forgetting that could lead an analyst to a wrong conclusion. *(Minimum 3 sentences.)*

```
The filter feature helps because when you're looking at a ton of packets, it gets overwhelming. Typing what you're looking for into the filter bar narrows it down so you're only seeing what matters instead of everything at once. And the ones that get filtered out aren't deleted; they're just not showing. It even tells you, like "2 of 15 packets," so you know the rest are still there, just hidden. That part matters, because if you forget it or you're worn out from staring at so many packets, it's easy to miss something or assume the hidden ones aren't there; and that's how you end up with the wrong answer.
```

**Analysis Question 2.** You have now seen the same DNS lookup twice — once as `dig` output in Lab 01, once as two packets here. Describe what each view is good for, and name one investigation where you would specifically want the packet view. *(Minimum 3 sentences.)*

```
The dig view is good when you just need a quick answer. It tells you the name goes to 10.20.5.20 and that's about it, so the info is kind of limited. The packet view in Wireshark is more for when you want to look closer, because you can actually see the messages going back and forth instead of just the answer. One time you'd want the packet view is the encryption part we did. In Wireshark I could read the HTTP one, but the HTTPS one was locked, and dig could never show me that.
```

**Analysis Question 3.** The handshake took three packets and about three milliseconds before a single byte of real content moved. Using the knock-at-the-door analogy from Lesson 3, explain what those three packets accomplish and why a connection would be less reliable without them. *(Minimum 3 sentences.)*

```
The handshake really works like someone knocking at a door. First, my machine knocks with a SYN. Basically saying "hey, are you there, I want to talk." Then, the server answers back with a SYN-ACK, like "yes, I'm here, come in." Then, my machine replies with an ACK, saying "okay, thanks." Now the connection is open. Those three steps matter because they make both sides confirm they can actually hear each other before sending any data. Without them, you'd just start talking without knowing if anyone was even listening, so the connection would be less reliable.
```

**Analysis Question 4.** You read packet 14 word for word, and packet 10 not at all — same capture, same network, same workstation. What do you think explains the difference between them? And why does it matter that the one you *could* read contained a staff code? You are not expected to know the mechanism yet; give us your best reasoning from what you observed. *(Minimum 4 sentences.)*

```
The biggest difference between the two is the protocol that was used. One used HTTP, which is port 80, and the other used HTTPS, which is port 443. Each one has its own rules, just like the SYN, SYN-ACK, ACK order we did above. In technology, you have to follow certain rules to keep things in order, and the rule here is that HTTPS encrypts the message and plain HTTP does not. That's why I could read packet 14 word for word, but packet 10 was locked. It matters that the readable one had a staff code in it, because sending things over plain HTTP means anyone capturing that traffic could read it. Sometimes you don't really care who sees a message, but something like a staff code you'd want encrypted so it's protected.
```

---

## Submission Checklist

- [x] Total packet count recorded with the filter bar empty (Part A, Step 1)

- [x] All five protocols listed (Part A, Step 2)

- [x] Source, Destination, Protocol and Info columns each explained in your own words (Part A, Step 3)

- [x] Workstation address identified with reasoning (Part A, Step 4)

- [x] `dns` filter applied; query and response packets identified by number (Part B, Steps 1–3)

- [x] Hostname queried, IP address returned, and port number recorded (Part B, Steps 2–4)

- [x] Lab 01 comparison written (Part B, Step 5)

- [x] **REQUIRED:** `packet-dns-query.png` uploaded to `assets/screenshots/week-05/` and its filename recorded (Part B, Step 6)

- [x] `tcp.port == 443` filter applied; SYN, SYN-ACK and ACK identified by number *and* direction (Part C, Steps 1–2)

- [x] `http` filter applied; the remaining packet numbers recorded (Part C, Step 4)

- [x] Both port numbers recorded, server's port identified (Part C, Step 3)

- [x] Packet 14's readable lines recorded, including the staff-code header and its exact value (Part C, Steps 5–6)

- [x] **REQUIRED:** `packet-http-plaintext.png` uploaded to `assets/screenshots/week-05/` and its filename recorded (Part C, Step 7)

- [x] Packet 10 opened and its contrast with packet 14 described (Part C, Step 8)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-05/labs/lab-03-reading-the-grids-mail.md`

---

## GitHub Commit Subsection

This lab's written answers are submitted through the **CyberFoundations Lab Portal**, the same way as Week 4.

1. Go to the CyberFoundations Lab Portal and sign in.
2. Open **Week 5 → Lab 03: Reading the Grid's Mail**.
3. Fill in the worksheet fields — they match the steps and questions in this file.
4. Connect your GitHub account if you haven't already (one-time setup), and select your portfolio repo.
5. Click **Submit to GitHub**. The Portal commits the completed file to `week-05/labs/lab-03-reading-the-grids-mail.md` for you.

**📸 REQUIRED — both screenshots.** This lab has two, and both are graded:

| Screenshot | From | Filename |
|---|---|---|
| Filtered DNS view with a query packet selected | Part B, Step 6 | `packet-dns-query.png` |
| Packet 14 expanded, readable lines visible | Part C, Step 7 | `packet-http-plaintext.png` |

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-05/` (create the folder if this is your first Week 5 screenshot).
2. Click **Add file → Upload files**, drag both images in, named exactly as above (lowercase, hyphens, no spaces).
3. Scroll down and click **Commit changes**.
4. Click each uploaded image's filename to open it and confirm the packet detail is readable at full size.
5. Record both filenames below so your grader knows to look for them.

The filename of your DNS screenshot:

```
https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week05/packet-dns-query.png
```

The filename of your packet 14 screenshot:

```
https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week05/packet-http-plaintext.png
```

Both screenshots live in `assets/screenshots/week-05/` in your repository. They do not need to be linked inside this worksheet.

**Commit message tip:** name the work, not the file type — *"Add Week 5 Lab 03 packet analysis evidence"* reads far better to an employer browsing your repo than "update."

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
