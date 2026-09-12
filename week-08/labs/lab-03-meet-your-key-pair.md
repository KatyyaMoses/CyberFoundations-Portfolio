# Week 8 Lab 03 — Meet Your Key Pair

**Student Name:** Kay Moses

**Date Completed:** Sept. 12, 2026

**Module:** 3 — Practical Cryptography  
**Submission Path:** `week-08/labs/lab-03-meet-your-key-pair.md`

> ## STOP — Protect secrets
> Never paste, upload, commit, or screenshot a password, passphrase, or private key. Do not run `cat` on any private-key file. Submit only the worksheet and the named screenshots. Keep all cryptographic files on the VM.

## How to Use This Lab

- **[TERMINAL]** means type or paste the command into the Cloud Heights terminal.
- **[WORKSHEET]** means type your response in this lab worksheet.
- Run commands in the order shown. Do not type the sample output.
- If your result does not match the stated success check, stop at the Troubleshooting box. Do not improvise with `sudo`, package installation, Azure settings, or SSH server configuration.

## Mission

Generate a dedicated Week 8 SSH key pair, identify its two files, and record the public-key fingerprint without exposing the private key.

## What You Already Know

The public and private keys have different jobs. The public key may be shared when appropriate. The private key stays under its owner's control. A key passphrase protects the private-key file; it is not the `analyst` account password.

## Lab Environment / Pre-Lab Check

**[TERMINAL] Run:** whoami cf-week8-check test ! -e ~/.ssh/week8_analyst_ed25519 &&   test ! -e ~/.ssh/week8_analyst_ed25519.pub &&   echo "READY: Week 8 key path is unused" ||   echo "STOP: Week 8 key already exists"

```bash
whoami
cf-week8-check
test ! -e ~/.ssh/week8_analyst_ed25519 &&   test ! -e ~/.ssh/week8_analyst_ed25519.pub &&   echo "READY: Week 8 key path is unused" ||   echo "STOP: Week 8 key already exists"
```

**Continue only if:** `whoami` prints `analyst`, all checks report `PASS`, and the final line begins with `READY`.

If the final line begins with `STOP`, do not overwrite the key. Ask the instructor whether to resume with the existing key or use the approved reset.

### If the VM Stops

Return to **My Lab Environment** in the Lab Portal and start your assigned VM. A stopped or deallocated VM is not deleted; saved disk files remain.

## Predict First

**[WORKSHEET]** Which file can be distributed when appropriate: the public key or private key? Explain.

```text
The public key is the one you can share. It is meant to be handed out to servers and other people so they can verify you. The private key has to stay secret on your own machine, because anyone who gets it could pretend to be you.
```

## Guided Steps

### Step 1 — Prepare the SSH Directory

**[TERMINAL] Run:** mkdir -p ~/.ssh chmod 700 ~/.ssh

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

### Step 2 — Generate the Key Pair

**[TERMINAL] Run:** ssh-keygen -t ed25519 -a 100   -f ~/.ssh/week8_analyst_ed25519   -C "week8-analyst-key"

```bash
ssh-keygen -t ed25519 -a 100   -f ~/.ssh/week8_analyst_ed25519   -C "week8-analyst-key"
```

At `Enter passphrase`, create a memorable Week 8 **key passphrase**. Type it again when asked. Nothing may appear while you type; this is normal. Do not use or enter the `analyst` account password unless it happens to be your deliberately chosen key passphrase.

### Step 3 — List the Two Key Files

**[TERMINAL] Run:** ls -l ~/.ssh/week8_analyst_ed25519 ~/.ssh/week8_analyst_ed25519.pub

```bash
ls -l ~/.ssh/week8_analyst_ed25519 ~/.ssh/week8_analyst_ed25519.pub
```

**Expected result:** Two filenames appear. The file ending in `.pub` is public. The file without `.pub` is private.

**Evidence moment:** Capture only this listing as `week08-lab03-key-files-permissions.png`.

### Step 4 — Display the Public-Key Fingerprint

**[TERMINAL] Run:** ls -l ~/.ssh/week8_analyst_ed25519 ~/.ssh/week8_analyst_ed25519.pub

```bash
ssh-keygen -lf ~/.ssh/week8_analyst_ed25519.pub
```

**Expected result:** A line containing key size, a `SHA256:` fingerprint, the comment `week8-analyst-key`, and `ED25519`.

**Evidence moment:** Capture the fingerprint as `week08-lab03-public-key-fingerprint.png`.

## Stop & Check

Do not run `cat`, `head`, `tail`, `less`, or `nano` on `~/.ssh/week8_analyst_ed25519`.

### Troubleshooting

- `Saving key ... failed`: confirm you are signed in as `analyst`; do not use `sudo`.
- `No such file` in Step 3 or 4: return to Step 2 and check whether key generation completed.
- Existing-key warning: answer `n` and contact the instructor. Never choose overwrite.

## Explain

**[WORKSHEET]** In 3–4 sentences, explain the handling difference between the two files and the purpose of the key passphrase.

```text
The two files work as a pair. The public key ends in .pub and is safe to give out, while the private key has no extension and must be kept secret and protected. The passphrase adds a second layer of safety. Even if someone got a copy of my private key file, they still could not use it without also knowing the passphrase.
```

## Analysis Questions

1. Why can the public key be distributed while the private key must remain protected?

```text
Because the public key is only used to check identity, not to prove it. Sharing it does no harm. The private key is what actually proves who you are, so if someone else got it they could impersonate you. That is why it never leaves your machine.
```

2. How is the key passphrase different from the `analyst` account password?

```text
The account password logs you into the analyst user on the machine. The key passphrase only unlocks the private key file so it can be used. They protect two different things, and they should not be the same.
```

3. Why did you use a unique Week 8 filename?

```text
So this new key does not overwrite or get confused with any existing keys. Giving it a clear Week 8 name keeps it separate and easy to find, and it avoids accidentally replacing a key I already had.
```

## Required Evidence

- `assets/screenshots/week-08/week08-lab03-key-files-permissions.png`
- `assets/screenshots/week-08/week08-lab03-public-key-fingerprint.png`

## Submission Checklist

- [x] The key pair uses the exact Week 8 filenames.

- [x] The private key was never displayed or submitted.

- [x] The fingerprint includes `SHA256:` and `ED25519`.

- [x] Both screenshots use the exact filenames.

- [x] Every worksheet response is complete.

## GitHub / Lab Portal Submission

1. In the Lab Portal, open the matching Week 8 lab.
2. Complete every **[WORKSHEET]** response.
3. Upload only the required screenshots to `assets/screenshots/week-08/`.
4. Select **Submit to GitHub**.
5. Open the committed worksheet and screenshots on GitHub. Confirm they are readable and contain no secrets.

**Never submit:** `.pem` files, files from `~/.ssh/`, passwords, passphrases, private-key contents, or a Bastion shareable URL.
