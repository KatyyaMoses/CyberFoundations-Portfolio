# Week 9 Lab 02 - Build and Test Digital Trust

**Student Name:** Kay Moses

**Date Completed:** 9/18/2026

**Module:** 3 - Practical Cryptography | **Week:** 9  
**Submission Path:** `week-09/labs/lab-02-build-and-test-digital-trust.md`

> ## Vault Exchange Trust and Key Safety Rule
> Use your assigned Ubuntu training computer (VM) as `analyst`. Keep Week 6–8 files and your SSH login keys unchanged. Do not run `sudo` (administrator commands), change network rules, add your pretend badge office to the computer or browser’s accepted list, or make the practice server available to other computers. All new work stays inside a fresh practice folder under `~/cloud-heights/week9-digital-trust/`.
>
> **Evidence safety:** Never display, upload, or commit private-key contents. Publish selected worksheet text and reviewed screenshots only. Do not upload the whole VM workspace or run `git add .` from it. Crop credentials, access URLs, and account information.

---

## Mission

Ivy needs a digital badge for a practice service running on her training computer. You will help her apply for the badge, issue it from a pretend badge office, and check it.

The digital badge is a **certificate**. The issuing office is a **certificate authority**, shortened to **CA**. You will play both roles for this exercise. Your pretend office will not become trusted by public websites or other people’s browsers.

You will also try a wrong name and a wrong office on purpose. A check refusing the wrong information is the result we want. Finally, you will compare your practice service with the real website from Lab 01.

The badge application is called a **certificate signing request (CSR)**. Signing an application shows that its maker used the matching secret key. It does not prove they have permission to get a badge for someone else’s website. A real issuing office must check that permission before issuing a certificate.

## What You Already Know

Think back to these ideas; you do not need to memorize the technical names:

- A **service** is a program waiting to help another program, like a reception desk waiting for visitors.
- A **VM (virtual machine)** is your assigned training computer, accessed through Cloud Heights.
- A **terminal** is a window where you type instructions, called **commands**. **Bash** reads and runs those instructions.
- **OpenSSL** is the tool we will use to make keys and check certificates.
- A **key pair** has a secret private key and a shareable public key. Never share the private key.
- **TLS** is the method programs use to set up a protected connection. It checks identity and arranges keys to protect the messages.

Looking at a badge, checking a badge, and successfully using it at a door are different activities. We will collect evidence for each one.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Assigned Cloud Heights Ubuntu VM, account `analyst`, Bash shell |
| Tools | OpenSSL 3.x, core utilities, `ss`, and two terminal sessions |
| Working root | `~/cloud-heights/week9-digital-trust/` |
| Separate practice area | Fresh `practice-*` folder; no preloaded certificate fixture assumed |
| Practice connection address | `127.0.0.1:8443`, expected DNS name `localhost` |
| Time | 90-120 minutes; work in Parts A-D with pauses |
| Submission | Selected evidence only; private keys stay on the VM |

- [x] I completed Lab 01 or have a dated public-site observation ready.

- [x] I am on my assigned VM and can open a second terminal session to that same VM.

- [x] My account is `analyst` and I can write to the Week 9 workspace.

- [x] I know that an intentional negative test is successful learning evidence when it rejects for the intended reason.

### Cloud Heights Idle Stop

Respond to the Portal's idle warning while actively working. A stopped/deallocated VM is not deleted; restart it from My Lab Environment. Saved files remain, but the TLS listener must be started again after a restart. Reopen your recorded practice path instead of regenerating keys over existing files.

### Before You Type Commands

1. Work in your assigned VM, using the account named `analyst`. Do not paste these commands into your personal computer’s terminal.
2. Copy one entire **bash** box at a time, paste it into the terminal, and press Enter if the last line has not run. Do not copy the box borders or the word `bash`.
3. Wait for your usual command prompt to return before pasting the next box. A **prompt** is the terminal’s “ready for your next command” line. Step 10 is the exception: the service keeps running there.
4. Long commands may wrap onto another screen line. Copy the whole command. You do not need to memorize the options beginning with `-`.
5. Text in **text** boxes is for your worksheet answers, not the terminal. Replace the blank prompts with your own observations.
6. If the result differs from the expected result, stop at that step and show your instructor the command and error. Do not remove checking options to make an error disappear.

A **path** is a file or folder’s address. `~` means your home folder. `cd` means “move into this folder”; `pwd` prints the folder you are currently in. A value such as `W9_RUN` is a named storage box the command uses to remember a path. Keep the commands exactly as written.

## Predict First

