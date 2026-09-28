# Cleartext FTP Credential Exposure Analysis with Wireshark

## Overview

This authorized lab demonstrated why plaintext application protocols create credential-exposure risk. I configured a temporary FTP service on a Windows virtual machine, created a synthetic test account, captured the login exchange from a Kali Linux system with Wireshark, filtered the traffic by protocol, and confirmed that both the username and password were visible in the packet data.

The exercise provided an introduction to packet capture and protocol analysis while showing the security difference between ordinary FTP and encrypted file-transfer options. This portfolio entry does not include the test credentials, server address, interface, port, commands, filenames, captures, or proprietary lesson instructions.

## Lab Environment

- Isolated virtual network
- Windows client/server simulation
- FileZilla Server
- Kali Linux analysis workstation
- Wireshark packet analyzer
- Temporary synthetic FTP account
- Plaintext FTP control connection

## Objectives

- Install and configure a temporary FTP service in a lab.
- Create a non-sensitive test identity.
- Capture network traffic with Wireshark.
- Filter a capture to isolate FTP communications.
- Identify application commands and server responses.
- Confirm that plaintext authentication exposes credentials.
- Compare FTP, FTPS, and SFTP.
- Recommend secure migration and detection controls.

## Authorization and Scope

All traffic was generated between purpose-built virtual machines in an isolated lab using a synthetic username and password. No production systems, public services, third-party accounts, real credentials, or personal data were involved.

Packet capture may collect unrelated credentials, communications, and personal information. It requires authorization, carefully selected interfaces and filters, minimum necessary retention, access controls, and redaction before evidence is shared.

## Plaintext Protocol Risk

Traditional FTP sends authentication commands and most session content without transport encryption. Anyone positioned to observe the traffic may be able to recover:

- Usernames and passwords
- Commands and server responses
- File and directory names
- File contents
- Server banners and implementation details
- Internal addresses and network structure

A strong password does not protect against passive observation when the protocol transmits it in readable form.

## FTP Server Configuration

The lab used FileZilla Server on the Windows VM. A temporary account was created solely for the exercise, and plaintext FTP was intentionally permitted so the exposure could be observed.

In a real environment:

- Administrative access to the server must be protected.
- Anonymous and guest access should be disabled unless explicitly required.
- User access should be scoped to approved directories.
- Write, delete, and rename permissions should follow least privilege.
- Plaintext authentication should be disabled.
- Server certificates and cryptographic settings should be managed securely.

The lab account and configuration were removed after testing.

## Packet Capture with Wireshark

Wireshark captured frames visible to the Kali system's selected virtual network interface. After the synthetic user authenticated, the capture was stopped and filtered to focus on FTP protocol traffic.

The filtered packets showed the authentication exchange at the application layer. Wireshark decoded the protocol fields, making the supplied account name and password readable in the packet details.

No packet capture or credential value is included in this repository.

## Why the Capture Was Possible

Capturing another system's traffic depends on network placement and design. Visibility may be provided by:

- A virtual network shared by the lab systems
- A switch mirror or SPAN port
- A network tap
- A hub or shared medium
- Capture on one of the communicating endpoints
- A gateway, firewall, or proxy on the traffic path
- Unauthorized interception such as spoofing or compromise

Modern switched networks do not normally deliver every unicast frame to every endpoint. The lab environment intentionally provided the visibility needed for authorized analysis.

## Packet Analysis Observations

The capture included multiple protocols beyond FTP, such as address resolution, service discovery, and TCP session establishment. This illustrated that a packet capture contains layered evidence:

- Ethernet source and destination
- IP addressing
- TCP ports, sequence information, and flags
- Application commands and responses
- Timing between events
- Other background network protocols

Protocol filtering reduced noise and helped isolate the authentication sequence.

## TCP Handshake Context

Before FTP application data was exchanged, TCP established a reliable session through its normal handshake. The capture provided visible examples of synchronization and acknowledgment flags before the client transmitted authentication information.

Understanding transport behavior helps analysts distinguish session setup, retransmission, closure, and application payload data.

## FTP, FTPS, and SFTP

| Protocol | Transport protection | Important distinction |
| --- | --- | --- |
| FTP | None by default | Credentials and content may be readable in transit. |
| FTPS | FTP protected by TLS | Retains FTP behavior but adds certificate-based transport encryption. |
| SFTP | File transfer over SSH | A different protocol that uses the SSH security model. |

FTPS and SFTP are not interchangeable. Client support, firewall behavior, identity integration, certificate or key management, and business requirements should guide migration.

## Encryption Enforcement

Wireshark does not make FTP encrypted and does not enforce FTPS. Encryption must be required by the file-transfer server, supported by the client, and reinforced through network and security policy.

Defensive actions include:

- Disable plaintext FTP listeners.
- Require FTPS with validated certificates or migrate to SFTP.
- Prevent clients from falling back to insecure modes.
- Block plaintext FTP at relevant network boundaries.
- Use managed file-transfer platforms for sensitive workflows.
- Monitor for failed TLS negotiation and downgrade attempts.

## Certificate and Key Management

