# Week 6 Lab 03 — The Grid, For Real

**Student Name:** Kay Moses

**Date Completed:** 8/20/2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-03-the-grid-for-real.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

In Week 5 you ran `ip addr`, `ip route`, `ping`, and `traceroute` in a simulator that always behaved. Today you run the same toolkit against real cloud infrastructure that does **not** always behave the way the textbook implies — and you learn to tell "broken" apart from "normal."

This is an **independent** lab. It tells you what to accomplish; you choose the commands. Expect about 40 minutes.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Commands used | `ip addr`, `ip route`, `ping`, `traceroute`, `curl` |
| Known-good target | **Grid Beacon — `10.60.6.4`** |
| Prerequisite | Week 6 Labs 01–02 |

---

## Part A — Where You Actually Are

### Step 1 — Read Your Own Address

Run the command that lists your interfaces and addresses.

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
3: enP17552s1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc mq master eth0 state UP group default qlen 1000
    link/ether 60:45:bd:4a:4a:6a brd ff:ff:ff:ff:ff:ff
    altname enP17552p0s2
```

Your private IPv4 address and prefix length:

```
10.306.34/26
```

### Step 2 — Read Your Route

Run the command that shows the routing table.

Command and output:

```
analyst@cf-student-15:~$ ip route
default via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.34 metric 100 
10.60.6.0/26 dev eth0 proto kernel scope link src 10.60.6.34 metric 100 
10.60.6.1 dev eth0 proto dhcp scope link src 10.60.6.34 metric 100 
168.63.129.16 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.34 metric 100 
169.254.169.254 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.34 metric 100 
```

Your default gateway:

```
10.60.6.0/26
```

### Step 3 — Compare to Week 5

Compare this live Ubuntu output to what the CLI Simulator produced in Week 5. What looks the same, what looks different, and what surprised you:

```
I went back to my Week 5 labs to see what we actually did, these are my findings.

What looks the same

A lot of the basic steps carried over from the Week 5 CLI Simulator to this week's live Ubuntu machine. In both weeks I had to find this workstation's IP address on the eth0 interface, and in both I had to find the default gateway. Those two tasks worked the same way and I read the answers the same way. We also confirmed the answer by pinging in both weeks. Overall the commands themselves pretty much worked the same, which told me that all the practice we did in the simulator actually paid off. It felt like taking the training wheels off, because the same commands I practiced were the ones I had to put into action for real.

What looks different

The biggest difference is that in Week 5 a lot of the work was already set up or done for us in the simulator, and this week on the live machine it wasn't. In Week 5 we had extra steps that we didn't do this week. We had to ping a name and not just an IP, we had to ask the DNS to look that name up, and then we compared pinging the actual IP address against pinging the DNS name. This week we didn't do any of the DNS part, so it was more stripped down to just the IP and the gateway on a real machine.

The other big difference this week was that there were no hints. In Week 5 the simulator would help you along if you got stuck. This week, if you didn't know a command or what it did, you had to go back to your own notes and figure it out. For example, if I forgot what ip route was, I had to look it up myself and remember that it shows the routing table. That made this week feel more like the real thing.

What surprised me

What surprised me is how much the simulator had already handled behind the scenes. On the real machine, things I expected to just work behaved differently. The gateway didn't answer ping at all, where the simulator gave cleaner and more predictable results.

The other thing that surprised me is how quickly the live machine signs you out. When I go through my notes looking for something, it times out and I have to log back in. That's totally fine, and honestly it makes sense. In a real environment, if someone walks away from a machine, it should time out on its own when it's not being actively used, for security. It just caught me off guard the first time.
```

---

## Part B — The Gateway That Does Not Answer

### Step 1 — Ping the Gateway

Ping the default gateway address you recorded. Let it run a few seconds, then stop it.

Command and output:

```
analyst@cf-student-15:~$ ping 10.60.6.1
PING 10.60.6.1 (10.60.6.1) 56(84) bytes of data.

--- 10.60.6.1 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3096ms

