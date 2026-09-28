# Secure Remote Administration with OpenSSH and PuTTY

## Overview

This lab established an encrypted remote-administration session from a Windows virtual machine to a Kali Linux virtual machine. I installed and started OpenSSH Server on Kali, created harmless test directories to validate remote access, identified the lab host's private address, connected from Windows with PuTTY, reviewed the initial SSH host-key prompt, authenticated with a lab account, and confirmed that commands executed on the remote Linux system.

The exercise demonstrated why SSH should replace plaintext Telnet for command-line administration. This portfolio entry does not include the supplied default username or password, host address, commands, directory names, download links, fingerprints, or proprietary lesson instructions.

## Lab Environment

- Isolated virtual network
- Kali Linux SSH server
- Windows client virtual machine
- OpenSSH Server
- PuTTY SSH client
- Temporary non-sensitive test directories

## Objectives

- Explain the security difference between SSH and Telnet.
- Install and start an SSH service on Linux.
- Confirm that the server is reachable on the intended interface.
- Connect from Windows using an SSH client.
- Understand SSH host keys and fingerprint verification.
- Authenticate and execute limited remote commands.
- Apply least privilege, firewall restrictions, and secure authentication.
- Configure logging, monitoring, and service lifecycle controls.

## Authorization and Scope

All activity occurred between purpose-built virtual machines on an isolated network. No public server, production host, third-party device, real administrator account, or personal credential was used.

Enabling a remote administration service increases attack surface. Production deployment requires an approved management design, restricted source networks, hardened authentication, logging, patching, and documented ownership.

## Why SSH Replaces Telnet

Telnet sends authentication and session content without transport encryption. A network observer may be able to recover usernames, passwords, commands, and output.

SSH provides:

- Encrypted transport
- Server authentication through host keys
- Password or public-key user authentication
- Integrity protection
- Secure file transfer and tunneling capabilities
- Auditable remote administration

Encryption protects session content in transit, but it does not make weak credentials, an unverified host key, or an overprivileged account safe.

## OpenSSH Server Deployment

The Kali system required the server component because it was accepting inbound SSH connections. Installing only a client would allow outbound connections but would not create a listening remote-administration service.

After installation, I started the service for the lab. A production design should also determine:

- Whether the service should start automatically
- Which interfaces and addresses it should bind to
- Which users or groups may authenticate
- Which authentication methods are allowed
- Whether interactive shells, forwarding, and file transfer are required
- Which source networks may reach it

## Service State and Persistence

Starting a service for the current session and enabling it at boot are different administrative actions. A temporary lab may need the service only while testing, whereas a managed server may require persistent startup.

Administrators should verify:

- Service status
- Listening sockets
- Startup configuration
- Effective server configuration
- Firewall policy
- Logs after a test connection

Unused services should be stopped and disabled.

## Remote Access Validation

The Kali VM contained a small directory structure created solely as a validation marker. After the Windows client established an SSH session, I navigated the remote file system and confirmed that the same directories were visible.

This demonstrated that commands typed in the PuTTY window were executed in the authenticated user's context on Kali rather than on the Windows client.

No test directory name or command sequence is included.

## PuTTY Client

PuTTY is a Windows SSH client that can open remote terminal sessions and save connection profiles. The lab used it to connect to the Kali host over the isolated network.

Modern Windows systems may also include an OpenSSH client, and organizations may use managed terminal platforms, PowerShell remoting, bastion hosts, or privileged-access systems. Tool selection should match the operating environment and governance requirements.

## SSH Host Keys

An SSH server uses a host key to prove its identity. On a first connection, PuTTY presents the server's host-key fingerprint because the client has no trusted prior record.

The client does not create the server's host key during this prompt. Instead, the user decides whether to trust and store the presented key.

The fingerprint should be verified through an independent trusted channel before acceptance. Blindly accepting it creates exposure to a man-in-the-middle attack.

## Trust on First Use

Many SSH clients use a trust-on-first-use model:

1. The first observed host key is reviewed and stored.
2. Future connections compare the presented key with the stored value.
3. A changed key triggers a warning.

A changed key may indicate legitimate reinstallation or key rotation, but it can also indicate interception or redirection. The warning should be investigated rather than bypassed automatically.

## Credential Security

The prebuilt lab image used publicly documented initial credentials. Those values were intentionally omitted and should be changed immediately.

Recommended controls include:

- Use long, unique credentials.
- Disable default and unused accounts.
- Restrict which users or groups can use SSH.
- Avoid direct privileged-account login.
- Apply multifactor authentication where feasible.
- Use a password manager or privileged-access system.
- Monitor repeated failures and unusual login sources.
- Rotate credentials after suspected exposure.

## Public-Key Authentication

Public-key authentication can reduce password-guessing risk when keys are generated, stored, and governed correctly. A secure process should:

- Use an approved modern key type and sufficient strength.
- Protect private keys with a passphrase when appropriate.
- Store private keys in protected user or hardware-backed locations.
- Distribute public keys through an authenticated process.
- Restrict file permissions.
- Remove stale keys promptly.
- Maintain owner, purpose, and expiration metadata.
- Avoid copying one private key across many administrators.

Keys do not eliminate the need for account lifecycle management.

## Server Hardening

