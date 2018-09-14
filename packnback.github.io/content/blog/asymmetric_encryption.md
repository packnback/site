---
title: "Asymmetric cryptography"
date: 2018-09-14T09:25:10+12:00
type: "blog_post"
draft: false
---


Rather than spend time explaining (poorly) how public/asymmetric key cryptography
works, I'll just summarize the parts we will be using.

- A key is two parts, public and private.
- A message can be encrypted and addressed to a key.
- The public part of a key can be freely distributed.
- An encrypted message can only be decrypted by the corresponding private key part.
- A message can be 'signed' by someone with the private key part.
- A message signature can be verified as authentic using the corresponding public key part.

# Encrypted email

A good example of a model we could emulate is gpg encrypted email. Anyone can send
encrypted emails to an email server, but the email contents are encrypted and addressed
to the recipients key pair. Only the recipient can read the message. At the same time,
the sender could sign the message, meaning the recipient could verify the authenticity
of the message. In this model, the server is merely a store and forward relay, which
seems perfect for a backup tool where the server is not fully trusted.

# How packnback should work

- A backup server allows public keys to send encrypted data.
- Only the holder of the recipient key is allowed to access, delete, or download data.
- The server is only responsible for access control by verifying key holders.
- The backup recipient fetches data and decrypts it client side like an email server.
- The server cannot decrypt data.
- If the server modified data on the server, message signatures would be invalid, alerting
  the backup recipient.

# Prototype code

A high performance prototype tool was written here:

https://github.com/andrewchambers/asymcrypt
https://github.com/andrewchambers/asymcrypt/blob/master/README.md
https://github.com/andrewchambers/asymcrypt/blob/master/asymcrypt_formats.5.md

This prototype is one of our first building blocks for public key encrypted deduplicated
backups. The data formats are documented there.

### Why not use GPG?

- Benchmarks showed that GPG was very slow compared
  to the prototype.
- In the authors opinion GPG has a poor interface for both
  human and automated interfaces.
- GPG file formats more complicated than needed.
- GPG can always be used as a master key, encrypting
  an asymcrypt private key.
- GPG is much more code, so is far harder to audit.

### Why use C in 2018?

- For this tool in particular, startup time and memory use are extremely important.
- Other portions of the code may not be in C.
- In a future post I will attempt to verify that the C
  code has no undefined behavior using frama-c.
- Failing formal verification, the language may change, but the tool specifications will be unchanged.