Make a best guess for each test. “Expected name” means the name we want the badge to cover. A “listener” is a running program waiting for a connection. Port `8443` is the numbered door this program uses. Explain each prediction in 2–3 sentences; it is okay to be unsure before trying it.

| Test | Your prediction and reason |
| --- | --- |
| Intended lesson CA + expected name `localhost` | This is the test where everything is set up correctly.  My prediction is this one passes, because the name matches and the issuer is trusted. |
| Same certificate and CA + expected name `wrong.test` | Here the certificate and issuer are fine, but I'm asking it to match the name "wrong.test," which isn't the name on the certificate. My prediction is this fails the name check. |
| Same certificate and expected name + unrelated CA | The name matches, but it to trust an unrelated certificate authority that didn't sign this certifcate. My prediction it will fails because the browser that can't trace the certificate back to CA it trusts. |
| Same intended inputs, but no listener on port 8443 | The inputs are all correct, but there's no program actually listening on port 8443. My prediction is the connection fails to happen at all because there's nothing there to answer. |

## Guided Steps

### Part A - Prepare the Key, Request, and Issuer

#### Step 1 - Check the Environment

This first box checks your account, the installed tools, the clock, and whether our numbered door is already in use. It also makes the Week 9 folder if it does not exist. **It does not start the practice service.**

```bash
whoami
openssl version
command -v ss
date -u
mkdir -p ~/cloud-heights/week9-digital-trust
test -w ~/cloud-heights/week9-digital-trust && printf 'Workspace writable\n'
ss -ltn 'sport = :8443'
```
**What you should see, in order**:

1. `analyst` — your account name.
2. An OpenSSL version beginning with `3.` — the tool is installed.
3. A file path ending in `ss` — the tool for listing waiting services is available.
4. Today’s date and time in **UTC**, a shared time standard. It can differ from your local clock by your time-zone offset.
5. `Workspace writable` — you can save files in the Week 9 folder.

```text
Account name shown: analyst
OpenSSL version shown: OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
Path to the ss tool: /usr/bin/ss/usr/bin/ss
UTC date and time shown: Fri Sep 18 14:38:45 UTC 2026
Workspace writable result: writable
Port 8443 listener result (blank output means nothing is listening): blank — no listener on 8443
```

6. A heading with no service row below it — port 8443 is free.

**Stop and ask for help** if an item is missing, the date is wrong, or a service row appears. Do not close an unknown program or change the clock yourself.

```text
Account: analyst
OpenSSL version: OpenSSL version: OpenSSL 3.0.2 15 Mar 2022
UTC clock: UTC clock: Fri Sep 18 14:38:45 UTC 2026
Port 8443 free? Evidence: yes, since it returned blank. nothing is listening
```

#### Step 2 - Create a Fresh Practice Folder

This box makes a new folder for this attempt. **Directory** means folder. `mktemp` chooses a fresh name so you do not overwrite earlier work. `umask` restricts who can read newly created files. The four inner folders will hold the badge office files, applications, finished badges, and check results.

```bash
umask 077
W9_RUN=$(mktemp -d "$HOME/cloud-heights/week9-digital-trust/practice-XXXXXX")
cd "$W9_RUN"
mkdir ca requests certificates verification
printf '%s\n' "$PWD"
```

```text
My exact practice path: /home/analyst/cloud-heights/week9-digital-trust/practice-8jRd0b
```

**What you should see:** a path ending in `practice-` plus a random ending. Copy that exact path into your answer above. It will be different for each attempt.

**To resume later:** type `cd`, then a space, then paste your recorded path and press Enter. Type `pwd` and press Enter to confirm you are in that folder. Do not run Step 2 again just to resume; that would create a different attempt.

#### Step 3 - Create the Service Key and CSR

