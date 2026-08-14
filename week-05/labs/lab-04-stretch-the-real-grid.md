# Week 5 Stretch Lab (OPTIONAL) — The Real Grid

**Student Name:** Katyya  Moses

**Date Completed:** 8/14/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 5  
**Submission Path:** `week-05/labs/lab-04-stretch-the-real-grid.md`

---

## Overview

Everything you've done this week ran inside The Grid — a network built to behave predictably so you could learn to read it. This stretch lab points the same four questions at a network nobody designed for teaching: your own. You'll find your real address (Part A), trace a path across the actual internet (Part B), look up a name that answers with more than one number (Part C), and then do the step that matters most professionally — decide what is safe to publish before you commit anything (Part D).

**This lab is optional, and it is rated when you submit it.** If you complete it, it's read and scored like any other lab and it strengthens your portfolio.

**Skipping it costs you nothing.** Your grade for Week 5 comes from Labs 01, 02 and 03 — those three are the graded path and they are complete on their own. A locked-down work laptop that blocks the terminal, a Chromebook, a shared family computer you'd rather not photograph, a machine you don't administer — all of these are perfectly legitimate reasons not to run this lab, and none of them says anything about you as a student. Choose freely.

**Built-in commands only.** Every command in this lab already exists on your computer. **You will not install anything** — not for this lab, not for this program. If any instruction anywhere seems to ask you to install software, stop and ask your instructor; that instruction is wrong.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Your own computer, Windows or macOS. Nothing to install, ever |
| What you'll open | **Windows:** Command Prompt (`cmd`) or PowerShell — press `Win`, type `cmd`, press Enter · **macOS:** Terminal — press `Cmd + Space`, type `terminal`, press Enter |
| Prerequisite | Week 5 Lessons 1, 2 and 4; Labs 01 and 02 completed first |
| Status | **Optional. Rated when submitted.** The three core labs are the graded path |
| Time | Plan for 30–45 minutes, including redaction |

**Linux users:** the macOS column works for you with one exception — `traceroute` may not be present on every distribution. If it isn't, use `tracepath` if you already have it, or skip Part B and say so. Do not install anything.

---

## The Command Table — Read This Before You Start

Your operating system decides which command you type. Find your column and stay in it.

| Goal | Windows | macOS |
|---|---|---|
| Your address | `ipconfig` | `ifconfig` or `ipconfig getifaddr en0` |
| Reachability | `ping -n 4 HOST` | `ping -c 4 HOST` |
| Trace the path | `tracert HOST` | `traceroute HOST` |
| Name lookup | `nslookup HOST` | `dig HOST` or `nslookup HOST` |

Replace `HOST` with the name or address you're testing.

**Windows has no `dig`.** It's a real command and a good one, but it does not ship with Windows — you used it in the CLI Simulator, which runs a Linux-style shell. On Windows, `nslookup` is the built-in equivalent and it answers the same question. **Do not install `dig`, or the BIND tools that contain it, to get around this.**

Two more differences worth knowing:
- **`ping` runs forever on macOS/Linux unless you stop it.** That's what `-c 4` is for — "send four and stop." Windows sends four by default; `-n 4` makes it explicit. If a ping ever won't stop, press `Ctrl + C`.
- **The trace command is spelled differently on each platform**: `tracert` on Windows (no "u"), `traceroute` on macOS. Same idea, same output shape.

---

## Part A — Your Real Address

### Step 1 — Ask Your Machine Where It Lives

Open your terminal and run the address command from your column. On macOS, `ipconfig getifaddr en0` gives you one clean line; plain `ifconfig` gives you everything, which is messier but more honest about what's really there.

The command you ran:

```
C:\Users\computername>ipconfig

```

### Step 2 — Find Your IPv4 Address and Gateway

Somewhere in that output is a four-part number like `192.168.1.something` or `10.0.0.something` — your IPv4 address on your local network. Nearby you should find a **default gateway** (Windows labels it plainly; on macOS you may need to look at your Wi-Fi settings instead, which is fine).

Your IPv4 address and, if you can find it, your default gateway:

```
IPv4 Address. . . . . . . . . . . : 192.168.0.169
 
Default Gateway . . . . . . . . . : 192.168.0.1
```