For FTPS:

- Use a certificate whose identity matches the service.
- Protect the private key.
- Track expiration and renewal.
- Configure supported protocol versions and cipher suites.
- Ensure clients validate the certificate chain and name.
- Avoid teaching users to bypass certificate warnings.

For SFTP:

- Verify server host keys.
- Protect user private keys and use passphrases where appropriate.
- Remove stale authorized keys.
- Restrict key use by account, source, and command when practical.
- Rotate keys after suspected exposure.

## Credential Security

- Use unique test credentials that are never reused elsewhere.
- Do not place real passwords in packet-capture exercises.
- Store service secrets in an approved credential manager.
- Prefer multifactor or key-based authentication when supported.
- Disable dormant accounts and review access regularly.
- Treat credentials transmitted over plaintext protocols as compromised.
- Rotate exposed credentials from a trusted system.

## Network Detection Opportunities

- FTP control traffic on internal or external networks
- Authentication commands transmitted without TLS
- File-transfer sessions to unapproved servers
- New listening services on endpoints
- Outbound transfers inconsistent with the asset's role
- Repeated failed logins or username enumeration
- Large data transfers over legacy protocols
- Plaintext protocol use after a migration deadline
- Connections that bypass an approved managed file-transfer gateway

Flow logs, intrusion detection, firewall records, packet metadata, and endpoint telemetry can support detection.

## Endpoint Detection Opportunities

- Installation of an unapproved FTP server
- Creation of new file-transfer accounts
- Changes to service configuration or listening interfaces
- Service execution from user-writeable paths
- Unexpected access to sensitive directories
- New firewall rules for legacy protocols
- Cleartext clients launched by unusual users or processes
- Credential files or server configuration stored insecurely

## Related Plaintext Protocols

The same security principle applies to other legacy or unprotected services, including:

- Telnet
- Unencrypted HTTP authentication
- Older SNMP versions using community strings
- POP3, IMAP, or SMTP without TLS
- Cleartext database connections
- Unencrypted directory and management protocols

Each should be replaced, upgraded, tunneled securely, or isolated according to business requirements.

## Incident Response

If plaintext credential exposure is discovered:

1. Identify affected protocols, systems, users, and time range.
2. Preserve relevant network and endpoint evidence.
3. Disable the insecure service or restrict it immediately when feasible.
4. Rotate all credentials observed or potentially exposed.
5. Review authentication logs for unauthorized use.
6. Hunt for credential reuse across other systems.
7. Deploy an encrypted replacement and validate client configuration.
8. Monitor for continued plaintext traffic.
9. Document scope, impact, and remediation evidence.

Changing the password without removing the plaintext channel only repeats the exposure.

## Evidence Handling

Packet captures should be treated as sensitive because they may contain:

- Passwords and session tokens
- Personal or business communications
- File contents
- Internal addressing and hostnames
- Application and operating-system details

Captures should be minimized, encrypted, access controlled, hashed when used as evidence, redacted before sharing, and destroyed according to retention policy.

## Validation After Remediation

- Confirm the plaintext listener is disabled.
- Verify clients cannot connect without encryption.
- Validate certificates or SSH host keys.
- Capture a new test session and confirm credentials are not readable.
- Check that firewall rules block the legacy protocol where required.
- Review logs for downgrade or fallback attempts.
- Verify authorized users retain required file-transfer functionality.
- Remove temporary test accounts and data.

Encrypted packet payloads should appear unreadable to ordinary passive capture, although metadata remains visible.

## Security Concepts Demonstrated

- Packet capture and protocol analysis
- FTP authentication
- Plaintext credential exposure
- TCP handshake interpretation
- Wireshark display filtering
- FTPS and TLS
- SFTP and SSH
- Network segmentation
- Credential hygiene
- Legacy protocol detection
- Incident response
- Evidence protection

## Results

- Installed and configured a temporary FTP service in the isolated lab.
- Created a synthetic account for controlled testing.
- Captured an FTP authentication session with Wireshark.
- Filtered the capture to locate relevant protocol messages.
- Confirmed that the username and password were readable in transit.
- Compared FTP with FTPS and SFTP.
- Developed migration, monitoring, credential-response, and validation recommendations.

## Key Takeaways

- Strong passwords cannot compensate for a protocol that sends them in plaintext.
- Wireshark reveals what is present on the network; it does not provide transport encryption.
- Switched-network capture depends on endpoint or network-path visibility.
- FTPS and SFTP both provide encryption but use different protocols and trust models.
- Exposed plaintext credentials should be rotated after the insecure service is removed or corrected.
- Packet captures are sensitive evidence and must be protected.
- Migration is complete only when clients cannot fall back to the legacy plaintext protocol.

## Skills Demonstrated

- Wireshark packet capture
- Protocol display filtering
- FTP session analysis
- Cleartext credential exposure validation
- TCP flag and handshake interpretation
- FileZilla Server configuration analysis
- FTPS and SFTP comparison
- Legacy protocol risk assessment
- Network and endpoint detection strategy
- Credential incident response
- Evidence handling
- Professional cybersecurity documentation