```

### Step 2 — Interpret It Correctly

You almost certainly got **no replies**. In Azure, the platform gateway commonly does not answer ICMP. This is **expected platform behaviour** and by itself proves nothing about whether your machine or network is broken.

Explain why "the gateway did not answer ping" is weak evidence:

```
I think "the gateway didn't answer ping" is weak evidence because a failed ping could mean two different things and I can't really tell which one from just looking at it. Either the gateway is actually down, or the gateway is fine and just isn't answering ping on purpose. Both give me the same result — 100% packet loss — so I can't tell them apart.
```

---

## Part C — The Known-Good Target

The **Grid Beacon** at `10.60.6.4` is a machine that is known to be up and known to answer. When your first probe fails, you test against something known-good before you conclude anything.

### Step 1 — Ping the Beacon

```
ping 10.60.6.4
```
Output:

```
PING 10.60.6.4 (10.60.6.4) 56(84) bytes of data.
--- 10.60.6.1 ping statistics ---
68 packets transmitted, 0 received, 100% packet loss, time 68597ms

analyst@cf-student-15:~$ ping 10.60.6.4
PING 10.60.6.4 (10.60.6.4) 56(84) bytes of data.
64 bytes from 10.60.6.4: icmp_seq=1 ttl=64 time=1.54 ms
64 bytes from 10.60.6.4: icmp_seq=2 ttl=64 time=1.22 ms
64 bytes from 10.60.6.4: icmp_seq=3 ttl=64 time=1.19 ms
64 bytes from 10.60.6.4: icmp_seq=4 ttl=64 time=1.21 ms
64 bytes from 10.60.6.4: icmp_seq=5 ttl=64 time=1.51 ms
64 bytes from 10.60.6.4: icmp_seq=6 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=7 ttl=64 time=1.15 ms
64 bytes from 10.60.6.4: icmp_seq=8 ttl=64 time=1.12 ms
64 bytes from 10.60.6.4: icmp_seq=9 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=10 ttl=64 time=0.972 ms
64 bytes from 10.60.6.4: icmp_seq=11 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=12 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=13 ttl=64 time=1.08 ms
64 bytes from 10.60.6.4: icmp_seq=14 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=15 ttl=64 time=0.974 ms
64 bytes from 10.60.6.4: icmp_seq=16 ttl=64 time=1.12 ms
64 bytes from 10.60.6.4: icmp_seq=17 ttl=64 time=1.01 ms
64 bytes from 10.60.6.4: icmp_seq=18 ttl=64 time=1.13 ms
64 bytes from 10.60.6.4: icmp_seq=19 ttl=64 time=2.20 ms
64 bytes from 10.60.6.4: icmp_seq=20 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=21 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=22 ttl=64 time=1.04 ms
64 bytes from 10.60.6.4: icmp_seq=23 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=24 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=25 ttl=64 time=1.15 ms
64 bytes from 10.60.6.4: icmp_seq=26 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=27 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=28 ttl=64 time=1.05 ms
64 bytes from 10.60.6.4: icmp_seq=29 ttl=64 time=1.12 ms
64 bytes from 10.60.6.4: icmp_seq=30 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=31 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=32 ttl=64 time=2.29 ms
64 bytes from 10.60.6.4: icmp_seq=33 ttl=64 time=1.14 ms
64 bytes from 10.60.6.4: icmp_seq=34 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=35 ttl=64 time=1.46 ms
64 bytes from 10.60.6.4: icmp_seq=36 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=37 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=38 ttl=64 time=1.12 ms
64 bytes from 10.60.6.4: icmp_seq=39 ttl=64 time=1.05 ms
64 bytes from 10.60.6.4: icmp_seq=40 ttl=64 time=1.05 ms
64 bytes from 10.60.6.4: icmp_seq=41 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=42 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=43 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=44 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=45 ttl=64 time=0.999 ms
64 bytes from 10.60.6.4: icmp_seq=46 ttl=64 time=1.04 ms
64 bytes from 10.60.6.4: icmp_seq=47 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=48 ttl=64 time=0.968 ms
64 bytes from 10.60.6.4: icmp_seq=49 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=50 ttl=64 time=0.988 ms
64 bytes from 10.60.6.4: icmp_seq=51 ttl=64 time=1.04 ms
64 bytes from 10.60.6.4: icmp_seq=52 ttl=64 time=1.99 ms
64 bytes from 10.60.6.4: icmp_seq=53 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=54 ttl=64 time=1.05 ms
64 bytes from 10.60.6.4: icmp_seq=55 ttl=64 time=1.12 ms
64 bytes from 10.60.6.4: icmp_seq=56 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=57 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=58 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=59 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=60 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=61 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=62 ttl=64 time=1.17 ms
64 bytes from 10.60.6.4: icmp_seq=63 ttl=64 time=1.16 ms
64 bytes from 10.60.6.4: icmp_seq=64 ttl=64 time=1.16 ms
64 bytes from 10.60.6.4: icmp_seq=65 ttl=64 time=1.35 ms
64 bytes from 10.60.6.4: icmp_seq=66 ttl=64 time=0.950 ms
64 bytes from 10.60.6.4: icmp_seq=67 ttl=64 time=2.04 ms
64 bytes from 10.60.6.4: icmp_seq=68 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=69 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=70 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=71 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=72 ttl=64 time=1.12 ms
64 bytes from 10.60.6.4: icmp_seq=73 ttl=64 time=2.34 ms
64 bytes from 10.60.6.4: icmp_seq=74 ttl=64 time=1.04 ms
64 bytes from 10.60.6.4: icmp_seq=75 ttl=64 time=1.06 ms
64 bytes from 10.60.6.4: icmp_seq=76 ttl=64 time=0.985 ms
64 bytes from 10.60.6.4: icmp_seq=77 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=78 ttl=64 time=1.16 ms
64 bytes from 10.60.6.4: icmp_seq=79 ttl=64 time=1.01 ms
64 bytes from 10.60.6.4: icmp_seq=80 ttl=64 time=0.948 ms
64 bytes from 10.60.6.4: icmp_seq=81 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=82 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=83 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=84 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=85 ttl=64 time=1.06 ms
64 bytes from 10.60.6.4: icmp_seq=86 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=87 ttl=64 time=1.08 ms
64 bytes from 10.60.6.4: icmp_seq=88 ttl=64 time=1.06 ms
64 bytes from 10.60.6.4: icmp_seq=89 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=90 ttl=64 time=0.980 ms
64 bytes from 10.60.6.4: icmp_seq=91 ttl=64 time=2.16 ms
64 bytes from 10.60.6.4: icmp_seq=92 ttl=64 time=1.17 ms
64 bytes from 10.60.6.4: icmp_seq=93 ttl=64 time=1.04 ms
64 bytes from 10.60.6.4: icmp_seq=94 ttl=64 time=0.978 ms
64 bytes from 10.60.6.4: icmp_seq=95 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=96 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=97 ttl=64 time=0.912 ms
64 bytes from 10.60.6.4: icmp_seq=98 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=99 ttl=64 time=0.934 ms
64 bytes from 10.60.6.4: icmp_seq=100 ttl=64 time=0.991 ms
64 bytes from 10.60.6.4: icmp_seq=101 ttl=64 time=1.12 ms
64 bytes from 10.60.6.4: icmp_seq=102 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=103 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=104 ttl=64 time=1.18 ms
64 bytes from 10.60.6.4: icmp_seq=105 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=106 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=107 ttl=64 time=1.19 ms
64 bytes from 10.60.6.4: icmp_seq=108 ttl=64 time=1.00 ms
64 bytes from 10.60.6.4: icmp_seq=109 ttl=64 time=2.94 ms
64 bytes from 10.60.6.4: icmp_seq=110 ttl=64 time=1.00 ms
64 bytes from 10.60.6.4: icmp_seq=111 ttl=64 time=1.06 ms
64 bytes from 10.60.6.4: icmp_seq=112 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=113 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=114 ttl=64 time=1.00 ms
64 bytes from 10.60.6.4: icmp_seq=115 ttl=64 time=1.00 ms
64 bytes from 10.60.6.4: icmp_seq=116 ttl=64 time=1.08 ms
64 bytes from 10.60.6.4: icmp_seq=117 ttl=64 time=1.23 ms
64 bytes from 10.60.6.4: icmp_seq=118 ttl=64 time=1.01 ms
64 bytes from 10.60.6.4: icmp_seq=119 ttl=64 time=1.05 ms

