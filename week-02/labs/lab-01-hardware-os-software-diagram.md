# Week 2 Lab 01 — Cybersecurity Landscape & Digital Infrastructure Overview

**Student Name:** Kay Moses

**Date Completed:** 7/22/2026

**Module:** 1 — Digital Infrastructure & CLI | **Week:** 2  
**Submission Path:** `week-02/labs/lab-01-hardware-os-software-diagram.md`

---

## Overview

In this lab, you build a working mental model of the system you'll be securing throughout this course: the hardware, operating system, and software layers that make up every computer, and where the cybersecurity field fits around them. This lab has two parts. Part A connects this week's material to the CyberFoundations City map. Part B has you build and explain a diagram of how a computer's hardware, OS, and software layers interact.

**No terminal or command line is required this week** — that starts in Week 3.

---

## Lab Environment

| Component | Details |
|---|---|
| Environment | Browser-based Lab Portal (Module 1 orientation) |
| Required Materials | CyberFoundations City map; a diagram tool of your choice (hand-drawn and photographed, or any digital tool) |

**Prerequisite:** Portfolio repo created from the CyberFoundations student template in Week 1. Fill out this worksheet here in the Lab Portal, then hit Submit to commit it directly to your repo at `week-02/labs/lab-01-hardware-os-software-diagram.md`.

---

## Part A — CyberFoundations City & the Cybersecurity Landscape

The CyberFoundations City map is your visual guide to the next 11 weeks. Each district represents a module of this course. This part connects this week's material to the map you were introduced to in Week 1.

### Step 1 — Open the Lab Portal Orientation Module

From your Student Dashboard, open the **Module 1 orientation** module.

### Step 2 — Complete the Orientation Walkthrough

Work through the orientation content. It covers the same hardware/OS/software material as this week's lessons from a different angle — use it to check your understanding, not to replace the lessons.

### Step 3 — Locate This Week's District on the City Map

Open the CyberFoundations City map (introduced in Week 1, Lesson 6). Identify which district corresponds to Module 1 — Digital Infrastructure & CLI.

**District name:** Foundry District

```
The CyberFoundations City map introduced in Week 0, Lesson 6 shows all the districts. The Foundry District represents Digital Infrastructure & CLI.
```

**Why this district fits this week's topics (1–2 sentences):**

```
The Foundry District focuses on computer hardware, operating systems, the command line (CLI), and virtualization, which are the main topics of Module 1. It discusses the Cybersecurity Landscape, What's Inside a Computer, and Operating Systems at a Glance.
```

---

## Part B — Hardware, OS, and Software Diagram

A computer is a stack of layers: physical hardware at the bottom, an operating system managing that hardware in the middle, and the software you actually use on top. This part has you draw that stack and explain it in your own words.

### Step 1 — Identify the Layers

Before drawing anything, name one example of what lives at each layer.

**Hardware layer — one example component (e.g., CPU, RAM, or storage):** RAM

```
RAM holds whatever the computer is actively working on (data in motion), such as open programs, open files, and whatever is on screen. Once the device is turned off, the RAM is cleared.
```

**Operating system layer — name an OS (e.g., Windows, Linux, or macOS):** Windows

```
The operating system communicates between the hardware and the software. When I print a document, it asks Windows, and Windows handles the printer. It works the same way in reverse: when I scan something, Windows takes the image from the printer and saves it into my scan folder so I can open it with a program or move it to another storage area.
```

**Software layer — one example application (e.g., a web browser or word processor):** web browser

```
The software layer is made up of applications, which we can't physically touch. I use Chrome to browse the web, check email, and shop online. Chrome doesn't control the hardware itself. When I download a file, it asks Windows to save it to storage.
```

### Step 2 — Sketch Your Diagram

Sketch a simple diagram (hand-drawn and photographed, or built in any digital tool) showing how the hardware, OS, and software layers stack and interact. Arrows or labels showing "what talks to what" matter more than visual polish. If you'd like a free browser-based option instead of hand-drawing, try [draw.io](https://www.drawio.com/) — no account required to get started.

### Step 3 — Upload and Embed Your Diagram