First make a new key pair for Ivy’s practice service. **RSA** is the kind of key we are making; **2048** is its size in bits. These values are provided for you. This box saves the private key and writes a separate file containing only the public key. Do not reuse a key you use to log in.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out requests/service.key.pem
chmod 600 requests/service.key.pem
openssl pkey -in requests/service.key.pem -pubout -out requests/service.public.pem
```
**What you should see:** progress symbols may appear, then the prompt returns. Some lines finish without printing anything. The private-key file is restricted to your account by `chmod 600`. It has no password protection for this temporary practice exercise; real systems need a planned way to protect their keys.

**Do not open or print `service.key.pem`.** `cat` displays a file’s contents, so use it only on the exact public-information files listed in these instructions.

Next make the badge application: the **CSR**. It includes the service’s public key, its requested name, and a signature made with its private key. `localhost` is the practice name for this computer. **SAN** means the list of names the certificate should cover. This request asks for `localhost`. The box also checks the request’s signature and displays the request, which contains public information.

```bash
openssl req -new -sha256 -key requests/service.key.pem -out requests/service.csr.pem -subj "/O=CyberFoundations Lab/CN=localhost" -addext "subjectAltName=DNS:localhost"
openssl req -in requests/service.csr.pem -noout -verify
openssl req -in requests/service.csr.pem -noout -text > verification/csr-inspection.txt
cat verification/csr-inspection.txt
```
Expected: a request-signature verification message such as `Certificate request self-signature verify OK`, plus requested `DNS:localhost` in the inspection. A CSR contains public information and a signature, not the private key. Record the actual output, not this example.

```text
Requested subject and SAN: Subject: O = CyberFoundations Lab, CN = localhost. SAN: DNS:localhost. The request asks for a certificate covering the name localhost.
CSR signature-check result: Certificate request self-signature verify OK
What the successful signature check tells me about the request: It confirms the request was signed by the private key that matches the public key inside it. So whoever made the request controls that private key, and the request wasn't altered. The key pair belongs together.
Why signing an application does not prove permission to use someone else’s website name: Signing only proves I control my own private key. It does not prove I'm allowed to use the name in the request. Anyone can make a key and request any name, even one they don't own. That's why a certificate authority must separately verify the requester actually controls the name before issuing a certificate. The signature proves key ownership, not name ownership.
```

#### Step 4 - Model the Issuing Office

Now pretend you work at the badge office. The office needs its **own** key pair, separate from the service’s key. This box makes that pair and the office’s certificate. The office signs its own certificate; that is called **self-signed**. Printing and signing your own badge does not make other people accept it. Later, we will tell our checking tool to accept this particular office for a particular test.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out ca/lesson-ca.key.pem
openssl req -new -x509 -sha256 -days 365 -key ca/lesson-ca.key.pem -out ca/lesson-ca.cert.pem -subj "/O=CyberFoundations Lab/CN=Week 9 Lesson CA" -addext "basicConstraints=critical,CA:TRUE,pathlen:0" -addext "keyUsage=critical,keyCertSign,cRLSign"
chmod 600 ca/lesson-ca.key.pem
openssl x509 -in ca/lesson-ca.cert.pem -noout -subject -issuer -dates
```
**What you should see:** Subject and Issuer both name `Week 9 Lesson CA`, followed by dates. That is expected for this self-signed office certificate. Here the office will sign the service badge directly; there is no middle office (intermediate CA). Your browser and computer’s accepted-office lists have not changed.

```text
Lesson CA subject shown: Week 9 Lesson CA
Lesson CA issuer shown: Lesson CA — same as subject, because it's self-signed
Lesson CA validity dates: notBefore Sep 22 12:48:56 2026 GMT, notAfter Sep 22 12:48:56 2027 GMT
Why a self-signed office certificate is not automatically accepted: The subject and issuer are identical, so it vouches for itself and nothing independent confirms it. Anyone can generate a self-signed CA claiming authority, so it's only trusted if it's deliberately placed in a trusted store. Self-signing proves the office issued its own badge, not that anyone should trust that office.
```

**Pause point:** save your worksheet and practice path before Part B. Your keys and request are saved on the VM.

### Part B - Issue, Inspect, and Verify

#### Step 5 - Set the Badge Rules and Issue the Certificate

This box writes the office’s rules into a small settings file. The rules say: “This badge is for a service called `localhost`, for use as a web server. It cannot issue other badges.”

Copy the entire box, including the last `EOF` on its own line. `EOF` tells the terminal “the file text ends here.” If you see a `>` prompt while entering the block, the terminal is waiting for the remaining lines. Do not paste the next command until the complete block has finished.

You do not need to memorize these settings. `CA:FALSE` means “not an issuing office”; `serverAuth` means “may identify a server”; `DNS:localhost` is the allowed name. The office chooses the final rules instead of accepting everything an applicant might ask for.

```bash
cat > ca/server-ext.cnf <<'EOF'
[server_cert]
basicConstraints=critical,CA:FALSE
keyUsage=critical,digitalSignature,keyEncipherment
extendedKeyUsage=serverAuth
subjectAltName=DNS:localhost
subjectKeyIdentifier=hash
authorityKeyIdentifier=keyid,issuer
EOF
```
**What you should see:** the usual prompt returns; this settings-file step normally prints nothing.

Now issue the badge. The next box uses the **office’s private key** to sign the certificate, gives it a 30-day date period, saves it, and displays it. Its serial number is a randomly chosen tracking number; yours need not match another student’s.