### Step 3 — Compare It With The Grid

In the simulator, your address was `10.20.5.42` — one adapter, one address, one gateway, perfectly tidy. Your real output is probably not tidy. Most machines show several network adapters (Wi-Fi and Ethernet, often both listed even when only one is connected), plus things like a VPN adapter, a virtual adapter from other software, and long IPv6 addresses full of colons alongside the IPv4 ones.

That mess is normal. Real machines wear several address plates at once.

What your real output has that the simulator's didn't — list what you see:

```
My real output shows three Wireless LAN adapters instead of just one: "Local Area Connection* 1," "Local Area Connection* 2," and "Wi-Fi." The first two say "Media disconnected," so they're not actually in use. It also shows a Link-local IPv6 address (fe80::fd2d:be26:b483:d8df%3) full of colons, which the simulator didn't have. So, my machine has multiple adapters and both IPv6 and IPv4 addresses, not just one clean IPv4 line.
```

Which of the addresses you found is the one your computer actually uses to reach the internet, and how did you decide?

```
My computer uses 192.168.0.169. That's my machine's address, the client getting access to the internet. I decided on it because the default gateway (192.168.0.1) is the access point to the worldwide web, so that's the connection my computer actually uses to get out.
```

### Step 4 — Private or Public?

Lesson 1 covered private address ranges — `10.x.x.x`, `172.16–31.x.x`, and `192.168.x.x` are *inside* addresses, used on local networks everywhere and never routed across the internet directly.

Does your IPv4 address fall in a private range, and what does that tell you about how your machine reaches the outside world?

```
My address is private, and it's a Class C address because it falls in the 192.168 range. That means it doesn't actually go out onto the internet, because remember it has to go through the gateway to do that. This address is just for talking to other machines on my own personal network. On my local network it uses MAC addresses to communicate with things like my router to get out to the internet.
```

---

## Part B — Trace Into the Real World

### Step 1 — Trace to a Distant Host

Pick one well-known, distant host and trace the path to it. Any of these work: `wikipedia.org`, `bbc.co.uk`, `cloudflare.com`, `example.com`. Pick one that's geographically far from you if you can — the distance is the point.

Run the trace command from your column. **Be patient:** a real traceroute can take a minute or more, because every hop that doesn't answer has to time out before the trace moves on.

The command you ran, including the host you chose:

```
C:\Users\computername> tracert cloudflare.com
```

### Step 2 — Count the Hops

Each numbered line is one router your traffic passed through on its way. In the simulator, `cloud-heights.grid.local` was three hops away. The real internet is deeper than that.

The number of hops your trace showed:

```
12
```

The first hop's address — and one sentence on what that device is:

```
192.168.0.1 is my own router (the default gateway). The first device my traffic passes through to leave my home network.
```

### Step 3 — Watch the Latency Climb

Each hop reports a round-trip time in milliseconds. Early hops (your own router, your ISP) are usually very fast. Later hops, further away, are slower — because the data is physically travelling further and passing through more equipment.

The round-trip time at your first hop, and at your last hop:

```
First hop: about 3-6 ms. Last hop: about 18-37 ms.
```

In two or three sentences: describe the pattern you see, and say what it suggests about the physical distance the data covered.

```
The first hop was the quickest since it did not have to travel far on my router. After that it timed out until hop 10. I guess it finally made it to a destination. Therefore, it was much further than in the beginning. As it passes through other servers, it makes the trip takes longer. 
```

### Step 4 — About Those Stars

You will almost certainly see lines that look like this:

```
 7   * * *   Request timed out.
```

**This is completely normal and it is not an error.** Plenty of routers on the internet are configured not to reply to trace requests — sometimes for security reasons, sometimes just because their operator saw no reason to answer. Your packets still passed through those routers perfectly well; the router simply declined to say hello on the way past.

**A trace that ends in a run of stars has not failed.** It very often means the destination's own network doesn't answer traces, even though the site loads fine in your browser. Nothing on your machine is broken, you didn't type it wrong, and you don't need to try again with a different host.