64 bytes from 10.60.6.4: icmp_seq=120 ttl=64 time=1.03 ms
64 bytes from 10.60.6.4: icmp_seq=121 ttl=64 time=0.947 ms
64 bytes from 10.60.6.4: icmp_seq=122 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=123 ttl=64 time=1.32 ms
64 bytes from 10.60.6.4: icmp_seq=124 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=125 ttl=64 time=1.05 ms
64 bytes from 10.60.6.4: icmp_seq=126 ttl=64 time=1.08 ms
64 bytes from 10.60.6.4: icmp_seq=127 ttl=64 time=1.10 ms
64 bytes from 10.60.6.4: icmp_seq=128 ttl=64 time=1.00 ms
64 bytes from 10.60.6.4: icmp_seq=129 ttl=64 time=1.04 ms
64 bytes from 10.60.6.4: icmp_seq=130 ttl=64 time=2.06 ms
64 bytes from 10.60.6.4: icmp_seq=131 ttl=64 time=2.66 ms
64 bytes from 10.60.6.4: icmp_seq=132 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=133 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=134 ttl=64 time=1.05 ms
64 bytes from 10.60.6.4: icmp_seq=135 ttl=64 time=1.01 ms
64 bytes from 10.60.6.4: icmp_seq=136 ttl=64 time=0.963 ms
64 bytes from 10.60.6.4: icmp_seq=137 ttl=64 time=1.11 ms
64 bytes from 10.60.6.4: icmp_seq=138 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=139 ttl=64 time=1.02 ms
64 bytes from 10.60.6.4: icmp_seq=140 ttl=64 time=1.06 ms
64 bytes from 10.60.6.4: icmp_seq=141 ttl=64 time=1.01 ms
64 bytes from 10.60.6.4: icmp_seq=142 ttl=64 time=1.09 ms
64 bytes from 10.60.6.4: icmp_seq=143 ttl=64 time=1.07 ms
64 bytes from 10.60.6.4: icmp_seq=144 ttl=64 time=2.31 ms
64 bytes from 10.60.6.4: icmp_seq=145 ttl=64 time=1.15 ms
64 bytes from 10.60.6.4: icmp_seq=146 ttl=64 time=3.91 ms
64 bytes from 10.60.6.4: icmp_seq=147 ttl=64 time=1.13 ms
64 bytes from 10.60.6.4: icmp_seq=148 ttl=64 time=1.07 ms
--- 10.60.6.4 ping statistics ---
148 packets transmitted, 148 received, 0% packet loss, time 147237ms
rtt min/avg/max/mdev = 0.912/1.180/3.910/0.396 ms
```

### Step 2 — Trace the Path

```
traceroute 10.60.6.4
```
Output:

```
traceroute to 10.60.6.4 (10.60.6.4), 30 hops max, 60 byte packets
 1  grid-beacon.internal.cloudapp.net (10.60.6.4)  1.160 ms  1.119 ms *
