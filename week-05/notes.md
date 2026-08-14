# Week 5 Notes — The Grid: Addresses, Names, Ports, and Diagnostics

**Student Name:** Katyya Moses

**Date Completed:** 8/14/2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- IP addresses — the dotted-quad number every device on a network needs (`10.20.5.42` on The Grid)
- The subnet mask — the answer to "which addresses are my neighbours?" (`/24` = `255.255.255.0`)
- The default gateway — the door out of your neighbourhood (`10.20.5.1` on The Grid)
- Private vs public addresses — `10.x`, `172.16–31.x`, and `192.168.x` are *inside* addresses
- DNS — the Grid's Directory Board: a name goes in, an IP address comes out
- NXDOMAIN vs a host that resolves but is down — two different failures with two different causes
- DHCP — the Address Office: leases, why addresses change, why a laptop "just works" on a new network
- Ports — the numbered doors on a building: 22 SSH, 53 DNS, 80 HTTP, 443 HTTPS, 3389 RDP, 25 SMTP
- TCP vs UDP — a confirmed conversation vs a shout across the room
- The TCP handshake — SYN → SYN-ACK → ACK (packets 7, 8 and 9 in Lab 03)
- The diagnostic toolkit — `ping` (is it alive?), `traceroute` (where does it stop?), `dig` (what number is behind that name?)
- **THE LADDER RULE** — check yourself → check your gateway → check the target by NAME → check the target by IP → trace the path. *Work outward, one rung at a time, and let the evidence pick the culprit.*

## My Command Table

You learned the same five jobs twice this week — once in bash, once in PowerShell. Fill the pairs in from memory if you can, and check them afterwards. This table is worth keeping.

The bash command and its PowerShell equivalent for each job — show my own address, show my default gateway, test reachability, trace the path, look up a name:

```
show my own address (bash) ip addr  (PS) Get-NetIPAddress
show my default gateway (bash) ip route   (PS) Get-NetRoute
test reachability  (bash) ping   (PS) Test-Connection
Trace the path  (bash) traceroute  (PS) tracert
lookup name (bash) dig   (PS) Resolve-DnsName
```

## In My Own Words

Your machine has three numbers: an address, a subnet mask, and a default gateway. Explain what each one is for, the way you'd explain it to someone who has never heard those words.

```
Your IP address is like your street address. It shows where your machine lives on the network, like where your house sits in a neighborhood.

The subnet mask tells your machine where its neighborhood ends, so it knows which addresses are close neighbors and which ones are outside it.

The default gateway is like the exit door. When your machine wants to reach the internet, it sends everything through the gateway to get out.
```

What does DNS actually do? Include the difference between a name that comes back "Name or service not known" (NXDOMAIN) and a name that resolves perfectly well to a host that never answers.

```
DNS takes a name and turns it into a number so your machine can find it on the web. There are two ways it can fail, and they're kind of opposite. When you get "Name or service not known," that means DNS doesn't have that name at all. So you never even get a number back because the name just isn't there. But when a name works fine and still doesn't answer, that means DNS gave you a real address, but the machine at that address is down or just not responding. 
```

An IP address gets your traffic to the right building. What does a port number add to that, and why would a defender care how many doors are open?

```
An IP address gets your traffic to the right building, which is the right machine. The port number specifies which door of that building to go to, because one machine runs a lot of services at once, and each one has its own door. For example, port 443 is the secure HTTPS door, port 80 is the plain HTTP door, and port 3389 is remote desktop. Based on what needs to be accomplish, the appropriate door would be granted.

As part of cybersecurity, our goal is to limit our attack surface. So a defender cares how many doors are open because we only want the ports that need to be open actually open, and all the rest closed. Every open door is another way an attacker could try to get in, so closing the ones you don't need helps limit the risk.
```

Write out THE LADDER RULE — all five rungs, in order — and say why running them in that order matters more than running them fast.

```
The Ladder Rule should always be followed every time, because it keeps you from guessing. 

Rung 1: check yourself with ipconfig or ip addr to validate you have a good address. 
Rung 2: ping your gateway to see if it's healthy and you can get out of your own network. 
Rung 3: ping the target by name and check for packet loss. 
Rung 4: ping the target by its number (the IP address) to see if the number works when the name doesn't. 
Rung 5: run a traceroute to the host to follow the path and see where it breaks.

Running them in this order matters more than running them fast, because it helps limit mistakes. Going in order lays everything out step by step, starting closest to you and working outward, so you can actually see where the problem is instead of jumping to a guess.
```

What is DHCP, and why does your laptop get an address automatically on a network it has never joined before, while a server like `grid-dns` keeps the same address permanently?

```
DHCP is the invisible setup that happens when you join a new network. It automatically hands your device an IP address, the gateway to get out to the internet, and the DNS server to look up names, so you don't have to set any of it up yourself. This is why your laptop gets an address automatically at a cafe, a friend's house, or a hotel. That kind of address is dynamic, which is like a lease, because it's temporary and can change. A server like grid-dns keeps the same address permanently because it's static, meaning fixed. Servers need a fixed address so other machines can always find them at the same spot, but a laptop is fine with a leased one since it's always joining different networks.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I completed the bash-to-PowerShell command table

- [x] I answered all five "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-05/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