> **Wait — didn't stars mean something was broken?**
>
> In Lab 02 you saw `* * *` immediately after the gateway and concluded the relay station was down. Here you'll see stars too, and nothing is wrong. The difference isn't the stars — it's **whether the trace keeps going**.
>
> A few starred hops in the middle of a trace that then *continues* and *arrives* means some router along the way politely declined to answer the probe. Your traffic went straight through it. That's ordinary internet behaviour and most real traces have it.
>
> Stars that start early and run all the way to the end, with the trace **never arriving**, are the Lab 02 pattern — the path stops being visible and nothing at the far end ever answers.
>
> Same symbol, two very different stories. Reading which one you're looking at is the actual skill.

Whether your trace showed any `* * *` lines, and roughly where:

```
yes, there were stars from hop 2 to hop 9.  8 hops with stars
```

In one or two sentences: what do those stars tell you, and what do they specifically *not* tell you?

```
The stars mean that hop didn't reply to my trace, and the "request timed out" is just it waiting and then moving on. They don't tell me anything is broken, because my traffic still passed through those hops and made it all the way to the last one. Some routers just don't answer trace requests on purpose.
```

### Step 5 — Capture Your Screenshot (REQUIRED if you submit this lab)

Take a screenshot of your traceroute output. Name it exactly **`stretch-real-traceroute.png`**.

**Do not upload it yet.** Part D is where you make it safe to publish, and Part D is not optional.

---

## Part C — One Name, Many Numbers

### Step 1 — Look Up a Large Website

Use the name lookup command from your column against a big, busy website — `google.com`, `amazon.com`, `wikipedia.org` and `cloudflare.com` all work well. Windows students: this is `nslookup`.

The command you ran and the name you looked up:

```
C:\Users\computername>nslookup amazon.com
```

### Step 2 — Count the Answers

In Lab 01, `foundry-archive.grid.local` gave you exactly one address: `10.20.5.20`. One name, one number, clean. Real large sites usually don't behave that way.

How many addresses came back, and what they were:

```
3 
Addresses:  98.82.161.185
          98.87.170.74
          98.87.170.71
```

### Step 3 — Reason About Why

A single name answering with several different addresses is deliberate engineering, not a glitch. Think about what a service like Google or Amazon has to handle: millions of people at once, spread across the world, and equipment that occasionally fails or needs maintenance.

Why would a very large service want one name to answer with several addresses? Give at least two distinct reasons.

```
This is load balancing. Amazon makes money by people buying stuff, so if millions of people all over the world are hitting the site at once, they have to make sure their servers can handle all that traffic. Having several addresses lets them spread the load across multiple servers instead of piling it all on one. They also need backups on top of backups, so that if one of those servers goes down, the others can pick up the load and the site stays working. (this is what I have gathered from my cybersecurity studies)
```

### Step 4 — Try It Again

Run the exact same lookup a second time, and compare.

Whether the addresses changed, stayed the same, or came back in a different order:

```
The addresses stayed the same, but they came back in a different order.
Addresses:  98.87.170.74
          98.82.161.185
          98.87.170.71
```

---

## Part D — Redaction (REQUIRED — do not skip this)

Everything you've run so far describes your actual home. Your terminal output can contain your **public IP address** (which maps to roughly where you live and who your internet provider is), your **computer's hostname** (which is very often your real name — `marias-macbook-pro`), the **username in your shell prompt**, and your **ISP's name** in traceroute hostnames.

You are about to publish this to a public GitHub repository that employers will read.

**This is not paranoia — it is the job.** Security professionals write reports, file tickets, post screenshots in chat, and present findings constantly, and every single time they make one judgement first: *what in this evidence is safe to share, and what has to be covered?* Get it wrong in a real incident report and you leak your client's infrastructure to whoever reads it. Practising that judgement here, on your own data, where the stakes are zero, is how you build the reflex before it counts.

**Redaction is a graded part of this lab.** An unredacted public IP or hostname in a committed screenshot comes back for correction every time.

### Step 1 — Find What Needs Covering

Look carefully at your screenshot from Part B, Step 5 and at anything you plan to paste into this worksheet. Hunt for these four things specifically:

