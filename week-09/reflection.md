# Week 9 Reflection - Vault Exchange Digital Trust

**Student Name:** Kay Moses

**Week:** 9

1. What clicked for you this week?

```text
Seeing a real certificate for the first time instead of just a screenshot. Reading the actual fields and the chain made it click that "valid" means the browser checked the name, the dates, and traced the chain up to a root it already trusts.
```

2. What's still confusing?

```text
How the browser actually verifies the signatures mathematically. I could read the names in the chain, but I'm still fuzzy on what's happening under the hood when it checks each signature.
```

3. How does this week's material connect to a cybersecurity career path you're interested in?

```text
Certificates and trust chains are core to identity and access management and cloud security, which are the paths I'm aiming for. Knowing how a system proves its identity and how trust is established is directly part of IAM and securing cloud services.
```

4. One thing you would tell a friend just starting this course:

```text
Don't panic when it gets technical. A lot of it is "find the button, read the field" once someone shows you where to look. Ask for the real thing to be shown, not just a slide.
```

## Professional Growth Check

- [x] I can explain why a working key’s label does not prove who owns it.

- [x] I can read the digital badge and describe the list of signing offices I actually saw.

- [x] I can explain the service’s key, its badge application, the office’s key, and the finished badge.

- [x] I can explain why correct information passed and why an intentionally wrong input was refused.

- [x] I can tell the difference between a service not answering and its badge failing a check.

- [x] I can share useful screenshots while keeping private keys and passwords secret.

## Portfolio Deliverable 3 Reflection

Write 5–7 sentences. What can a correctly checked digital badge tell you about the service? What can it NOT promise? Describe one problem using the message you actually saw, and explain what you checked next. Compare the real website with your practice service. You may start with “I used to think…”, “My check showed…”, and “I now know…”.

```text
My check showed that a valid certificate really means something more specific: the site proved it's example.com, the certificate is inside its valid dates, and the chain traces up to a root my browser already trusts. It can tell me who I'm connected to and that the connection is encrypted. It cannot promise that the website's content or offers are honest or safe, since even a scam site can have a valid certificate. When I looked at the chain, I saw the root signs itself, which showed me trust doesn't come from the certificate but from my browser's trusted list. I now know the difference between reading a badge and actually trusting it, and I know I checked the name and dates myself but did not verify the signatures or check for revocation.
```
