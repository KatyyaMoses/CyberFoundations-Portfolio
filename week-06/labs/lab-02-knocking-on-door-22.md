# Week 6 Lab 02 — Knocking on Door 22

**Student Name:** Katyya Moses

**Date Completed:** 8/19/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-02-knocking-on-door-22.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

Week 5 told you SSH is how administrators reach a machine over the network, and that it knocks on **port 22**. This week you knock yourself. You are already inside Cloud Heights through Bastion — now you will open a second, nested SSH session from your machine *to itself* and watch every step of what SSH does before it lets you in.

Starts **guided**, finishes **independent**. Expect 30–40 minutes.

**This lab uses password authentication only.** SSH keys are Week 8. Do not go looking for them yet.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Username | `analyst` |
| Password | Provided separately. Never typed into this worksheet. |
| Commands used | `ssh`, `whoami`, `hostname`, `pwd`, `exit` |
| Prerequisite | Week 6 Lab 01 completed |

**Before you start:** open **My Lab Environment**, start your VM if needed, wait for **Running**, then open Cloud Heights.

---

## Part A — Two Ways Into the Same Room

### Step 1 — Name the Path You Already Used

You reached Cloud Heights through a browser session. Something else handled the network hop for you.

Describe, in your own words, what the Bastion/browser path did on your behalf:

```
I got into my virtual machine through a browser session, so I didn't have to connect on my own. The browser did the network hop and the SSH handshake for me in the background, and it handled proving I was allowed in. Then it dropped me inside the machine, and that's when I ran hostname, whoami, and pwd to make sure I was in the right place.
```

### Step 2 — Predict the Manual Path

You are about to type an SSH command by hand. Before you run it, write what you expect to happen and what you expect to be asked for:

```
When I type the SSH command by hand (i.e. ssh maya@10.60.6.24), I expect it to connect to the machine first. The first time, I expect it to ask me to confirm the server is the right one. Then, I expect it to ask me to prove who I am, with a password or a key. Once that is accepted, I expect the door to open and put me inside the machine.
```

---

## Part B — Knocking

### Step 1 — Run the SSH Command

In your Cloud Heights terminal, run:
```
ssh analyst@localhost
```

**Stop before typing anything else.**

### Step 2 — Read the First-Connection Prompt

The first time SSH connects to a host it has never seen, it shows you the host's **fingerprint** and asks whether you want to continue connecting. This is not an error. It is SSH telling you: *I have no record of this machine yet — do you recognise it?*

Paste the prompt you saw (fingerprint line included — a fingerprint is not a credential):

```
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:lLNjUYh0vfUfSNAF9vWIFuYf0uFe+xzlR0EooZW6XTA.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

Explain why you were willing to answer `yes` here — what makes this an expected first connection rather than a suspicious one:

```
I was willing to answer yes because this was my first time connecting to this machine, so SSH had no record of it yet. That's expected on a first connection, not a sign of a problem. I was the one choosing to connect to this machine on purpose, so seeing a first-time prompt made sense. It would only be suspicious if I had connected before and suddenly got asked again like it was new.
```

### Step 3 — Enter Your Password

Type `yes`, then enter your password when prompted.

**Linux does not echo password input.** No characters, no dots, no asterisks appear as you type. The terminal looks frozen. It is not — type the password and press Enter.

What did the screen show while you typed:

```
Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.
ssh_dispatch_run_fatal: Connection to 127.0.0.1 port 22: Broken pipe
```

### Step 4 — Prove You Are in the Nested Session

Inside the new session run each of these and record the output:
```
whoami
```

```
analyst
```

```
hostname
```

```
cf-studnet-15
```

```
pwd
```

```
/home/analyst
```

### Step 5 — Notice the Prompt

Compare the prompt now to the prompt before you ran `ssh`. Describe anything that changed and anything that looks identical, and explain why it looks that way given where you connected to:

```
Comparing the prompt after ssh to local host looked basically the same. Since the machine I connected to is the same machine I started on, the prompt, the hostname, whoami, and pwd all stayed identical. There was nothing different to show because I logged into myself.
```

### Step 6 — Capture Your Evidence

Screenshot the terminal showing the first-connection prompt and the successful session.

**Required filename:** `ssh-first-connection.png`

**Crop rules.** No Bastion URL, no address bar, no password field, no login screen. The fingerprint text is fine.

![SSH first connection and nested session](https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week-06/ssh-first-connection.png?raw=true)

### Step 7 — Leave

Run:
```
exit
```
What did the prompt look like after exiting, and how do you know you are back in the original session:

```
After I ran exit, the screen showed logout, and then a message saying the connection to the remote machine was closed. I know I'm back in my original session because the prompt changed back to my own machine's name. I'm no longer inside the remote one, so I'm back where I started.
```

---

## Part C — The Deliberate Failure (Independent)

### Step 1 — Knock With the Wrong Name

Run an SSH command to `localhost` using a username that does not exist on this machine — for example `ssh notauser@localhost`. Enter anything at the password prompt.

Command you ran:

```
analyst@cf-student-15:~$ ssh notuser@localhost
```

Output:

```
notuser@localhost's password: 
Connection closed by 127.0.0.1 port 22