1. **Your public IP address** — the address the outside world sees you as. It often appears as an early hop in a traceroute, and it is not a private `10.x` / `192.168.x` address.
2. **Your computer's hostname** — check your terminal's title bar and window title, not just the command output. This is where real names hide.
3. **The username in your shell prompt** — the text before the `$` or `>` on every line, e.g. `maria@marias-macbook ~ %`.
4. **Any ISP name** — real traceroute hops carry hostnames like `cust-73-42-19-8.yourisp.net`, which name your provider and sometimes your city.

What you found in your own output that needs covering — list each item:

```
my username

```

### Step 2 — Cover It Properly

Two methods, both built into your operating system. **You are not installing an editing tool for this.**

**Crop it.** The simplest and safest option. If the sensitive lines are at the top or bottom, just cut them off — cropped pixels are gone, not hidden.
- **macOS:** open the image in **Preview**, drag a box around the part you want to keep, then `Tools → Crop`.
- **Windows:** open it in **Photos** or **Paint**, use **Crop**, keep the region you want.

**Or draw a solid filled box over it.**
- **macOS Preview:** `Tools → Annotate → Rectangle`, then use the **fill colour** control to make it a *solid* colour — black is fine. A rectangle with no fill is just an outline around your data.
- **Windows Snip & Sketch / Paint:** choose the filled-rectangle shape, set the fill to a solid colour, and draw it over the text.

**⚠️ Do not blur, pixelate, or use a marker-pen highlight.** Blurring and pixelation transform the original pixels rather than replacing them, and there are well-known techniques for reversing them and recovering the original text — this has burned real professionals in published reports. A **solid, opaque box** contains no information about what was underneath it. Cropping is even better, because the data is simply not in the file.

**Also check:** semi-transparent boxes (drag the opacity slider to full), and boxes that don't quite cover the last character.

Which method you used and what you covered:

```
I cropped the username in shell prompt
```

### Step 3 — Re-Read Your Own Worksheet

Screenshots aren't the only leak. Scroll back through the answers you typed in Parts A, B and C and check the *text* too — a pasted traceroute in Part B or an address in Part A can carry exactly the same information.

Where a private local address like `192.168.1.14` is fine to publish (it means nothing outside your house, and millions of people have the same one), a **public** IP is not.

What you changed in your typed answers, if anything:

```
nothing needed to be changed here
```

### Step 4 — Pre-Flight Checklist

Tick every line before you upload anything. If you can't tick one, fix it first.

- [x] My public IP address does not appear in the screenshot

- [x] My public IP address does not appear in any answer I typed

- [x] My computer's hostname is not visible — including in the terminal's title bar

- [x] My username is not visible in the shell prompt

- [x] My ISP's name does not appear in any hop hostname

- [x] Every redaction is a **solid filled box or a crop** — no blur, no pixelation, no outline-only rectangle, no partial transparency

- [x] I opened the final image full size and looked at it once more, edge to edge

- [x] I would be comfortable with this image being visible to anyone on the internet, forever

That last line is the real test. A public repository is public, and it is permanent.

---

## Analysis Questions

**Analysis Question 1.** Compare the simulator's network to your real one. Name two specific ways your real output was messier or more complicated than `10.20.5.42`, and explain why a teaching environment is built tidy in the first place. *(Minimum 3 sentences.)*

```
The simulated one is just information the person who created it put in there so we can learn, and it's cleaner that way so we can see things clearer while we're learning. But when you go into my real command prompt, you see way more things because it's real. For example, I can see adapters that say "media disconnected" and the IPv6 address and other extra stuff the simulator didn't show. The timeouts were different too, because in my real trace it went through about eight hops that timed out and didn't do anything, and then around the tenth one it started working again, which is messier than the clean simulator trace. The teaching environment is built tidy on purpose so we can understand what's happening before dealing with the messy real thing.
```

**Analysis Question 2.** Your traceroute crossed routers belonging to organisations you've never heard of, and some of them refused to identify themselves. Using what you saw in Part B, explain what a traceroute can and cannot tell you — and why `* * *` is not evidence that anything is broken. *(Minimum 3 sentences.)*