```bash
openssl x509 -req -in requests/service.csr.pem -CA ca/lesson-ca.cert.pem -CAkey ca/lesson-ca.key.pem -set_serial "0x$(openssl rand -hex 16)" -days 30 -sha256 -extfile ca/server-ext.cnf -extensions server_cert -out certificates/service.cert.pem
openssl x509 -in certificates/service.cert.pem -noout -text > verification/certificate-inspection.txt
cat verification/certificate-inspection.txt
```
**What you should see:** Subject includes `localhost`; Issuer includes `Week 9 Lesson CA`; SAN includes `DNS:localhost`; Basic Constraints says `CA:FALSE`; and Extended Key Usage lists TLS Web Server Authentication. If a required value differs, stop and ask for help before the next step.

```text
SAN shown in the certificate inspection: DNS:localhost
Basic Constraints shown in the certificate inspection: critical, CA:FALSE
Extended Key Usage shown in the certificate inspection: TLS Web Server Authentication
Any difference from the expected result (write "none" if there is none): none
```

Use these reminders to fill the table: **Subject** describes the badge holder; **Issuer** names the signing office; **SAN** lists allowed names; **Not Before/Not After** give the start/end dates; **algorithm** means calculation method; **Basic Constraints** says whether this is an issuing office; **Key Usage/EKU** describe allowed jobs. The website’s key and the office’s signature have different jobs.

Your dates, tracking number, and key details will differ from classmates’ files. Copy your finished certificate’s values, not just the application’s values.

| Field | Issued value | Why it matters |
| --- | --- | --- |
| Subject | O = CyberFoundations Lab, CN = localhost | Names who the badge is for — the service "localhost |
| Issuer | O = CyberFoundations Lab, CN = Week 9 Lesson CA | Names the office that signed it; different from subject, so it's not self-signed |
| SAN | DNS:localhost | The actual names the certificate is valid for; this is what gets name-checked |
| Not Before / Not After | Sep 22 12:57:46 2026 GMT / Oct 22 12:57:46 2026 GMT | The 30-day window the certificate is valid |
| Subject key algorithm and size | RSA (rsaEncryption), 2048 bit | The service's public key type and strength |
| Certificate signature algorithm | sha256WithRSAEncryption | How the CA signed the certificate (the office's job, not the site's key) |
| Basic Constraints | critical, CA:FALSE | This is a leaf certificate — it cannot issue other certificates |
| Key Usage / Extended Key Usage | Digital Signature, Key Encipherment / TLS Web Server Authentication | What the certificate is allowed to do — identify a web server |

#### Step 6 - Check That the Same Public Key Was Carried Through

Did the public key from Ivy’s key pair make it into the application and then the finished badge? This box copies out the public information and calculates a **SHA-256 fingerprint** for each public-key file. A fingerprint is a calculated label we can compare. This box does not display private keys.

```bash
openssl req -in requests/service.csr.pem -pubkey -noout > requests/csr.public.pem
openssl x509 -in certificates/service.cert.pem -pubkey -noout > certificates/service.public.pem
sha256sum requests/service.public.pem requests/csr.public.pem certificates/service.public.pem
```
**What you should see:** three lines beginning with the same long string of letters and numbers. Those strings are the fingerprints, also called **digests**. Matching strings show that the same public key appears in the original public-key file, the CSR, and the certificate. They do not tell us whether the name, dates, or issuing office should be accepted. If the strings differ, stop and ask for help before continuing.

```text
Fingerprint for requests/service.public.pem: 6328fa825373b27e3376985c3c38c05497616b6533c2923153ed1da73f1d86a
Fingerprint for requests/csr.public.pem: 6328fa825373b27e3376985c3c38c05497616b6533c2923153ed1da73f1d86a
Fingerprint for certificates/service.public.pem: 6328fa825373b27e3376985c3c38c05497616b6533c2923153ed1da73f1d86a
Do all three match, and what does that show: Yes, all three fingerprints match. This shows the same public key was carried through from the original key pair into the CSR and into the finished certificate, so the key never changed along the way. It confirms the certificate belongs to the key I generated.
```

#### Step 7 - Run the Passing Verification

Now check the saved certificate without connecting to a running service. This is an **offline check**: it reads files on your VM.

Read the main choices in the command before running it:

| Command part | What we are asking |
|---|---|
| `-CAfile ca/lesson-ca.cert.pem` | Accept this badge office for this check. This chosen starting point is called a trust anchor. |
| `-no-CApath -no-CAstore` | Do not also use the computer’s default certificate folders or store. |
| `-purpose sslserver` | Check that the badge is suitable for a server. |
| `-verify_hostname localhost` | Check that it covers the name `localhost`. |