This step happens directly on GitHub, not through this worksheet — there's no upload field here, since screenshots and diagrams are added through GitHub's own upload UI, the same way as every other week.

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-02/`.
2. Click **Add file → Upload files**, then drag in your diagram image (name it something descriptive, like `hardware-os-software-diagram.png` — no spaces, lowercase, no timestamps).
3. Commit the upload.
4. Click on the uploaded image to open it, then click the **Raw** button. Copy the URL from your browser's address bar.
5. After you submit this worksheet, it will be committed to your repo. Go back to GitHub, open the committed file, click the pencil (edit) icon, and paste your raw URL into the embed line below:

```markdown
![Hardware/OS/software diagram](paste your raw image URL here)
```

**My Diagram** (added directly on GitHub after you submit):

### Step 4 — Explain Your Diagram

In your own words — not a copied definition — explain how the three layers interact. Reference your own diagram directly.

**Explanation (minimum 3 sentences):**

```
Software is the apps I use, like Chrome, but Chrome can't control the hardware by itself. It asks the operating system, and Windows handles the hardware, like saving my file to storage. The same thing happens in reverse when I scan something: the printer sends it to Windows, and Windows puts it where Chrome can open it.
```

---

## Analysis Questions

Answer each question in your own words. These questions connect what you did in Parts A and B to the bigger picture of this course.

### Analysis Question 1

If the operating system crashed on the computer you diagrammed, which layer(s) would stop working, and which (if any) would keep working? Explain your reasoning.

```
The software stops working. Chrome, Word and other apps have nothing holding them up. Therefore, they shut down too. The hardware keeps going since there is power. The fan spins, the drive holds the file, and software running should open up again. The RAM would restart if the computer reboots itself. 
```

### Analysis Question 2

Pick one piece of software you use daily. Trace it down through the OS to the hardware it ultimately depends on. What would happen to that software if the hardware layer failed?

```
Chrome is the software I use every day. I run my online shops through it, and I use Google Docs and Canva inside it. When I download a file, Chrome doesn't touch the drive itself; it asks Windows, and Windows saves it to storage.

If the hardware fails, Chrome stops too. Anything I already downloaded is safe on the drive, but anything only open in a tab is gone after a restart.
```

### Analysis Question 3

Explain, in your own words, why a cybersecurity professional needs to understand all three layers — hardware, OS, and software — rather than just the software layer where most visible attacks (like phishing emails) happen.

```
Most attacks people see happen at the software layer, like phishing. But attackers don't stop there. If someone gets into the operating system, they can hide from the security software running on top of it. If they get into the hardware, wiping the computer won't remove them. A cybersecurity professional has to understand all three layers to know where an attack is actually coming from, rather than only seeing the top layer.
```

---

## Lab Report Questions

Answer each question in complete sentences.

**1. What is the cybersecurity landscape, and why does it matter to someone starting this course?**

```
The cybersecurity landscape is everything that can be attacked: devices, accounts, software, people, and processes. It matters at the start of a course because you can't protect something until you know what you're protecting and where the openings are. It also keeps changing, so the landscape you learn today isn't the same one you'll defend later.
```

**2. Which CyberFoundations City district did you identify in Part A, and how does its theme connect to the hardware/OS/software material in Part B?**

```
Honestly, I really do not understand this one. The Foundry District is Week 1, and it deals with Digital Infrastructure, and this is included in Part B. Part A introduced all the districts.
```

**3. Of the three layers (hardware, OS, software), which one do you think is hardest to secure, and why?**

```
I think software is the hardest to secure. There are only a few operating systems, but a computer runs dozens of applications from dozens of different companies. Every one is a possible way in, and they all need their own patches. Users also install software themselves, so security teams don't always know what's running. Hardware is harder to attack in the first place, and the OS gets steady updates from one vendor.
```

---

## Submission Checklist

- [x] Lab Portal Module 1 orientation completed

- [x] District identified and explained

- [x] Hardware, OS, and software layer examples listed

- [x] Diagram uploaded to `assets/screenshots/week-02/` via GitHub and embedded directly in the committed file

- [x] Diagram explanation written in your own words (minimum 3 sentences)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] All three Lab Report Questions answered in complete sentences

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
