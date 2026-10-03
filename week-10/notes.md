# Week 10 Notes — Security Fundamentals and Risk

**Student Name:** Katyya Moses

**Date:** October 3, 2026

Use your own words and everyday examples. These notes support your thinking; they are not your lab answers. Do not copy clinic scenario answers here — those belong in the Demo Lab.

## 1. Assets and Business Purpose

An asset is something a business depends on. Pick an everyday example (a bakery, a gym, a school).

```text
My everyday business: Tax firm
One asset it depends on: computer
Why the business needs that asset: to collect tax documents and file cleint taxes
```

## 2. Confidentiality, Integrity, Availability

Define each goal in your own words, then describe one situation where more than one goal is affected and explain why.

```text
Confidentiality means: keeping the important info secret
Integrity means: making sure that it is not altered
Availability means: it can be accessed when needed
A situation where goals overlap, and why: A hacker gets into my tax software. They can read client tax returns (confidentiality). They could change a bank account number (integrity). They could lock me out before the filing deadline (availability). One attack can hit all three.
```

## 3. Event, Weakness, Consequence and Risk

Use your OWN non-clinic example to separate these four ideas.

```text
My example setting: I run a virtual tax firm. I work online with clients across the country. I prepare and file their returns in my tax software.
Threat / event (what could happen): A client's email gets hacked, or the client shares my video with someone else. The other person watches the video and sees the whole tax return.
Vulnerability (the weakness that lets it cause harm): The video and emailed documents leave my system. I can't control the client's email. If the video isn't deleted after it is viewed, it stays available to anyone who gets the file.
Consequence (what goes wrong if it happens): Someone could see a client's Social Security number, income and bank details. The client could face identity theft or tax fraud. I could lose their trust.
Risk (how the pieces combine into something to manage): The risk is medium. My tax software has multi-factor sign-in, and I never use public Wi-Fi. But I can't control the client's email. The harm would be big, because a tax return is very private.
```

## 4. Observed, Inferred and Unknown

Evidence you saw is different from a guess. If you did not see a safeguard, it is unknown — not proof it is missing.

```text
Something I observed directly: Clients email me documents, and I send them a video of their return when we can't meet.
Something I inferred from it: If a client's email is hacked, the hacker could see the video and documents.
Something that is still unknown: I don't know how well each client protects their email. I don't know if any client has shared the video with someone else.
How I will label a safeguard I did not see: I will write "unknown." I will not say it is missing. For example, I haven't seen a client's email settings, so I can't say they are weak.
```

## 5. Warning Signs and Safe Reporting

Suspicious is not the same as proven.

```text
Warning signs I would look for: A client asks me to send documents to a new email address. An email asks me to act fast or sends me to a link. A message looks like it's from a client but the address is slightly different. A client says they didn't send a document.
How I would safely verify without clicking or replying: I would call the client on the phone number I already have on file. I would not use any number in the email. I would not click the link or reply. If it still looks wrong, I would report it to my email provider.
Who I would report to, and how: I would report it to my email provider as phishing. If a client's account looks hacked, I would call that client and tell them. I would also tell them to change their password.
Why suspicion alone does not prove compromise: A strange email only looks wrong. It doesn't prove anyone's account was hacked. I would need to check, like calling the client, before I say anything was compromised.
```

## 6. Likelihood and Impact

Likelihood and impact are each rated 1–3. The classroom score is Likelihood x Impact. Bands: 1–2 low, 3–4 medium, 6–9 high. These are classroom judgments, not measured probabilities.

```text
What a likelihood of 1, 2 or 3 means to me: means it is unlikely, because I have strong protections in place. 2 means it could happen, but something partly stops it. 3 means it happens often and nothing stops it.
What an impact of 1, 2 or 3 means to me: 1 means a small problem and my work goes on. 2 means some disruption or limited private data is involved. 3 means client private data is exposed and recovery is hard.
Existing controls vs proposed controls, in my words: Existing controls are what I already do. My tax software has multi-factor sign-in, and I never use public Wi-Fi. Proposed controls are new things I would add, like deleting the video after it is viewed.
Why recording my reason matters more than the number: A number is just my opinion. My reason shows what evidence I used. Someone else can read it and agree or disagree.
```

## 7. Controls and Residual Risk

Use a non-clinic example.

```text
A specific control: I delete each client's review video right after they confirm they have watched it.
How it helps: The video can't be found later if a client's email is hacked. It also can't be shared after it is gone.
Risk remaining afterwards: The client could still share the video before I delete it. Documents they email me are still outside my control. The risk is lower, but not zero.
```

## 8. Talking to a Manager

```text
How I would explain a risk in plain language to a non-technical manager: Our clients send us private tax documents by email. We can't control how safe their email is. If it is hacked, someone could see those documents. I suggest we delete files once we are done with them. This lowers the risk, but we still can't control the client's side.
```

## 9. Questions and Terms to Revisit

```text
Terms I want to review: Residual risk, vulnerability vs threat,
Questions for my instructor: How should I rate a risk when part of it is outside my control, like a client's email?
```
