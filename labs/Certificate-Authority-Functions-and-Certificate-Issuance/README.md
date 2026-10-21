# Certificate Authority Functions and Certificate Issuance

## Overview

This learning case study reviews a narrated OpenSSL demonstration of requesting and issuing a web server certificate. The focus is on how certificate authorities (CAs), registration authorities (RAs), and certificate signing requests (CSRs) support public key infrastructure (PKI).

This is a student demonstration review, not a production CA deployment or an independently verified certificate issuance exercise. The supplied lesson is summarized in original language; its full transcript, organization-specific details, and private material are not published here.

## Environment and Tools Reviewed

- Kali Linux as the demonstration platform.
- OpenSSL for key generation, CSR creation and inspection, and certificate signing.
- A simulated certificate applicant and an illustrative CA with an existing certificate and private key.
- X.509 certificates, trust anchors, intermediate CAs, and certificate lifecycle concepts.

## Objectives

- Explain the separate responsibilities of a CA and an RA.
- Follow the relationship between a private key, CSR, and signed certificate.
- Understand certificate chains and the limits of shared trust.
- Identify controls needed beyond a simplified issuance demonstration.

## CA and RA Responsibilities

A CA signs certificates that associate a public key with a subject according to its validation policy. It also manages issuance records and lifecycle activities such as renewal and revocation. An RA can check applicant identity and authorization before an approved request reaches the CA; this function may be integrated with the CA rather than operated as a separate service.

The certificate is an assertion under a particular policy, not a guarantee that its holder is safe or authorized for every action. Domain validation establishes control of a domain; it does not, by itself, establish a legal organization's identity.

## Demonstration Summary

1. **Generate the applicant key:** The demonstration describes creating a 2048-bit RSA private key. The corresponding public key is included in the request; the private key remains with its owner.
2. **Create the CSR:** The applicant supplies subject information and signs the request with the private key. A CSR carries the public key and requested attributes, not the private key.
3. **Inspect the request:** OpenSSL displays the CSR contents for review. The transcript describes this inspection but does not provide independently verifiable command output.
4. **Submit the request:** Transfer to the CA is simulated rather than demonstrated.
5. **Validate the applicant:** Domain control and organization checks are assumed complete. Their execution and results are not evidenced.
6. **Issue the certificate:** The CA-side demonstration describes signing the request using an existing CA private key and producing a certificate with a stated 365-day validity period. This is a demonstration parameter, not a recommendation for current publicly trusted certificate lifetimes.

The supplied text refers to commands displayed in the lesson but does not include their full syntax. No reconstructed commands or invented results are presented as executed work.

## CSR Contents and Review

A CSR can include a subject distinguished name, public key information, and requested extensions. Common subject fields describe an organization and location, but required fields depend on the certificate profile and validation policy. CSRs can use binary DER or text PEM encoding; Base64 text is not the only representation. OpenSSL documents these request formats and inspection options in its [request utility reference](https://docs.openssl.org/3.5/man1/openssl-req/).

Before issuance, a reviewer should check requested names, key properties, and authorization. A valid CSR signature demonstrates possession of the corresponding private key; it does not prove domain control or the truth of every requested identity field. The CA must apply its own policy rather than blindly copy requested extensions.

For TLS service identity, DNS names belong in the Subject Alternative Name (SAN) extension. A Common Name alone is not a substitute for SAN-based identity matching under [RFC 9525](https://www.rfc-editor.org/rfc/rfc9525).

## Chain of Trust

```text
Trusted root CA
    |
    +-- Intermediate CA
            |
            +-- Web server certificate
```

The root becomes a trust anchor through a client's trust configuration, not merely because it is self-signed. An intermediate can issue certificates within its permitted constraints. A private CA requires deliberate trust distribution to managed clients; a public CA is trusted only where the relevant trust anchor and policies are accepted.

Path validation involves signatures, validity periods, constraints, and certificate purpose. TLS clients also check the requested service identity. A common root does not automatically grant two certificate holders application access to each other. [RFC 5280](https://www.rfc-editor.org/rfc/rfc5280) defines the foundational certificate, CRL, and certification-path validation framework.

## Lifecycle and Security Considerations

- Protect applicant and CA private keys with restricted access and suitable key storage. Never include them in a portfolio repository.
- Separate root and issuing-CA responsibilities where appropriate, and maintain auditable issuance and approval records.
- Track expiration and plan renewal, replacement, and compromise response.
- Revocation status distribution through CRLs or OCSP requires additional infrastructure and client behavior. Signing a certificate does not automatically configure either service.
- Installing a certificate alone does not demonstrate secure TLS. Server configuration, the certificate chain, key access controls, and client validation still require testing.

These are analysis points and production considerations, not controls implemented in this review.

## Results and Limitations

The lesson illustrates the progression from applicant key generation to CSR inspection and CA signing. It helped clarify the difference between requesting identity claims and validating them before issuance.

No original key files, CSRs, certificates, screenshots, or terminal output were supplied with the transcript. Consequently, this case study does not independently confirm certificate contents, SAN inclusion, successful chain validation, browser trust, or a working HTTPS endpoint. Root and intermediate CA deployment, real applicant validation, revocation publication, and OCSP operation were not demonstrated. No production security outcome is claimed.

## Key Takeaways

- Private keys stay private; CSRs and certificates serve different purposes.
- Identity checks are essential even when the cryptographic request is well formed.
- Trust depends on client configuration, certificate policy, and validation, not simply on possessing a signed certificate.
- A short issuance demonstration is only one part of operating a secure certificate lifecycle.

## Skills and Concepts Reviewed

- PKI and certificate lifecycle analysis.
- CA and RA responsibility separation.
- OpenSSL workflow interpretation and CSR review.
- Certificate chains, trust anchors, and TLS identity validation.
- Evidence-based technical documentation and private-key handling awareness.

## Sources

- User-supplied CA functions lesson transcript: learning source, paraphrased rather than reproduced.
- [OpenSSL: openssl-req](https://docs.openssl.org/3.5/man1/openssl-req/) — request generation, formats, and inspection.
- [RFC 5280: X.509 PKI Certificate and CRL Profile](https://www.rfc-editor.org/rfc/rfc5280) — certificate structure and path validation.
- [RFC 9525: Service Identity in TLS](https://www.rfc-editor.org/rfc/rfc9525) — service name representation and verification.