```

### Step 3 — Ask the Application

```
curl http://10.60.6.4
```
Output:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GRID BEACON | CVI CyberFoundations</title>
    <style>
        body {
            background: #071426;
            color: #d9f7ef;
            font-family: monospace;
            max-width: 850px;
            margin: 80px auto;
            padding: 30px;
        }
        .beacon {
            border: 1px solid #31d6a6;
            padding: 35px;
        }
        h1 { color: #31d6a6; }
        .label { color: #8ca8ff; }
        .status { color: #31d6a6; }
        .classified {
            margin-top: 30px;
            border-top: 1px solid #31445e;
            padding-top: 20px;
        }
    </style>
</head>
<body>
<div class="beacon">

    <h1>GRID BEACON</h1>

    <p><span class="label">NODE:</span> grid-beacon</p>
    <p><span class="label">NETWORK:</span> CVI Training Grid</p>
    <p><span class="label">STATUS:</span>
       <span class="status">ONLINE</span></p>

    <p>
        Network beacon established.<br>
        If you reached this node, your route is operational.
    </p>

    <div class="classified">
        <p>INVESTIGATION CHECKPOINT</p>

        <p>
            Observe the path that brought you here.
            The destination is only part of the story.
        </p>

        <p>TRACE ID: CF-NET-0604</p>
    </div>

</div>
</body>
</html>
```

> ### ⚠️ Grid Beacon not responding?
> The Grid Beacon is shared course infrastructure and should normally be available. First, confirm your Cloud Heights VM shows **Running** and that you completed the preceding network checks. Then retry the command once after a minute or two.
>
> If the Grid Beacon still does not respond, **stop this part of the lab and contact your instructor.** Record that the shared service was unavailable; do not treat the result as evidence that your VM or your work is incorrect.
>
> Do not change networking, NSGs, firewall rules, routes, DNS, or any Azure settings to try to reach the beacon.
>
> *Instructor note: a confirmed Grid Beacon outage is an environment issue, not a student error. Affected students may complete this portion of Lab 03 after the service is restored, with no penalty.*

### Step 4 — Record the Application Evidence

The beacon returns a banner and a trace ID. Record exactly what you received:

```
Banner: GRID BEACON 
TRACE ID: CF-NET-0604
```

Explain the difference between what the `ping` proved and what the `curl` proved:

```
Ping proves connectivity. It proves that the machine is reachable on the network. Curl proves the service is actually up and responding. It proves that an application on that machine answered you with real data. 
```

### Step 5 — Capture Your Evidence

Two screenshots, both cropped to the terminal only:

**Required filename:** `vm-toolkit-live.png` — your `ip addr` and `ip route` output

![Live VM toolkit — ip addr and ip route](https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week-06/vm-toolkit-live.png?raw=true)

**Required filename:** `beacon-reply.png` — your beacon ping/traceroute/curl evidence

![Grid Beacon reply](https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week-06/beacon-reply.png.pdf)

---

## Part D — Rewrite the Ladder Rule

Week 5 taught the Ladder Rule: test the near thing before the far thing. Real infrastructure adds a wrinkle — a silent rung is not automatically a broken rung.

Rewrite the Ladder Rule in your own words so that it survives real cloud infrastructure. Your version must include both **route/path evidence** and **a known-good target**:

```
In the cloud, the Ladder Rule means you troubleshoot in order and never guess. You start with the nearest rung and climb to the farthest, and you don't blame the far end until you've checked each rung along the way.

The catch is that a silent rung isn't automatically broken. Something might not answer even though it's fine, so silence alone doesn't prove anything.

That's why you need two things. First, route evidence: did the connection actually reach the target, through DNS, routing, and port 22? Second, a known-good target: test against something you know works. If a website I know is fine opens but mine doesn't, that points to where the real problem is.
```

---

## Analysis Questions

**Analysis Question 1.** Your ping to the gateway failed and your ping to the beacon succeeded. What does that pair of results, taken together, prove about your machine's networking? *(Minimum 3 sentences.)*

```
The ping to the beacon succeeded, which means my traffic is reaching the outside network. For that to happen, it had to pass through the gateway. So even though my ping to the gateway failed, my networking is actually fine. Therefore, the gateway ping is silent, not broken.
```

**Analysis Question 2.** Why is `traceroute` useful even when `ping` already answered? What extra thing does it show you? *(Minimum 2 sentences.)*

```
Ping only tells me if my traffic reached the target or not. Traceroute is useful because it shows me the whole path my traffic took to get there, stop by stop. So even when ping already answered, traceroute shows me the route and where any problem along the way might be.
```

**Analysis Question 3.** A service is unreachable and ping to it succeeds. Where would you look next, and why is "the network is fine" an incomplete answer? *(Minimum 3 sentences.)*

```
When ping works, it means the network can reach the machine. So the computer is on and the network is fine. But if the service is unreachable and isn't answering, the problem isn't the network, it's that program. Saying "the network is fine" is only half the story, because reaching the computer doesn't mean the program on it is running. So I'd check that program next.
```

**Analysis Question 4.** Something already controls what is allowed to reach your machine in Cloud Heights. If you could decide those rules, what would you want to allow, what would you want to block, and who in an organization should get to make that decision? *(Minimum 3 sentences.)*

```
I would allow the traffic that I actually need, like the SSH connection on port 22 so I can log in, and block everything else that has no reason to reach my machine. Blocking the extra stuff keeps it safer, because less is open to attackers. I don't think one regular employee should decide this alone. It should be someone like an admin or security team, so the rules stay consistent and the company stays protected. This follows least privilege, where you only allow what's needed and nothing more.
```

---

## Submission Checklist

- [x] `ip addr` output recorded and own private IP/prefix identified (Part A)

- [x] `ip route` output recorded and default gateway identified (Part A)

- [x] Live output compared to the Week 5 simulator (Part A, Step 3)

- [x] Gateway pinged and the silent result interpreted correctly (Part B)

- [x] Beacon `ping`, `traceroute`, and `curl` all run and recorded (Part C)

- [x] Beacon banner and TRACE ID recorded (Part C, Step 4)

- [x] `vm-toolkit-live.png` and `beacon-reply.png` captured, cropped, uploaded to `assets/screenshots/week-06/` (Part C, Step 5)

- [x] Ladder Rule rewritten with route evidence + known-good target (Part D)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-03-the-grid-for-real.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 03: The Grid, For Real** in the Lab Portal.
2. Fill in the worksheet fields and upload both screenshots to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-03-the-grid-for-real.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