- Keep OpenSSH and the operating system updated.
- Disable obsolete protocol and cryptographic options.
- Permit only required users or groups.
- Disable direct privileged login.
- Prefer key-based or multifactor authentication for administration.
- Limit authentication attempts and session duration appropriately.
- Disable forwarding, tunneling, graphical forwarding, or file transfer when unnecessary.
- Configure idle timeouts with operational care.
- Display approved legal banners where required.
- Protect host private keys and rotate them through a controlled process.

## Firewall and Network Scope

SSH should not be reachable from every network merely because the service is secure. Access should be limited through:

- Host firewall rules
- Network access-control lists
- Management VLANs
- Bastion or jump hosts
- VPN or zero-trust access controls
- Source-address restrictions
- Just-in-time access workflows

Direct exposure to the public internet should be avoided when a controlled management path is available.

## Least Privilege

The remote session inherited the permissions of the authenticated Kali user. Administrative elevation should be performed only for approved tasks and should be logged.

Good practices include:

- Use named individual accounts.
- Separate routine and privileged identities.
- Grant only required command or group privileges.
- Avoid shared administrator accounts.
- Use controlled elevation rather than persistent privileged shells.
- Review authorized keys and group membership regularly.

## Logging and Monitoring

Relevant SSH events include:

- Successful and failed authentication
- Invalid users
- Source addresses
- Host-key changes
- Service start and stop events
- Privilege elevation after login
- Session duration
- Forwarding and tunneling activity
- Configuration changes

Logs should be forwarded to protected centralized storage where appropriate and correlated with firewall, endpoint, identity, and network telemetry.

## Detection Opportunities

- Repeated failed logins from one or many sources
- Successful login after numerous failures
- Access from a new device, region, or network
- Privileged login at an unusual time
- New authorized keys
- Direct privileged-account authentication
- Unexpected port forwarding or tunneling
- SSH launched by an unusual parent process
- Remote sessions to systems without an approved management need
- Service activation on endpoints that do not normally host SSH

## Windows Remote Administration Nuance

Remote Desktop is one Windows administration option, but it is not the only one. Windows can also support:

- OpenSSH
- PowerShell remoting
- Windows Admin Center
- Remote management protocols through protected channels
- Endpoint-management and privileged-access platforms

Likewise, Linux and network devices may offer web or API management in addition to command-line SSH. The secure choice depends on business need, protocol hardening, identity controls, and auditability.

## Validation Strategy

A complete test should verify:

1. The SSH service listens only where intended.
2. Approved clients can connect.
3. Unapproved source networks are blocked.
4. The host-key fingerprint matches the authoritative record.
5. Default or disallowed accounts cannot authenticate.
6. The user receives only intended privileges.
7. Logging captures authentication and session events.
8. The service stops or remains persistent according to design.
9. Disabled features such as forwarding remain unavailable.

## Incident Response

If unauthorized SSH access is suspected:

1. Preserve authentication, process, network, and privilege logs.
2. Identify the account, source, session duration, and commands executed.
3. Disable or contain the affected account and host when necessary.
4. Revoke compromised passwords, keys, and active sessions.
5. Review authorized-key files and server configuration.
6. Hunt for persistence, tunneling, file transfer, and lateral movement.
7. Validate host-key integrity and management DNS records.
8. Rebuild the host when system integrity cannot be established.
9. Close the access-control gap and monitor for recurrence.

## Safe Lab Cleanup

- Stop or disable the SSH service if it is no longer required.
- Remove temporary test directories.
- Remove saved PuTTY sessions containing sensitive details.
- Delete unneeded cached host keys only through a documented process.
- Remove temporary user keys and accounts.
- Restore firewall and network settings.
- Revert or snapshot the virtual machines according to the lab plan.

## Security Concepts Demonstrated

- Secure remote administration
- SSH and Telnet comparison
- OpenSSH Server
- PuTTY client operation
- Host-key authentication
- Trust on first use
- Password and public-key authentication
- Management-plane segmentation
- Least privilege
- Firewall scoping
- Authentication logging
- Incident response

## Results

- Installed and started OpenSSH Server on the Kali VM.
- Verified that the service was available on the isolated network.
- Connected from Windows using PuTTY.
- Reviewed the first-connection host-key trust prompt.
- Authenticated with a non-production lab account.
- Confirmed remote command execution through harmless directory validation.
- Developed credential, host-key, firewall, logging, and response recommendations.

## Key Takeaways

- Telnet should be replaced because it exposes credentials and commands in transit.
- SSH protects transport confidentiality and integrity but still requires strong identity controls.
- A first-connection prompt asks the user to trust the server's existing host key; it does not generate that key.
- Host-key fingerprints should be verified through an independent trusted source.
- Publicly documented default credentials must be replaced immediately.
- SSH access should be limited to approved accounts and management networks.
- Remote administration is safest when identity, network, endpoint, and logging controls work together.

## Skills Demonstrated

- OpenSSH Server deployment
- Linux service management concepts
- PuTTY SSH client usage
- Remote Linux administration
- SSH host-key verification
- Password and public-key security
- Management-plane access design
- Host firewall and source scoping
- SSH logging and detection strategy
- Least-privilege administration
- Incident-response planning
- Professional cybersecurity documentation