OpenSSL also uses the computer’s clock to check dates. The last three lines save the result number, show the message, and print that number. An **exit status** of `0` means the command succeeded; another number means it did not. Read the message to learn why. `> ... 2>&1` saves the command’s messages in a text file; it is included for you.

```bash
openssl verify -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname localhost certificates/service.cert.pem > verification/pass.txt 2>&1
W9_STATUS=$?
cat verification/pass.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** `certificates/service.cert.pem: OK` and exit status `0`. This is our working starting test, also called the **baseline**. If it fails, stop and ask for help. The later wrong-input tests are meaningful only after this starting test works.

```text
Command message shown by verification/pass.txt: certificates/service.cert.pem: OK
Exit status printed: 0
What this baseline result proves: It proves the certificate passes every check when everything is correct: it chains to the trusted lesson CA, it's valid for a server, it covers the name localhost, and its dates are current. Exit status 0 confirms the command succeeded.
```

#### Step 8 - Ask for the Wrong Name on Purpose

Keep the same badge and office, but ask whether the badge covers `wrong.test`. It was issued for `localhost`, so these names should not match. Copy this complete box; the change is already made for you.

```bash
openssl verify -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname wrong.test certificates/service.cert.pem > verification/wrong-name.txt 2>&1
W9_STATUS=$?
cat verification/wrong-name.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
Expected: hostname mismatch and a nonzero exit status. The leaf and CA did not change. `wrong.test` is just an expected-name input; this offline command makes no connection to that name.

```text
Command message shown by verification/wrong-name.txt: error 62 at 0 depth lookup: hostname mismatch — certificates/service.cert.pem: verification failed
Exit status printed: 2
Why the check refused this name: The certificate's SAN only covers localhost, not wrong.test. The name I asked it to verify doesn't match any name in the certificate, so the hostname check fails with error 62. they are valid
```

#### Step 9 - Ask a Different Office to Vouch for the Same Badge

First make a second pretend office with its own key. It did not issue our service’s badge. This is a **negative test**: we intentionally give wrong information to see whether the check refuses it. The first box makes the second office; the next box checks the unchanged service certificate using only that second office.

```bash
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out ca/unrelated-ca.key.pem
openssl req -new -x509 -sha256 -days 365 -key ca/unrelated-ca.key.pem -out ca/unrelated-ca.cert.pem -subj "/O=CyberFoundations Lab/CN=Unrelated Lesson CA" -addext "basicConstraints=critical,CA:TRUE,pathlen:0" -addext "keyUsage=critical,keyCertSign,cRLSign"
chmod 600 ca/unrelated-ca.key.pem
```

