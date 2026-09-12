# Week 8 Lab 01 — Protect the Incident File

**Student Name:** Kay

**Date Completed:** 9/11/2026

**Module:** 3 — Practical Cryptography  
**Submission Path:** `week-08/labs/lab-01-protect-the-incident-file.md`

> ## STOP — Protect secrets
> Never paste, upload, commit, or screenshot a password, passphrase, or private key. Do not run `cat` on any private-key file. Submit only the worksheet and the named screenshots. Keep all cryptographic files on the VM.

## How to Use This Lab

- **[TERMINAL]** means type or paste the command into the Cloud Heights terminal.
- **[WORKSHEET]** means type your response in this lab worksheet.
- Run commands in the order shown. Do not type the sample output.
- If your result does not match the stated success check, stop at the Troubleshooting box. Do not improvise with `sudo`, package installation, Azure settings, or SSH server configuration.

## Mission

Encrypt a readable incident report, inspect the encrypted file safely, decrypt it, and prove the recovered file is identical to the original.

## What You Already Know

Encryption changes readable **plaintext** into protected **ciphertext**. In this lab, one passphrase protects and recovers the file. Encryption supports confidentiality, but it does not prevent someone from deleting or copying the encrypted file.

## Lab Environment / Pre-Lab Check

| Item | Required value |
|---|---|
| VM | Your assigned `cf-student-XX` VM |
| Linux account | `analyst` |
| Working directory | `~/cloud-heights/week8-cryptography` |
| Starting file | `evidence/incident-report.txt` |
| Estimated time | 25–35 minutes |

**[TERMINAL] Run:**

```bash
whoami
hostname
cf-week8-check
cd ~/cloud-heights/week8-cryptography
pwd
```

**Continue only if:** `whoami` prints `analyst`, the hostname begins with `cf-student-`, every environment check reports `PASS`, and `pwd` ends with `/cloud-heights/week8-cryptography`.

### If the VM Stops

Return to **My Lab Environment** in the Lab Portal and start your assigned VM. A stopped or deallocated VM is not deleted; saved disk files remain.

## Predict First

**[WORKSHEET]** What do you expect to see when encrypted data is inspected as bytes? Why should it not look like the original report?

```text
When I look at encrypted data as bytes, I expect to see scrambled, random-looking characters instead of readable words. It won't look like the original report because encryption turns the text into a form that nobody can read.
```

## Guided Steps

### Step 1 — Confirm the Original Is Readable

**[TERMINAL] Run:** cat evidence/incident-report.txt

```bash
cat evidence/incident-report.txt
```

**Expected result:** A readable report beginning with `CLOUD HEIGHTS INCIDENT REPORT`.

### Step 2 — Encrypt the Report

Choose one temporary lab passphrase that you can re-enter during this lab. Do not write it in the worksheet.

**[TERMINAL] Run:** openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000 -in evidence/incident-report.txt -out encrypted/incident-report.enc

```bash
openssl enc -aes-256-cbc -salt -pbkdf2 -iter 100000   -in evidence/incident-report.txt   -out encrypted/incident-report.enc
```

At `enter AES-256-CBC encryption password:`, type the passphrase. Nothing may appear while you type; this is normal. Press Enter, type the same passphrase again, and press Enter.

**Expected result:** The prompt returns without an error message.

### Step 3 — Inspect the Encrypted File

**[TERMINAL] Run:** file encrypted/incident-report.enc xxd -l 64 encrypted/incident-report.enc

```bash
file encrypted/incident-report.enc
xxd -l 64 encrypted/incident-report.enc
```

**Expected result:** `file` identifies data, and `xxd` displays hexadecimal bytes. The readable incident report does not appear.

**Evidence moment:** Capture the terminal now as `week08-lab01-encrypted-inspection.png`.

### Step 4 — Decrypt the File

**[TERMINAL] Run:** openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000   -in encrypted/incident-report.enc   -out encrypted/incident-report-decrypted.txt

```bash
openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000   -in encrypted/incident-report.enc   -out encrypted/incident-report-decrypted.txt
```

Enter the same passphrase from Step 2.

### Step 5 — Prove the Files Match

**[TERMINAL] Run:** cmp -s evidence/incident-report.txt encrypted/incident-report-decrypted.txt   && echo "MATCH: decrypted file equals original"   || echo "MISMATCH: stop and troubleshoot"

```bash
cmp -s evidence/incident-report.txt encrypted/incident-report-decrypted.txt   && echo "MATCH: decrypted file equals original"   || echo "MISMATCH: stop and troubleshoot"
```

**Required result:** `MATCH: decrypted file equals original`

**Evidence moment:** Capture the terminal now as `week08-lab01-decryption-match.png`.

## Stop & Check

Do not submit unless both required evidence moments succeeded.

### Troubleshooting

- `bad decrypt` or `bad password read`: rerun Step 4 and enter the exact Step 2 passphrase.
- `No such file or directory`: run `cd ~/cloud-heights/week8-cryptography`, then rerun the failed step.
- `MISMATCH`: delete only the decrypted copy with `rm -f encrypted/incident-report-decrypted.txt`, then repeat Steps 4–5.

## Explain

**[WORKSHEET]** In 3–4 sentences, explain how this lab demonstrates confidentiality and why encryption does not prevent deletion.

```text
This lab showed confidentiality in action. I used AES-256 encryption to scramble the incident report so that no one could read it without the passphrase. Even though the file was protected, encryption only controls who can read the contents. It doesn't lock the file in place. That's why encryption keeps information private but doesn't stop someone from deleting the file itself.
```

## Analysis Questions

1. Why can an encrypted file still be copied or deleted?

```text
Because encryption only protects what's inside the file, not the file itself. The system still sees it as a normal file sitting in a folder, so anyone with access to that location can copy or delete it. They just can't read what's in it without the passphrase.
```

2. What job did the passphrase perform?

```text
The passphrase was the key. It locked the file during encryption and was the only thing that could unlock it during decryption. Without the exact same passphrase, no one could turn the scrambled data back into the readable report.
```

3. Why does the match test prove correct recovery but not prove who handled the file?

```text
The match test only compares the two files and confirms the decrypted copy is identical to the original — so it proves the data recovered correctly. It says nothing about who ran the commands. To know who handled the file, you'd need logs or records of who had the passphrase, since the test only checks the content, not the person.
```

## Required Evidence

Save exactly these files in `assets/screenshots/week-08/`:

- `week08-lab01-encrypted-inspection.png`
- `week08-lab01-decryption-match.png`

## Submission Checklist

- [x] Both required results appeared.

- [x] Both screenshots use the exact filenames above.

- [x] No passphrase or other secret appears.

- [x] Every worksheet response is complete.

- [x] The worksheet is saved at the stated submission path.

## GitHub / Lab Portal Submission

1. In the Lab Portal, open the matching Week 8 lab.
2. Complete every **[WORKSHEET]** response.
3. Upload only the required screenshots to `assets/screenshots/week-08/`.
4. Select **Submit to GitHub**.
5. Open the committed worksheet and screenshots on GitHub. Confirm they are readable and contain no secrets.

**Never submit:** `.pem` files, files from `~/.ssh/`, passwords, passphrases, private-key contents, or a Bastion shareable URL.
