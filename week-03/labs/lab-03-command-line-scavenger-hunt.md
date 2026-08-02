# Week 3 Lab 03 — Command Line Scavenger Hunt (CLI Simulator)

**Student Name:** Katyya Moses

**Date Completed:** 8/1/2026

**Module:** 1 — Digital Infrastructure & CLI | **Week:** 3  
**Submission Path:** `week-03/labs/lab-03-command-line-scavenger-hunt.md`

---

## Overview

Labs 01 and 02 walked you through each command step by step. This lab is Week 3's wrap-up challenge: a deeper, more independent folder structure with three hidden files to track down, using the navigating and reading commands from Lessons 3A/3B, the creating and organizing commands from Lesson 3C, and your own judgment about when to ask for help. There's less hand-holding here on purpose — this is your chance to prove to yourself that the blinking cursor from the start of Lesson 3A doesn't intimidate you anymore.

**Nothing here can break anything real.** Same consequence-free CLI Simulator as Labs 01 and 02. Getting "lost" in the folder tree costs you nothing but a few extra `cd` moves.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | CyberFoundations CLI Simulator (browser-based, inside the Lab Portal) |
| Shell | Your choice — bash or PowerShell |
| Prerequisite | Labs 01 and 02 completed |

**Before you start:** log into the Lab Portal, open **Week 3 → CLI Simulator**, and load the **"Foundry District Archive Room"** scenario. This tree goes several folders deeper than Labs 01 and 02, and includes a few similarly-named folders on purpose — read carefully before you `cd` into anything.

---

## Part A — The Hunt

Find all three of the following, hidden at different depths in the Archive Room tree:

- A file related to a **shift log**
- A file related to a **maintenance note**
- A file related to a **supply inventory**

For each one, use `pwd`/`Get-Location` and `ls`/`dir` as many times as you need while you search, then record the **full path** once you find it.

Shift log file — full path once found:

```
archivist@archive-room:/home/archivist$ pwd
/home/archivist
archivist@archive-room:/home/archivist$ ls
README.md  operations  records
archivist@archive-room:/home/archivist/operations$ cd operations
archivist@archive-room:/home/archivist/operations$ ls 
ops-log  ops-notes
archivist@archive-room:/home/archivist/operations/ops-log$ cd ops-log
archivist@archive-room:/home/archivist/operations/ops-log$ ls
shift-log.txt
archivist@archive-room:/home/archivist/operations/ops-log$ cat shift-log.txt
Shift Log - Foundry District Archive Room
07:00 - Archive opened, no incidents overnight.
15:00 - Routine filing complete.
archivist@archive-room:/home/archivist/operations/ops-log$

```

Maintenance note file — full path once found:

```
archivist@archive-room:/home/archivist$ pwd
/home/archivist
archivist@archive-room:/home/archivist$ ls
README.md  operations  records
archivist@archive-room:/home/archivist/records$ cd records
archivist@archive-room:/home/archivist/records/records-2025$ cd records-2025
archivist@archive-room:/home/archivist/records$ ls
records-2024  records-2025
archivist@archive-room:/home/archivist/records/records-2025$ cd records-2025
archivist@archive-room:/home/archivist/records/records-2025$ ls
maintenance-note.txt
archivist@archive-room:/home/archivist/records/records-2025$ cat maintenance-note.txt
Maintenance Note - Conveyor belt 3 serviced, next check due in 90 days.
```

Supply inventory file — full path once found:

```
archivist@archive-room:/home/archivist$ pwd
/home/archivist
archivist@archive-room:/home/archivist$ ls
README.md  operations  records
archivist@archive-room:/home/archivist/records$ cd records
archivist@archive-room:/home/archivist/records$ ls
records-2024  records-2025
archivist@archive-room:/home/archivist/records/records-2024$ cd records-2024
archivist@archive-room:/home/archivist/records/records-2024$ ls
supply-inventory.txt
archivist@archive-room:/home/archivist/records/records-2024$ cat supply-inventory.txt
Supply Inventory - Q4 2024
Gloves - 400 units
Masks - 250 units
Tape - 60 rolls
```

---

## Part B — Read and Report

For each of the three files you found in Part A, use `cat`/`type` to read it and record what it says.

Shift log contents:

```
Shift Log - Foundry District Archive Room
07:00 - Archive opened, no incidents overnight.
15:00 - Routine filing complete.
```

Maintenance note contents:

```
Maintenance Note - Conveyor belt 3 serviced, next check due in 90 days.
```

Supply inventory contents:

```
Supply Inventory - Q4 2024
Gloves - 400 units
Masks - 250 units
Tape - 60 rolls
```

---

## Part C — Organize Your Findings

Now that you've located and read all three files, clean up after yourself the way a professional would — don't leave your findings scattered across the tree.

### Step 1 — Create a Sorted-Findings Folder

Create a new folder called `sorted-findings` in your home directory.

Command you ran:

```
archivist@archive-room:/home/archivist$ mkdir sorted-findings
archivist@archive-room:/home/archivist$ ls
```

### Step 2 — Move All Three Files Into It

Move the shift log, maintenance note, and supply inventory files — the same three you found in Part A — into `sorted-findings`.

Commands you ran:

```
mv operations/ops-log/shift-log.txt sorted-findings/
```

### Step 3 — Confirm the Move

List the contents of `sorted-findings` to confirm all three files are now there.

Command you ran:

```
archivist@archive-room:/home/archivist$ cat sorted-findings
```

Output:

```

Shift Log - Foundry District Archive Room
07:00 - Archive opened, no incidents overnight.
15:00 - Routine filing complete.
```

---

## Part D — When You Get Stuck

At some point in the Archive Room, you'll likely run across a command or folder name you don't immediately recognize.

### Step 1 — Ask the Terminal

When that happens, use `--help`, `man`, or `Get-Help` instead of guessing. Record what you looked up and what you learned.

Command or term you looked up:

```
morgan@foundry:/home/morgan/archive$ help
morgan@foundry:/home/morgan/archive$ ls
```

What the help text (or the folder's contents) told you:

```
README.md  

Archive Room
```

### Step 2 — Describe a Wrong Turn

Everyone takes at least one wrong turn in a tree this size. Describe one moment you ended up somewhere unexpected, and how you used `pwd`/`Get-Location` and `cd ..` to recover.

```
At one point I ran cd .. and ended up in /home instead of my /home/archivist folder. I didn't realize it at first — I tried to list a folder called records and got "No such file or directory," which confused me because I knew that folder existed. I ran pwd to check where I actually was, and it showed /home. That's when I saw I had gone up one level too far. I used cd archivist to go back down into my home folder, and then ls worked again and showed my folders. It taught me that when a folder I know exists suddenly can't be found, the first thing to check is pwd — because I'm probably not standing where I think I am.
```

---

## Analysis Questions

### Analysis Question 1

Which of the three files in Part A took the longest to find, and what was it about the tree's structure (depth, similarly-named folders, etc.) that made it harder?

```
The shift-log.txt file took the longest to find. What made it hard was that the folders had very similar names — there was an op-log folder and an ops-log folder that were only one letter apart, so it was easy to type the wrong one. The tree also went several levels deep, so I had to use ls one folder at a time to find the real path before I could reach the file.
```

### Analysis Question 2

Compare how you felt starting this lab to how you felt at the very start of Lesson 3A, looking at a blank blinking cursor for the first time. What changed?

```
What made it hard at first wasn't the commands or fear of breaking something. Tonia explained that we couldn't break it, so I knew it was just for practice. The real thing that threw me was that I was looking for an actual terminal. In a prior course, the instructor worked in a real command line, so I was expecting the same thing. I scanned the page, didn't see a separate terminal, and got lost. I went into "SOS mode" instead of investigating. Once I realized the simulator on the page was the terminal, it made sense. So what changed is that I learned to look at what's actually in front of me instead of expecting it to match what I'd seen before.
```

### Analysis Question 3

Week 4 moves from managing your own files to controlling who's allowed to do what to them — permissions — plus your first look at what a virtual machine is. Based on everything you've practiced this week, what's one thing you're curious about or looking forward to?

```
Honestly, everything interests me! I'm a novice and haven't taken a legitimate tech course since college, so it feels good to be working in something I love. I'm happy to learn whatever builds a good foundation. I'm especially looking forward to the permissions piece, because I'm actually studying cybersecurity exam, and permissions are a big part of that. It will be good to see a practical version of what I have been learning in isolation. Seeing a virtual machine and how permissions are set up will give me a visual that helps the material actually click more.
```

---

## Submission Checklist

- [x] All three target files located, with full paths recorded (Part A)

- [x] All three target files read and their contents recorded (Part B)

- [x] `sorted-findings` folder created and all three files moved into it, confirmed with a listing (Part C)

- [x] At least one command or term looked up with `--help`/`man`/`Get-Help`, with what you learned recorded (Part D, Step 1)

- [x] One wrong-turn moment described, including how you recovered (Part D, Step 2 — minimum 2 sentences)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-03/labs/lab-03-command-line-scavenger-hunt.md`

---

## GitHub Commit Subsection

Same mechanism as Labs 01 and 02: fill out this lab's worksheet in the **CyberFoundations Lab Portal** (Week 3 → Lab 03) and click **Submit to GitHub** — the Portal commits the completed file to `week-03/labs/lab-03-command-line-scavenger-hunt.md` automatically. No manual typing or commit needed.

**📌 Optional:** a CLI Simulator session screenshot can be added the same way as Labs 01 and 02 — upload to `assets/screenshots/week-03/`, then right-click the uploaded image and choose **Copy image address**/**Copy Image Link** to embed it — but it isn't required and won't affect your grade.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