```bash
openssl verify -CAfile ca/unrelated-ca.cert.pem -no-CApath -no-CAstore -purpose sslserver -verify_hostname localhost certificates/service.cert.pem > verification/wrong-ca.txt 2>&1
W9_STATUS=$?
cat verification/wrong-ca.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** a message such as `unable to get local issuer certificate`, and an exit status other than `0`. In plain language: “The office you gave me cannot vouch for this badge.” That refusal is expected. A missing-file error is a different problem; ask for help if you see one. Do not install these offices into your browser or computer’s accepted list.

```text
Command message shown by verification/wrong-ca.txt: unable to get local issuer certificate — certificates/service.cert.pem: verification failed (copy exact wording)
Exit status printed:
Why the unrelated office could not vouch for this badge: The certificate was signed by the Week 9 Lesson CA, not the Unrelated CA. When I tell the check to trust only the Unrelated CA, it can't build a chain back to a trusted issuer, so it fails with "unable to get local issuer certificate." The name and dates are fine — the problem is that the wrong office was asked to vouch for a badge it never signed
```

**Pause point:** record all three results below before Part C. Keep the same service certificate throughout these tests.

| Test | Leaf file | CA file | Expected name | Actual result / exit status | Explanation |
| --- | --- | --- | --- | --- | --- |
| Passing baseline | certificates/service.cert.pem | ca/lesson-ca.cert.pem | localhost | OK/exit | Correct CA, correct name — passes |
| Wrong name | certificates/service.cert.pem | ca/unrelated-ca.cert.pem | wrong.test | verification failed, hostname mismatch / exit 2 | Right CA, but name not covered — fails |
| Wrong CA | certificates/service.cert.pem | ca/unrelated-ca.cert.pem | localhost | 	verification failed, unable to get local issuer certificate / exit 2 | Right name, but wrong signing office — fails |

### Part C - Try a Protected Connection on This VM

So far we checked saved files. Now we will connect two programs on the same VM.

- The **server** waits for a visitor. The **client** makes the visit and checks the badge.
- `127.0.0.1` is the address meaning “this same computer.” Connecting back to yourself is called **loopback**.
- `8443` is the numbered door, or **port**, where our server waits.
- `localhost` is the name we expect on its certificate. An address tells the client where to go; the expected name tells it which identity to check.
- **TLS 1.3** is the protected-connection version we will use. Its handshake is the initial exchange where the programs check identity and arrange message-protection keys.

Keep two terminal windows open: **A runs the server; B runs the checks.** Both must connect to your assigned VM.

#### Step 10 - Start the Listener in Terminal A

Use your existing terminal as **Terminal A**. Make sure you are in the practice folder from Step 2. This box starts your waiting service:

```bash
openssl s_server -accept 127.0.0.1:8443 -cert certificates/service.cert.pem -key requests/service.key.pem -www -tls1_3
```
**What you should see:** `ACCEPT`, and the usual prompt does not return. This is normal: the server is waiting. No client has checked its identity yet. Leave this window alone and continue in Terminal B. Keep the address exactly `127.0.0.1` so the service is available only on this VM. Do not change it to `0.0.0.0` or change network rules.

```text
Message shown in Terminal A after starting the listener: State    Recv-Q  Send-Q  Local Address:Port   Peer Address:Port   Process
LISTEN   0       4096    127.0.0.1:8443       0.0.0.0:*
Time you started the listener:
```

#### Step 11 - Check the Connection in Terminal B

Open a second terminal to the **same VM**. Use `cd` with the exact practice path you recorded in Step 2. Confirm it with `pwd`; the `ca/` and `certificates/` directories must belong to that run. Then execute:

```bash
ss -ltn 'sport = :8443'
```
Expected: a listener at `127.0.0.1:8443`. Save that evidence before the connection test.

```text
Listener row shown by ss: LISTEN 0 4096 127.0.0.1:8443 0.0.0.0:*
```

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname localhost -verify_return_error -tls1_3 -brief < /dev/null > verification/tls-pass.txt 2>&1
W9_STATUS=$?
cat verification/tls-pass.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** TLS 1.3, `Verification: OK`, and exit status `0`. If not, confirm Terminal A is still waiting and both terminals use the same practice folder. Ask for help if it still differs.

Two command options have different jobs. `-servername localhost` tells the server which service name we want; this label is called **SNI**. `-verify_hostname localhost` checks whether the certificate actually covers the name. Requesting a name is not the same as checking it. `-verify_return_error` tells the client to stop if the certificate check fails. The short output also shows a **cipher**: the chosen method for protecting messages.

Now change only the expected name, keeping SNI and all other inputs the same:

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname wrong.test -verify_return_error -tls1_3 -brief < /dev/null > verification/tls-wrong-name.txt 2>&1
W9_STATUS=$?
cat verification/tls-wrong-name.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```
**What you should see:** a certificate/name mismatch message and a status other than `0`. Reaching the server’s door is not enough; the badge must also pass the name check. If the message says “connection refused,” the server is not waiting, so that is not evidence of the intended name rejection.

```text
Listener address and port: 127.0.0.1:8443
Connected destination: 127.0.0.1:8443
Expected identity for the passing test: localhost
SNI value: localhost
CA file used: ca/lesson-ca.cert.pem
Negotiated TLS version and cipher from the passing output: TLSv1.3, TLS_AES_256_GCM_SHA384
Passing verification message and exit status: Verification: OK, Verify return code: 0 (ok), exit status 0
Failing verification message and exit status:
What changed between the two tests:
Explain each job: the certificate links a name to a public key; the handshake checks use of the matching private key; newly arranged traffic keys protect messages. How are these different? The certificate links a name (localhost) to a public key — it's the identity document. The handshake checks that the server actually holds the matching private key, proving it's the real owner of that certificate, not just someone holding a copy. Then the two sides arrange fresh keys to encrypt the actual traffic. they are different because the handsnake proves who owns the key, and the traffic keys protect the messages.
```

#### Step 12 - Stop Only Your Listener

In Terminal A, press **Ctrl+C**. In Terminal B, rerun `ss -ltn 'sport = :8443'`; no listener row should remain. Do not use a broad process-kill command.

In Terminal B, use the original passing inputs but save this stopped-listener result in its own file:

```bash
openssl s_client -connect 127.0.0.1:8443 -servername localhost -CAfile ca/lesson-ca.cert.pem -no-CApath -no-CAstore -verify_hostname localhost -verify_return_error -tls1_3 -brief < /dev/null > verification/no-listener.txt 2>&1
W9_STATUS=$?
cat verification/no-listener.txt
printf 'Exit status: %s\n' "$W9_STATUS"
```

**What you should see:** “connection refused” and a status other than `0` when no service is waiting. There is no one at the door, so the certificate check never gets that far. Keep this result separate from the earlier wrong-badge result.

```text
ss output after you pressed Ctrl+C:
Message shown by verification/no-listener.txt:
Exit status printed:
Why this failure is different from a badge failing its check:
```

**Pause point:** the server is now stopped. Save your worksheet and screenshots before Part D.

### Part D - Explain Problems and Collect Your Work

#### Step 13 - Interpret Four Conditions

For each row, explain what went wrong, what message or field you would look at, what you would correct, and which check you would repeat. **Retest** means try the check again after a correction. Use your earlier wrong-name, wrong-office, and stopped-server results to reason about rows 1, 3, and 4. Row 2 is a pretend situation to explain in writing; do not change the VM clock or certificate dates.

| Condition | Which check cannot pass? | What would you look at? | What would you fix, and why? | Which check would you repeat? |
| --- | --- | --- | --- | --- |
| `localhost` is absent from a service's SAN |   |   |   |   |
| Not After is in the past relative to the correct clock |   |   |   |   |
| An unrelated CA file is supplied |   |   |   |   |
| No listener exists at `127.0.0.1:8443` |   |   |   |   |

A badge may need replacing when its dates run out. Getting a new certificate is called **renewal**. The service must also be set up to use the new file; that is **deployment**. An office can cancel a certificate before its end date; that is **revocation**. Our file checks did not contact an online cancellation service.

In your own words, explain why getting a replacement file is not enough until the server uses it. Then explain why our passing file check does not prove that someone checked an up-to-date cancellation list.

```text
(your explanation here)
```

#### Step 14 - Compare the Real Website with Your Practice Service

Use your dated Lab 01 observation and your actual local certificate.

| Comparison | Public website from Lab 01 | Isolated lesson service |
| --- | --- | --- |
| Intended hostname / SAN |   |   |
| Issuer and chain roles |   |   |
| Validity interval |   |   |
| Allowed purpose |   |   |
| Who accepts the top issuing office, and how is that choice made? |   |   |
| Where does the service run, and how did you connect? |   |   |
| Evidence actually observed |   |   |
| What does this evidence NOT tell you? |   |   |

## Stop & Check

- Can you explain the separate jobs of the service’s secret key, its application (CSR), the office’s secret key, and the finished badge (certificate)?
- Does the issued SAN contain the intended name?
- Do all three public-key fingerprints match, with no private key displayed?
- Did the correct-input check pass before you tried the wrong inputs?
- Did each failure occur for the predicted reason rather than because of a missing file?
- Did the service wait only at `127.0.0.1:8443`, and did you stop it afterward?

## Test

Your evidence must show: three matching public-key fingerprints; a passing file check; refusal of the wrong name; refusal of the wrong office; a passing live TLS connection; refusal of the wrong name during TLS; and a refused connection after stopping the service. For each check, include the command you ran, the message, and the exit-status number. A number other than `0` is not enough by itself: explain the message. Do not copy the terminal’s prompt into a command.

## Capture Evidence

Take screenshots of the public CSR/certificate inspection and each required test outcome. Capture command inputs and result together when possible. Do not screenshot private keys or unfiltered verbose TLS session dumps. Copy exact relevant text into the worksheet as well, so another learner can follow your reasoning.

## Explain

Write 5–7 sentences telling the story of what you did. Start with “First I made…”, then describe the badge application, the issuing office’s signature, your inspection, your file checks, and the live connection. Say which office you told the client to accept. Include one wrong-input test, why it failed, and what you kept the same.

```text
(your walkthrough here)
```

## Required Evidence

Save screenshots under `assets/screenshots/week-09/`:

- `week09-lab02-csr-check.png`
- `week09-lab02-issued-certificate.png`
- `week09-lab02-key-correspondence.png`
- `week09-lab02-verify-pass.png`
- `week09-lab02-verify-wrong-name.png`
- `week09-lab02-verify-wrong-ca.png`
- `week09-lab02-loopback-listener.png`
- `week09-lab02-tls-pass.png`
- `week09-lab02-tls-wrong-name.png`
- `week09-lab02-no-listener.png`

