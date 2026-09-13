# Week 8 Lab 04 — Prove Who Signed It

**Student Name:** Kay

**Date Completed:** 9/12/2026

**Module:** 3 — Practical Cryptography  
**Submission Path:** `week-08/labs/lab-04-prove-who-signed-it.md`

> ## STOP — Protect secrets
> Never paste, upload, commit, or screenshot a password, passphrase, or private key. Do not run `cat` on any private-key file. Submit only the worksheet and the named screenshots. Keep all cryptographic files on the VM.

## How to Use This Lab

- **[TERMINAL]** means type or paste the command into the Cloud Heights terminal.
- **[WORKSHEET]** means type your response in this lab worksheet.
- Run commands in the order shown. Do not type the sample output.
- If your result does not match the stated success check, stop at the Troubleshooting box. Do not improvise with `sudo`, package installation, Azure settings, or SSH server configuration.

## Mission

Create a separate signing key pair, sign the incident report, verify the original, then prove that the same signature fails against a changed copy.

## What You Already Know

A digital signature uses a private key to sign and the matching public key to verify. Verification supports integrity and authenticity claims about the key. It does not encrypt the report or prove which human physically used the private key.

## Lab Environment / Pre-Lab Check

**[TERMINAL] Run:**  test ! -e keys/week8_signing_private.pem &&   test ! -e keys/week8_signing_public.pem &&   echo "READY: signing-key paths are unused" ||   echo "STOP: signing-key file already exists"

```bash
whoami
cf-week8-check
cd ~/cloud-heights/week8-cryptography
test ! -e keys/week8_signing_private.pem &&   test ! -e keys/week8_signing_public.pem &&   echo "READY: signing-key paths are unused" ||   echo "STOP: signing-key file already exists"
```

**Continue only if:** `whoami` prints `analyst`, all checks report `PASS`, and the final line begins with `READY`.

If it begins with `STOP`, do not overwrite or delete anything. Ask the instructor whether to resume or reset.

### If the VM Stops

Return to **My Lab Environment** in the Lab Portal and start your assigned VM. A stopped or deallocated VM is not deleted; saved disk files remain.

## Predict First

**[WORKSHEET]** Should the original signature verify after the report content changes? Explain.

```text
No, it should not verify after the content changes. A signature is tied to the exact contents of the file at the time it was signed. The file no longer matches the signature; therefore, the verification should fail. 

openssl dgst -sha256   -verify keys/week8_signing_public.pem   -signature signatures/incident-report.sig   signatures/incident-report-modified.txt

```

## Guided Steps

### Step 1 — Generate the Private Signing Key

**[TERMINAL] Run:** openssl genpkey   -algorithm RSA   -aes-256-cbc   -pkeyopt rsa_keygen_bits:2048   -out keys/week8_signing_private.pem

```bash
openssl genpkey   -algorithm RSA   -aes-256-cbc   -pkeyopt rsa_keygen_bits:2048   -out keys/week8_signing_private.pem
```

Create one **signing-key passphrase** when prompted. You will use this same passphrase in Steps 2 and 4. Do not record or screenshot it.

### Step 2 — Derive the Public Key

**[TERMINAL] Run:** openssl pkey   -in keys/week8_signing_private.pem   -pubout   -out keys/week8_signing_public.pem

```bash
openssl pkey   -in keys/week8_signing_private.pem   -pubout   -out keys/week8_signing_public.pem
```

Enter the signing-key passphrase from Step 1.

### Step 3 — Confirm Both Files Exist

**[TERMINAL] Run:** ls -l keys/week8_signing_private.pem keys/week8_signing_public.pem

```bash
ls -l keys/week8_signing_private.pem keys/week8_signing_public.pem
```

Do not display the private-key contents.

### Step 4 — Sign the Original Report

**[TERMINAL] Run:** openssl dgst -sha256   -sign keys/week8_signing_private.pem   -out signatures/incident-report.sig   evidence/incident-report.txt

```bash
openssl dgst -sha256   -sign keys/week8_signing_private.pem   -out signatures/incident-report.sig   evidence/incident-report.txt
```

Enter the signing-key passphrase from Step 1.

