# Week 6 Notes — Cloud Heights: Cloud VMs, SSH, VNets & Layers

**Student Name:** Katyya Moses

**Date Completed:** 8/21/2026

Summarize this week's key concepts in your own words — not copy-pasted definitions. This week moved from the simulated Grid into a real cloud environment, so focus on what you personally observed as well as what each term means.

> **Cloud Heights Security Rule:** Your Bastion shareable link and Cloud Heights password are private access credentials. Never paste either into this file, a screenshot, your GitHub repository, Circle, or a chat message.

## Key Concepts This Week

- **Cloud** — other people's computers, professionally operated and reached over a network
- **Datacenter** — the physical facility where cloud computing equipment lives
- **Region** — a geographic area where a cloud provider operates datacenters
- **Virtual machine (VM)** — a computer created in software; in Cloud Heights, your VM runs on hardware in a real datacenter
- **IaaS / PaaS / SaaS** — different levels of cloud service: rent the room, rent the workshop, or rent the finished service
- **Shared responsibility model** — the cloud provider secures the building and underlying platform; the customer is still responsible for what belongs to them
- **Provisioning** — creating and preparing a resource so it is ready to use
- **Golden image / snapshot** — a known starting point that can be used to create consistent machines
- **Snapshot vs backup** — a snapshot is a point-in-time copy used for recovery or cloning; a backup is a separate recovery copy with a different purpose
- **Azure Bastion** — the guarded front desk that gives you browser-based SSH access without giving your VM a public IP
- **Bastion shareable link** — sensitive access information that must never be committed to GitHub or exposed in screenshots
- **SSH (Secure Shell)** — remote command-line access to another machine
- **SSH client and server** — the client starts the connection; the server listens and answers
- **Port 22** — the standard numbered door used by SSH
- **Host / fingerprint verification** — the verify-before-approve habit when connecting to a host for the first time
- **Authentication** — proving that you are the account you claim to be
- **Remote session / remote shell** — the live command-line session running on another machine
- **Getting TO vs getting INTO a machine** — network reachability and authentication are different problems
- **`hostname`** — asks which machine you are on
- **`whoami`** — asks which account you are using
- **`pwd`** — asks where you are in the filesystem
- **Private IP address** — an address used inside a private network rather than directly on the public internet
- **Virtual network (VNet)** — the private cloud neighborhood where resources communicate
- **Subnet** — a smaller address range inside a VNet; a floor inside the larger building
- **NAT / outbound translation** — lets a privately addressed machine communicate outward without giving the machine its own public IP
- **Network Security Group (NSG)** — the network guard post that controls what traffic is allowed; you take control of these rules in Week 7
- **Known-good reference point** — a target whose expected behavior gives you something reliable to compare against
- **Grid Beacon** — the known-good Cloud Heights host at `10.60.6.4`
- **The silent Azure gateway** — Azure's default gateway may not answer ICMP ping even when the network is healthy
- **OSI model** — the seven-layer vocabulary used to organize network and application behavior
- **TCP/IP model** — the more compact layer model commonly used by practitioners
- **Layers** — a way to separate different jobs in a communication path so troubleshooting can be systematic
- **Encapsulation** — information travelling inside other information, like a letter inside an envelope inside a mailbag
- **The Ladder Rule in the real cloud** — work outward, prove what works, use the route and a known-good target, and never let one silent tool response choose the culprit by itself

## My Cloud Heights Command Table

You used these commands on a real Ubuntu machine this week. Instead of memorizing syntax, write down the **question each command answers** or the job it performs.

| Command | What question does it answer / what does it do? |
| --- | --- |
| `hostname` | What machine am I on? Shows the machine's name |
| `whoami` | Who am I logged in as? Shows your username |
| `pwd` | Where am I in the filesystem? Shows your currecnt folder |
| `ip addr` | What is my address? Shows my IP and subnet on eth0 |
| `ip route` | How do I get off my own network? Shows my default gateway |
| `ping` | Does the machine answer? Tests if a host reponds |
| `traceroute` | What path does my traffic take to get there? Shows each hop |
| `dig` | What address does this name map to? Looks up the DNS/ name resolution |
| `curl` | Does the web page or service actually respond? Fetches from a web address |
| `ssh` | Can I log in to another machine and get s session? Connects into a remote machine |
| `exit` | Leave the session and go back to where I was  It Closes the SSH session |

## In My Own Words

### 1. Getting TO vs Getting INTO

Explain the difference between getting **TO** a machine and getting **INTO** a machine. Use something you personally observed in Cloud Heights as evidence.

```
Getting to a machine means you reached it across the network. That just proves the machine is there and answering. Getting into a machine is different, because that means you have to log in. You have to get a session, the SSH has to be working, and you have to know the password. So you need permission to move forward. Reaching it is not the same as being let in.
```

### 2. The Silent Gateway

Your Azure gateway did not answer `ping`, but your VM was still healthy. Explain how you proved the network was working and what this taught you about interpreting tool output.

```
It proved the network was working because other traffic still went out and came back, like my name lookup and my beacon session. That showed the path was fine, since all of that had to travel through the same gateway. It taught me that a tool going quiet does not mean something is broken. The gateway just does not answer ping on purpose, so silence is not the same as failure.
```

### 3. Private on the Inside, Connected to the Outside

Explain how your Cloud Heights VM can reach the internet even though it has only a private IP address. Then explain how **you** reach the VM from outside its VNet.

```
It can reach because of NAT (Network Address Translation). When my Cloud Heights VM reaches the internet with a private address, NAT swaps my private address for a public one to surf the World Wide Web. Then it swaps it back when the reply comes from the public internet. To reach the VM from outside, you do not use a public address, because it does not have one. You come in through the Azure Bastion link in the browser, which is the one guarded entrance.
```

### 4. VNet vs Subnet

Explain the difference between a VNet and a subnet using the Cloud Heights building/floor analogy. Then explain why separating systems into smaller network ranges can help security.

```
A VNet is like the whole building, and a subnet is like one floor inside it. The VNet is the big private space, and each subnet is a smaller piece of it. Dividing it up into smaller ranges is called segmentation, and it helps with security. If there is trouble, like some kind of attack, it stays contained inside that one subnet instead of spreading to the whole building. So keeping things separated means a problem on one floor cannot easily reach the others.
```

### 5. The Ladder Rule Has a Map Now

The Ladder Rule never used the words OSI or TCP/IP. Explain how the layer models give you a map for the same troubleshooting process you have already been using.

```
The Ladder Rule always said to start with the nearest thing and work outward, and the layer models give that same process a map. The layers are stacked from the bottom up, so starting at the bottom and climbing is the same as checking the near rung first. The OSI and TCP/IP models just name each step, so instead of "near to far" I can now say which layer I am checking. It is the same troubleshooting I was already doing, just with labels on each rung.
```

---

## Submission Checklist

- [x] I summarized the Week 6 concepts in my own words, not copied definitions

- [x] I completed my Cloud Heights command table

- [x] I explained getting TO vs getting INTO a machine

- [x] I documented what the silent Azure gateway taught me

- [x] I explained the Cloud Heights private-network design

- [x] I connected the Ladder Rule to network layers

- [x] I checked that my Bastion shareable URL does not appear anywhere in this file

- [x] I checked that my Cloud Heights password does not appear anywhere in this file

- [x] This file is committed to my portfolio repo at `week-06/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
