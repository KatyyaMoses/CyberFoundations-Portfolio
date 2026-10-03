# Week 11 Notes — IAM & Active Directory

**Student Name:** Katyya Moses

**Date:** October 4, 2026

Use your own words and everyday examples. These notes support your thinking; they are not your lab answers. Do not copy your lab answers here — those belong in the Cloud Heights Identity Center.

## 1. Person vs Identity vs Account

```text
A person is: A real human being. It is someone you could meet and talk to.
An identity is: How a system knows a person. One person has one identity.
An account is: The login that an identity uses. One person can have more than one account.
An everyday example that shows the difference: Maria is one person with one identity, but she can have a work account and an admin account.
```

## 2. OU vs Group vs Role vs Permission

```text
An organizational unit (OU) is: A folder that organizes accounts, like "Front Desk."
A group is: A list of accounts that get the same access. If I add someone to the group, they get what the group gets.
A role is: A named set of permissions, like "Billing Clerk."
A permission is: One single action that is allowed, like opening a file.
How they fit together in one sentence: An OU organizes accounts, a group collects them, a role bundles permissions, and a permission is one allowed action.
```

## 3. Authentication vs Authorization

```text
Authentication means: Proving who you are. A password and a code are examples.
Authorization means: Deciding what you are allowed to do after you sign in
A real-life analogy (for example, an airport or an office building): At an airport, showing my ID and boarding pass proves who I am. That is authentication. The pass says which plane I can board. That is authorization.
```

## 4. Least Privilege

```text
Least privilege means: Giving people only the access they need for their job. Nothing extra.
One place in the labs where giving more access would have been risky: At the clinic, the front desk shares one login for patient records. If that login also had admin access, anyone who found the password could change or see everything.
```

## 5. RBAC

```text
Role-based access control means: Access is given based on a person's job role. Everyone in the same role gets the same access.
One role from the labs and what it allowed: A help desk role that could reset passwords but could not change settings for the whole company.
```

## 6. ABAC

```text
Attribute-based access control means: Access depends on facts about the person or situation. Examples are department, location or time of day.
How it differs from RBAC: RBAC looks at the job role only. ABAC looks at more details, so the rules can be more specific.
```

## 7. AD DS vs Entra ID

```text
AD DS is: Active Directory Domain Services. It manages users and computers inside a company's own network.
Microsoft Entra ID is: icrosoft's cloud identity service. It manages sign-ins to cloud apps like Microsoft 365.
One similarity and one difference: Both manage user accounts and sign-ins. AD DS is mostly for on-site networks. Entra ID is for the cloud.
```

## 8. Azure Resource RBAC

```text
What Azure resource RBAC controls: Who can see, change or manage Azure resources, like virtual machines and storage.
How it relates to what I did in the simulator: I gave a user a role at a certain level. That decided what they could do with the resource. A reader role could look but not change anything.
```

## 9. Joiner / Mover / Leaver

```text
Joiner (what happens when someone starts): I create their account. I add them to the right group and give them only the access their job needs.
Mover (what happens when someone changes roles): I remove the access from their old job. Then I add the access for the new one.
Leaver (what happens when someone leaves): I turn off their account the same day. I remove their access and take back their devices
Why the leaver step matters most for security: Old accounts that stay active are easy to misuse, and nobody is watching them. The clinic lab showed this. Two staff left and the shared password never changed.
```

## 10. Sign-in Logs vs Audit Logs

```text
Sign-in logs show: Who tried to sign in, when, from where, and whether it worked.
Audit logs show: Changes made in the system. Examples are a new account, a password reset or a changed permission.
One question each log can answer: Sign-in log: Did someone sign in from a strange place at 3 a.m.? Audit log: Who added this person to the admin group?
```

## 11. Useful Terms and Questions

```text
Terms I want to review: Least privilege, ABAC vs RBAC, and the difference between Entra ID and AD DS.
Questions for my instructor: Can a company use AD DS and Entra ID together?
```
