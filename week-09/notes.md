# Week 9 Notes - Vault Exchange Digital Trust

**Student Name:** Kay Moses

**Week:** 9

Use your own words. Short notes and everyday examples are welcome. You do not need to memorize commands.

## From a Key to an Identity

Ivy has two keys with the same label. Why is the label not enough? How can checking a visitor badge help explain the problem?

```text
My explanation: A label just says what a key claims to be, not what it actually is. Ivy has two keys with the same label, so the label alone can't tell you which one is the real one. It's like a visitor badge: anyone could print the right name on a badge, so the name being there doesn't prove the person is who they say. You need a trusted office to confirm the badge is real, not just read what's printed on it.
```

## Certificate Fields

A certificate is a digital badge. Explain the name list (SAN), signing office (issuer), start/end dates, public key, signature method, and allowed job (purpose). Which field would you check to see whether the badge covers the right website?

```text
Field and its job: A certificate is a digital badge with several labeled parts. The SAN is the list of website names the badge is valid for. The issuer is the office that signed it. The start and end dates say when it's valid. The public key is the site's key, and the signature method is how the issuer signed it. The purpose says what the badge is allowed to be used for, like identifying a web server. To check whether the badge covers the right website, I'd look at the SAN field.
```

## Issuers and Accepted Trust

Who signed the badge? Who chooses whether to accept the top office? Explain why an office signing its own badge is not enough to make your browser trust it.

```text
My explanation: The badge was signed by Cloudflare, and above that the chain goes up to SSL.com's root. My browser is what chooses whether to accept the top office, using its stored list of trusted roots. An office signing its own badge isn't enough because anyone can sign their own certificate. It only becomes trustworthy when the root is already on my browser's trusted list.
```

## Key CSR and Certificate Roles

The CSR is a badge application. Explain the separate jobs of the service’s private key, application, office’s private key, and finished certificate. Who signs the application? Who signs the finished badge?

```text
My explanation: The service's private key stays secret and is used to prove the site owns its key. The CSR is the application the site sends to the certificate office, containing its public key and the name it wants on the certificate. The site signs its own application with its private key to show it controls that key. The office's private key is what the CA uses to sign the finished certificate. So the site signs the application, and the certificate office signs the finished badge.
```

## TLS and Verification Evidence

Compare looking at a certificate, checking its saved file, and connecting to a running service. TLS sets up a protected connection. What did you observe in each activity?

```text
I looked at: the certificate for example.com in my browser, reading its fields and chain in the certificate viewer.
I checked: a saved certificate file
I connected to: example.com over HTTPS, where TLS set up the encrypted connection and the browser reported it as secure.
```

## Troubleshooting and Remediation

These words mean finding and fixing a problem. Record an actual message, what it meant, and your next step. Consider a wrong name, expired dates, wrong office, canceled certificate, or service that is not running. Label situations you only discussed; do not claim to have tested them.

```text
Message or situation: I did not trigger an error, so this is a situation I only discussed: a certificate with a name that doesn't match the site.
What it means: The certificate's SAN doesn't include the site I visited, so the browser can't confirm I'm on the right site.
What I would check or fix: I'd check the SAN field against the address I'm visiting, and make sure the site is using the correct certificate for its hostname.
Which check I would repeat: I'd repeat the name check, comparing the SAN list to the hostname.
```

## Questions I Still Have

```text
A word or step I want explained again: A word or step I want explained again: how the browser actually verifies a signature mathematically, since I read the names in the chain but didn't check the signatures myself.
```