```
Traceroute shows you the route to an address and all the hops in between, so you can see how far your traffic traveled. It reached my router fast, then a bunch of hops timed out. After that, it picked back up and made it to the last hop. But those stars don't mean anything is broken. Some routers just don't reply to the trace. My traffic still passed through and made it all the way to the end.
```

**Analysis Question 3.** Part D asked you to decide what was safe to publish. Walk through your own judgement: what did you choose to hide, what did you judge safe to leave visible, and how did you decide where the line was? Name one thing you deliberately left in and explain why it was safe. *(Minimum 4 sentences.)*

```
What I chose to hide was my username that showed in my command prompt. I hid it because it's personal to my computer, and it's just something you don't want other people to have access to. What I judged safe to leave visible was my private gateway address, 192.168.0.1. At first I wasn't sure, but then I realized private addresses like that exist inside millions of home networks and don't identify me to the outside world, and reading further in the lab confirmed that. I decided where the line was by looking at what actually identifies me personally versus what's just generic network info, and I could see both right there on the image and in the command line output.
```

---

## Submission Checklist

- [x] Address command run and IPv4 address recorded (Part A, Steps 1–2)

- [x] Real output compared with the simulator's `10.20.5.42`, extra adapters listed (Part A, Step 3)

- [x] Private vs public reasoning recorded (Part A, Step 4)

- [x] Traceroute run to a distant host; hop count and first hop recorded (Part B, Steps 1–2)

- [x] Latency pattern described from first hop to last (Part B, Step 3)

- [x] `* * *` timeouts observed and interpreted correctly (Part B, Step 4)

- [x] Name lookup run against a large site; multiple addresses recorded (Part C, Steps 1–2)

- [x] At least two reasons given for why one name answers with several addresses (Part C, Step 3)

- [x] Lookup repeated and any change described (Part C, Step 4)

- [x] **REQUIRED:** all four redaction targets checked — public IP, hostname, username, ISP name (Part D, Step 1)

- [x] **REQUIRED:** redaction done by crop or solid filled box — never blur or pixelation (Part D, Step 2)

- [x] **REQUIRED:** typed answers re-read and redacted where needed (Part D, Step 3)

- [x] **REQUIRED:** every line of the pre-flight checklist ticked (Part D, Step 4)

- [x] **REQUIRED:** `stretch-real-traceroute.png` — **redacted** — uploaded to `assets/screenshots/week-05/` and its filename recorded (Part B, Step 5)

- [ ] All three Analysis Questions answered (minimum sentence counts met)

- [ ] This file is committed to your portfolio repo at `week-05/labs/lab-04-stretch-the-real-grid.md`

---

## GitHub Commit Subsection

Submit this lab exactly like the three core labs — through the **CyberFoundations Lab Portal**.

1. Go to the CyberFoundations Lab Portal and sign in.
2. Open **Week 5 → Stretch Lab: The Real Grid**.
3. Fill in the worksheet fields — they match the steps and questions in this file.
4. Connect your GitHub account if you haven't already (one-time setup), and select your portfolio repo.
5. Click **Submit to GitHub**. The Portal commits the completed file to `week-05/labs/lab-04-stretch-the-real-grid.md` for you.

**📸 Your screenshot — redacted first.** Do not start these steps until every line of the Part D pre-flight checklist is ticked. Once an image is committed to a public repository, deleting it later does not reliably remove it from the history.

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-05/`.
2. Click **Add file → Upload files**, drag in your **redacted** screenshot, named `stretch-real-traceroute.png` (lowercase, hyphens, no spaces).
3. Scroll down and click **Commit changes**.
4. Click the uploaded image's filename to open it — the image itself will display on the page.
5. **Look at it one final time, full size, on GitHub.** This is your last checkpoint before it's public. If anything sensitive survived, delete the file and re-upload a corrected version now.
6. Record the filename below so your grader knows to look for it.

The screenshot filename you uploaded:

```
https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week05/stretch-real-traceroute.png
```

Your screenshot lives in `assets/screenshots/week-05/` in your repository. It does not need to be linked inside this worksheet.

**Commit message tip:** *"Add Week 5 stretch lab — real-network trace, redacted"* tells a reader both what you did and that you thought about disclosure. That second half is the part hiring managers notice.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
