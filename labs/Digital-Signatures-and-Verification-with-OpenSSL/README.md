# Digital Signatures and Verification with OpenSSL

## Overview

This learning exercise reviewed a supplied OpenSSL demonstration of creating a signing key pair, signing a synthetic message using SHA-256, and verifying the resulting detached signature with the corresponding public key.

The lesson reported successful verification. This entry analyzes that demonstration; it does not claim an independently executed test, deployed email-security system, or verified identity infrastructure. No private keys, signature artifacts, source filenames, or proprietary course instructions are published.

## Environment and Tools Reviewed

- OpenSSL command-line utilities
- A private signing key and corresponding public key
- A synthetic text message
- SHA-256 hashing
- A detached binary signature

The transcript did not provide complete command text or reusable artifacts. Exact key algorithm, key size, padding, and OpenSSL version are not asserted here.

## Objectives

- Explain signing and verification roles.
- Distinguish integrity, authenticity, confidentiality, and non-repudiation.
- Understand the importance of public-key trust.
- Interpret a successful verification result within its limits.
- Identify key-management and validation requirements.

## Demonstration Summary

The supplied lesson showed:

1. Generating a private signing key.
2. Deriving its corresponding public key.
3. Creating a synthetic message.
4. Producing a detached signature using the private key and SHA-256.
5. Verifying the signature against the message using the public key.

The recipient needs the signer's public key for verification, not the signer's private key. A recipient does not need a separate key pair merely to verify a signature.

## Security Properties

| Property | Role of a digital signature | Additional dependency |
| --- | --- | --- |
| Integrity | Detects changes to the signed content during verification | Secure algorithms and verification of the intended bytes |
| Authenticity | Links a signature to a corresponding signing key | Trusted association between the public key and the claimed signer |
| Confidentiality | Does not hide signed content | Separate encryption when secrecy is required |
| Non-repudiation | Can support evidence of origin | Key custody, identity assurance, policy, records, and relevant legal context |

NIST's Digital Signature Standard describes signatures as supporting detection of unauthorized modification and authentication, including evidence relevant to third-party disputes. These properties do not eliminate operational trust requirements. [NIST FIPS 186-5](https://csrc.nist.gov/pubs/fips/186-5/final)

## Signing Is Not General-Purpose Encryption

Signature schemes use algorithm-specific signing and verification operations. Describing every signature as an encrypted digest that is decrypted with a public key is misleading, particularly for schemes that do not resemble encryption.

OpenSSL exposes signing and verification through dedicated operations, with supported algorithms and options varying by interface. Its digest utility documents signing with a private key and verifying with a public key. [OpenSSL digest and signature documentation](https://docs.openssl.org/3.0/man1/openssl-dgst/)

Hashing contributes a compact representation of content in many schemes. A cryptographic digest is designed to resist collisions; it is not guaranteed to be unique across all possible messages.

## Public-Key Trust

A valid signature under an arbitrary public key does not prove that a named person signed the message. If an attacker can substitute both the message and the supplied public key, a self-consistent verification can still be misleading.

Public keys should be bound to identities through an appropriate trust mechanism, such as a validated certificate chain, an authenticated directory, or an independently confirmed fingerprint. Certificate-based verification also requires applicable validity, usage, and revocation checks.

The supplied demonstration did not show certificate validation, fingerprint confirmation, or a trusted key-distribution process.

## Interpreting Verification

A successful verification indicates that the supplied signature is valid for the supplied message and public key under the selected scheme. It does not prove:

- The human signer personally initiated the action
- The private key was never copied or compromised
- The message was fresh rather than replayed
- The signed content is factually correct or safe
- A certificate remains trusted or unrevoked
- The message was encrypted

Verification should cover the exact intended content. Encoding changes, line-ending changes, or omitted fields can affect what is actually signed and verified.

## Non-Repudiation and Key Custody

A private key can be stolen, shared, or used by compromised software. Attribution therefore needs more than mathematical verification. Relevant safeguards include protected key storage, access controls, hardware-backed signing where justified, audit records, incident procedures, and trusted timestamps when required.

The lab illustrated the cryptographic relationship between keys and signatures. It did not establish legal non-repudiation or exclusive human control of a signing key.

## Confidentiality Is Separate

Signed data can remain readable to anyone who receives it. Confidentiality requires a separate encryption design. Secure messaging systems commonly combine public-key mechanisms with symmetric content encryption rather than directly encrypting an entire document using a public-key operation.

Recipient-key validation and correct protocol use remain necessary. The supplied walkthrough did not demonstrate message encryption or an operational secure-email exchange.

## Proposed Follow-Up Tests

A controlled follow-up could check that verification fails after modifying the message, using an unrelated public key, or corrupting the signature. It could also inspect failure handling, key-file permissions, and a trusted public-key distribution path.

These are proposed tests, not completed results. No commands, output, or artifacts were invented to imply they occurred.

## Results and Limitations

The supplied demonstration reported successful verification of its synthetic message and detached signature. It illustrated signing with a private key and verification with a corresponding public key.

No independent execution, tampering test, certificate deployment, revocation check, trusted timestamp, email integration, or key-compromise response was provided.

## Key Takeaways

- Keep the private signing key protected; distribute only the public verification key.
- Signature validity and trust in the signer's identity are separate questions.
- Signing provides no confidentiality by itself.
- Non-repudiation depends on operational evidence and key custody.
- Validate altered-message and wrong-key failures before relying on an implementation.

## Skills and Concepts Reviewed

- Asymmetric cryptography and signing-key roles
- SHA-256 and detached signatures
- OpenSSL verification concepts
- Public-key trust and identity binding
- Integrity and confidentiality distinctions
- Key custody and evidence limitations

## Sources

- Supplied digital-signatures lesson transcript, summarized in original language without redistributing course artifacts.
- [OpenSSL: digest operations and digital signatures](https://docs.openssl.org/3.0/man1/openssl-dgst/)
- [NIST: Digital Signature Standard, FIPS 186-5](https://csrc.nist.gov/pubs/fips/186-5/final)
