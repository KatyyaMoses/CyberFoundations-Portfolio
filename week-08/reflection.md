# Week 8 Reflection — Practical Cryptography

**Student Name:** Kay

**Date:** 9/12/2026

Answer each question in 3–5 sentences.

1. How are encryption and hashing different?

```text
Encryption scrambles data so it can be unlocked and read again later with the right key. It works both ways. Hashing only goes one way. It makes a fingerprint of the data that can't be turned back into the original. Encryption protects privacy, while hashing checks whether something changed.
```

2. How are symmetric and asymmetric cryptography different?

```text
Symmetric uses one single key to both lock and unlock the data, so both sides need that same key. Asymmetric uses a pair, a public key and a private key, where one locks and the other unlocks. Asymmetric is what makes things like SSH keys work without sharing a secret.
```

3. Why does a private key need stronger protection than a public key?

```text
Because the private key is what actually proves your identity. If someone gets it, they can pretend to be you. The public key can't do that. It's only used to verify or to lock data, so sharing it causes no harm.
```

4. What changed between Week 6 password-based SSH and Week 8 key-based SSH? What stayed the same?

```text
What changed is how I proved who I was. In Week 6 I logged in with a password, and in Week 8 I used a key pair instead, which is harder to guess or steal. What stayed the same is that SSH was still the tool making the secure connection, and the goal was still logging in safely to a remote machine.
```

5. Which Week 8 task felt most connected to a real cybersecurity job, and why?

```text
Generating the SSH key pair felt the most real to me. Setting up secure key-based login is something companies actually rely on to protect their servers, and it showed how a real analyst controls who is allowed to connect without handing out passwords.
```

6. What question do you have about certificates, identity, or trust before Week 9?

```text
My question is what happens when a certificate expires or gets stolen. Does everything that trusted it stop working right away, and how does a system know to stop trusting it?
```