Add numbered image suffixes if necessary for readability. Keep `verification/*.txt` on the VM for your own reference; publishing those logs is optional only after a content review. Do not upload any `.key.pem` file, even though it is a classroom key.

### Evidence Uploads (required)

![week09-lab02-csr-check.png](https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week-09/week09-lab02-csr-check.png?raw=true)

**Caption - CSR inspection:**

![week09-lab02-issued-certificate.png](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/5b224881117d638da7e991db4473ac69753e4b30/assets/screenshots/week-09/week09-lab02-issued-certificate.png)

**Caption - issued certificate:**

![week09-lab02-key-correspondence.png](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/ee8d5f580d45108a51d4649761ac157db6b48f55/assets/screenshots/week-09/week09-lab02-verify-pass.png)

**Caption - public-key fingerprints:**

![week09-lab02-verify-pass.png](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab02-verify-pass.png)

**Caption - passing offline check:**

![week09-lab02-verify-wrong-name.png](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/2c1acd6007c1b819bffd0b0c75ed91a31d9dd43d/assets/screenshots/week-09/week09-lab02-verify-wrong-name.png)

**Caption - wrong-name refusal:**

![week09-lab02-verify-wrong-ca.png](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/bb558876fb24320edc712f5067d0cb03df65cb75/assets/screenshots/week-09/week09-lab02-verify-wrong-ca.png)

**Caption - wrong-office refusal:**

![week09-lab02-loopback-listener.png](https://github.com/KatyyaMoses/CyberFoundations-Portfolio/blob/main/assets/screenshots/week-09/week09-lab02-loopback-listener.png?raw=true)

**Caption - loopback listener:**

![week09-lab02-tls-pass.png](https://raw.githubusercontent.com/KatyyaMoses/CyberFoundations-Portfolio/d0730a2cc994a0c01fbd4e7df49d4cd7bfc43ecd/assets/screenshots/week-09/week09-lab02-tls-pass.png)

**Caption - passing TLS connection:**

**Caption - TLS wrong-name refusal:**

**Caption - stopped listener:**

### Optional Extra Evidence (leave blank if you do not need it)

**Caption - extra image 1:**

**Caption - extra image 2:**

## Analysis Questions

**Analysis Question 1.** Who signs the badge application: the service or the office? Who signs the finished badge? Why does signing an application not prove you have permission to use someone else’s website name? Write at least 3 sentences.

```text
(your answer here)
```

**Analysis Question 2.** You kept the same certificate but changed the name or office used for the check. Why did the answer change? Use the messages from both wrong-input tests. Write at least 3 sentences.

```text
(your answer here)
```

**Analysis Question 3.** Explain the difference between a server waiting at a door, a badge passing its checks, and messages traveling through a protected connection. If you see “connection refused,” what would you check first, and why? Write at least 3 sentences.

```text
(your answer here)
```

## Submission Checklist

- [ ] Fresh practice path recorded; no previous files or SSH configuration changed

- [ ] Dedicated service key and CSR created; issuer role explained

- [ ] Issued certificate fields and matching public components recorded

- [ ] Offline success, wrong-name failure, and wrong-CA failure explained

- [ ] Loopback listener address and passing/failing TLS evidence captured

- [ ] Listener stopped; connection failure distinguished from certificate failure

- [ ] Four-condition diagnosis and public/local comparison completed

- [ ] Analysis questions and walkthrough written in my own words

- [ ] Lab 01, notes, and reflection included for Portfolio Deliverable 3

- [ ] No private keys, credentials, or access URLs in the submission

- [ ] Worksheet committed to `week-09/labs/lab-02-build-and-test-digital-trust.md`

## GitHub / Lab Portal Submission

Your **portfolio repository** is your course-work folder on GitHub. A **commit** saves a set of changes there. Use the editing or upload method you practiced in Week 7; ask for a demonstration if you need one. Upload only the listed documents and reviewed images, never the whole VM folder.

1. Fill in this worksheet on this page and press **Save Progress** as you work. Your answers are stored in the portal and reload when you return.
2. Press **Submit to GitHub**. The portal commits the finished worksheet to the submission path shown at the top of your connected portfolio repository.
3. Add only the reviewed evidence images under `assets/screenshots/week-09/`, or paste their image addresses in the evidence fields above.
4. Complete the Week 9 Notes and Reflection worksheets in the portal the same way. Use the submissions guide to assemble Portfolio Deliverable 3.
5. Open the committed Markdown and every image on GitHub to confirm formatting and privacy.

*CyberVisionaries Institute · CyberFoundations · Tier I*
