# Week 8 Notes — Practical Cryptography

**Student Name:** Kay

**Date:** 9/12/2026

## Vocabulary in My Own Words

- Plaintext: The original readable data before it's protected. Anyone can open it and understand it. 
- Ciphertext: Data after it's been encrypted. It looks like scrambled nonsense and can't be read without the key.
- Encryption: The process of scrambling data so only someone with the right key or passphrase can read it. It protects privacy.
- Hash / digest: A fixed fingerprint made from a file's contents. If the file changes even a little, the fingerprint changes, so it's used to check if data was altered.
- Public key: The key you can share freely. Others use it to verify you or send you protected data.
- Private key: The secret key you keep on your own machine. It proves who you are, so it must never be shared.
- Digital signature: A way to prove a message really came from you and wasn't changed, made using your private key.
- `authorized_keys`: A file on a server that lists the public keys allowed to log in. If your public key is in it, you can connect without a password.

## Command-to-Purpose Map

| Command | What it demonstrated |
| --- | --- |
| `openssl enc` | Encrypted a file so its contents couldn't be read without the passphrase (confidentiality).  |
| `sha256sum` |Created a hash of a file to check whether its contents had changed (integrity).   |
| `ssh-keygen` |Generated a public/private key pair for secure login.   |
| `openssl dgst` |Created a digest or signature of a file to verify its integrity and origin.   |
| `ssh ... -o PasswordAuthentication=no` |Forced login to use keys only, not passwords, which is more secure.   |


## Safety Rules I Must Remember

1. Never share or display my private key. It stays on my machine only.
2. Keep my passphrases secret and don't write them where others can find them.
3. Only share the public key (the .pub file), never the private one.



## Question for the Instructor
none at this time