### Step 5 — Verify the Original Report

**[TERMINAL] Run:** openssl dgst -sha256   -verify keys/week8_signing_public.pem   -signature signatures/incident-report.sig   evidence/incident-report.txt

```bash
openssl dgst -sha256   -verify keys/week8_signing_public.pem   -signature signatures/incident-report.sig   evidence/incident-report.txt
```

**Required result:** `Verified OK`

**Evidence moment:** Capture this result as `week08-lab04-signature-valid.png`.

### Step 6 — Change a Copy

**[TERMINAL] Run:** cp evidence/incident-report.txt signatures/incident-report-modified.txt echo "Change: post-signature modification." >> signatures/incident-report-modified.txt

```bash
cp evidence/incident-report.txt signatures/incident-report-modified.txt
echo "Change: post-signature modification." >> signatures/incident-report-modified.txt
```

### Step 7 — Test the Original Signature Against the Changed Copy

**[TERMINAL] Run:** openssl dgst -sha256   -verify keys/week8_signing_public.pem   -signature signatures/incident-report.sig   signatures/incident-report-modified.txt

```bash
openssl dgst -sha256   -verify keys/week8_signing_public.pem   -signature signatures/incident-report.sig   signatures/incident-report-modified.txt
```

**Required result:** `Verification failure`. Additional OpenSSL error lines are normal for this intentional failure.

**Evidence moment:** Capture this result as `week08-lab04-signature-invalid-after-change.png`.

## Stop & Check

Do not create the changed copy until Step 5 returns `Verified OK`.

### Troubleshooting

- Step 2 or 4 cannot read the private key: re-enter the Step 1 signing-key passphrase.
- Step 5 reports failure: confirm you used the original report, public key, and signature paths exactly as shown.
- Step 7 reports `Verified OK`: run `tail -n 2 signatures/incident-report-modified.txt`. If the Change line is missing, repeat Step 6 once and rerun Step 7.

## Explain

**[WORKSHEET]** In 4–5 sentences, explain both verification results and why signing is not encryption.

```text
When I verified the original report, it passed with Verified OK because the file was exactly the same as when it was signed. When I checked the changed copy, it failed because the contents no longer matched the signature. This shows a signature protects integrity and proves a file has not been altered. Signing is not the same as encryption, because the report stayed fully readable the whole time. Encryption hides the contents, while signing only proves who signed it and that it has not changed.
```

## Analysis Questions

1. What does `Verified OK` establish within this lab?

```text
Because a line was added to it after it was signed. The signature was made from the original contents, so once the file changed it no longer matched, and verification failed. That is the signature doing its job.

```

2. Why did the changed copy fail verification?

```text
Because a line was added to it after it was signed. The signature was made from the original contents, so once the file changed it no longer matched, and verification failed. That is the signature doing its job.
```

3. Why is the signed report still readable?

```text
Because signing does not hide or scramble the contents. It only creates a separate signature to prove the file is genuine. The report itself stays in plain readable text.
```

4. Why does the signature alone not prove which human used the private key?

```text
Because the signature only proves the private key was used, not who was holding it. If someone else got the key, they could sign too. Tying it to a person needs other controls like access records or protecting who can reach the key.
```

## Required Evidence

- `assets/screenshots/week-08/week08-lab04-signature-valid.png`
- `assets/screenshots/week-08/week08-lab04-signature-invalid-after-change.png`

## Submission Checklist

- [x] The original returned `Verified OK`.

- [x] The changed copy returned `Verification failure`.

- [x] The private key and passphrase were never displayed or submitted.

- [x] Both screenshots use the exact filenames.

- [x] Every worksheet response is complete.

## GitHub / Lab Portal Submission

1. In the Lab Portal, open the matching Week 8 lab.
2. Complete every **[WORKSHEET]** response.
3. Upload only the required screenshots to `assets/screenshots/week-08/`.
4. Select **Submit to GitHub**.
5. Open the committed worksheet and screenshots on GitHub. Confirm they are readable and contain no secrets.

**Never submit:** `.pem` files, files from `~/.ssh/`, passwords, passphrases, private-key contents, or a Bastion shareable URL.
