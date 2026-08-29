# Week 7 Notes — Cloud Heights: The Guard Post

**Student Name:** Katyya Moses

**Week:** 7

## Firewall and Security Group

```text
An NSG (Network Security Group) is a list of firewall rules that decide what traffic can reach a machine. It works like a security guard checking a list. Inbound and outbound are two separate lists, so allowing traffic in doesn't allow anything out.
```

## Rule Anatomy

```text
(priority, direction, source, destination, protocol, port, action)
```

## First Match Wins

```text
Every rule answers these: priority is the order it's read, direction is in or out, source is where traffic comes from, destination is where it's going, protocol is TCP/UDP/ICMP, port is the service being reached (22 is SSH, 8080 is web), and action is Allow or Deny.
```

## Least Privilege

```text
Rules are read from the lowest number up. The first rule that matches the traffic wins, and everything below it is skipped. It's not about which rule is stricter, just which one comes first. This is why a Deny at 300 beats an Allow at 400, and why my Allow at 300 beats the protected deny at 1000.
```

## Testing and Evidence

```text
Give the smallest amount of access that still does the job. My rule allowed only one host (10.60.6.4) to one port (8080), not the whole subnet or Any. Even though a wider rule would pass the same test, the narrow one is safer because it leaves fewer ways in.
```

## Troubleshooting and Remediation

```text
If a test fails, check two things: is the service running, and does the rule allow the traffic. SERVICE_NOT_LISTENING means the Python server isn't on, so start it again. If the wrong source gets in, check the Source field on the rule to make sure it's the right IP. Don't touch the protected rules while troubleshooting.
```

## Questions I Still Have

```text
Is there a way to test a rule without starting a listener each time?
```
