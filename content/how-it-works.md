---
title: How it works?
slug: how-it-works
---

Remote code signing enable *sig4all* to offer a lot of features that couldn't be achieved otherwise. Such features include 2fa on signing requests, organization shared private keys, rights management for organization, and rate limiting. In order to enforce that, we never send the private key to the client, and every signing must take place on the server.

In order to better understand how the technology works, let's run through the process when signing a binary.

## Client side hashing

The first step for the client is to create the cryptographic digest of it's document. This might be as simple as running all the bytes of the documents in a cryptographic hash function (e.g., SHA256), but is often a bit more complicated. For instance, when signing Windows executable, or Android APKs.

It's important to understand that this process is not security sensitive. Everybody can and in fact should, calculate the same digest for the same file. Lying about the value of this digest is also not an issue, because the signature at the end of the process won't authenticate the document.

When completed, the client has digest, which is usually between 20 bytes to 64 bytes. He send this information to the server.

## Server creating the signature

After receiving the digest from the client, the server will use the appropriate private key and certificate, to construct the signature. This signature include different information, such as the name of the signer, the public key, and very importantly, contains a value encrypted with the private key.

This value ensure that the constructed signature can't be modified or forged in any meaningful way. Once created, the document doesn't contains sensitive information, and is returned to the client. While not fixed, the size of the signature is usually around few KBs.

## Embedding the signature with the target document.

When the client received the signature created by the server, he may need to embed it with the document. The technical details are specific to each file formats, but in the case of a Windows executable, it is written in the same file, following a specific technique.
