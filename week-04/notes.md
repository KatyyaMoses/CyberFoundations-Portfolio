# Week 4 Notes — Permissions, Searching, and Virtual Machines

**Student Name:** Katyya Moses

**Date Completed:** August 5, 2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- File permissions: read/write/execute × owner/group/other, and reading `ls -l`
- Changing permissions with `chmod` (symbolic and numeric) — and THE GATEKEEPER'S RULE
- Windows ACLs, read with `Get-Acl`/`icacls`
- Wildcards (`*`, `?`, `[ ]`) and searching inside files with `grep`/`Select-String`
- Virtual machines: host vs. guest, the hypervisor, Type 1 vs. Type 2, isolation
- The VM lifecycle: create, start, stop (deallocate), snapshot, delete — and what each costs
- Golden snapshots — how your Weeks 6–12 lab machines are made

## In My Own Words

**Decode `-rw-r-----` audience by audience: who can do what to this file?**

```
The owner can read and write. That's the rw- part. The group can read only. That's the r-- part. Other has no access at all. All three are dashes, so ---, nobody outside the owner and group can touch this file.
```

**What is a hypervisor, and what are its two jobs?**

```
A hypervisor is a software that sits in between the physical hardware and the virtual machine, which is the guest. Its two jobs are dividing resources, like the CPU and the memory, and making sure everything is separated. All the other virtual machines are kept separate (isolation), and that's for security purposes.
```

**A stopped VM still costs a little money. What is it paying for, and what's the only way to reach a true zero?**

```
If the virtual machine is only stopped, the storage is still on. So you still pay a small cost for the disk that holds your files. The only way to pay nothing is to delete everything. No snapshots, no storage, nothing left. Delete it completely and the cost goes to zero.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I answered all three "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-04/notes.md`