```

### Step 2 — Read the Failure Correctly

`Permission denied` is a **failure of authentication**, not a failure of the network.

Explain what the network and SSH already had to do successfully in order for you to be told "permission denied" at all:

```
To even get "permission denied," the network already had to reach the machine. SSH had to connect and start talking to the server. So the connection was working fine. It only failed at the last step, where I had to prove who I was. Getting told "denied" means everything before that already worked.
```

---

## Analysis Questions

**Analysis Question 1.** Distinguish *reach* from *authentication*. Which one had already succeeded when you saw a password prompt, and how do you know? *(Minimum 3 sentences.)*

```
Reach and authentication are the two locks. Lock 1 is reach, which is getting to the machine at the network level, through DNS, routing, port 22, and the TCP handshake. Lock 2 is proving who you are, which is authentication. When I saw a password prompt, Lock 1 had already succeeded, because the machine was reached and now it was asking me to prove who I am. I know that because you can't reach Lock 2 without passing Lock 1 first.
```

**Analysis Question 2.** You accepted a host fingerprint today because you knew you had just connected to your own machine. Describe a situation where accepting a fingerprint without thinking would be a real problem. *(Minimum 3 sentences.)*

```
To even get "permission denied," Lock 1 already had to succeed. The network had to reach the machine through DNS, routing, port 22, and the TCP handshake. SSH had to connect and start talking to the server. So Lock 1 was fully passed. It only failed at Lock 2, where I had to prove who I am. Being told "denied" means everything up through Lock 1 already worked, and only authentication failed.
```

**Analysis Question 3.** What changed and what stayed the same when you moved from the outer session into the nested SSH session, and why? *(Minimum 2 sentences.)*

```
Since I connected to my own machine, most of it stayed the same, like the hostname and location. What changed was that I was now in a nested SSH session instead of my first one. It looked the same because I logged into the same machine I started on.
```

**Analysis Question 4.** A colleague says "SSH is broken, I got permission denied." Using only what you learned in this lab, what would you tell them is already working, and what would you check next? *(Minimum 3 sentences.)*

```
I'd tell them SSH isn't broken, because permission denied means the connection already worked. The network reached the machine and SSH was talking to it. It only failed at proving who they are. So I'd check their login info next, like their username, password, or key.
```

---

## Submission Checklist

- [x] Bastion path vs. manual SSH path described (Part A)

- [x] `ssh analyst@localhost` run and the first-connection prompt recorded (Part B, Steps 1–2)

- [x] Password entered; non-echoing input observed and described (Part B, Step 3)

- [x] `whoami`, `hostname`, `pwd` run inside the nested session (Part B, Step 4)

- [x] Prompt change described (Part B, Step 5)

- [x] `ssh-first-connection.png` captured, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 6)

- [x] Session exited cleanly (Part B, Step 7)

- [x] Bad-username test run and `Permission denied` output recorded (Part C)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-02-knocking-on-door-22.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 02: Knocking on Door 22** in the Lab Portal.
2. Fill in the worksheet fields and upload `ssh-first-connection.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-02-knocking-on-door-22.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
