# Analyzing Output from Endpoint Monitoring Tools

## Overview

This lab examined how endpoint and network telemetry can reveal malicious behavior that signature-based security controls may miss. In an isolated Hyper-V environment, I analyzed a controlled phishing and Trojan-delivery scenario using NetworkMiner and the Microsoft Sysinternals suite. I correlated packet data, file hashes, process relationships, integrity levels, network connections, Sysmon events, and autorun entries to identify execution and persistence activity.

The exercise emphasized defensive analysis and incident investigation. No malware, credentials, internal addressing, challenge answers, or operational exploitation instructions are included in this repository.

## Lab Environment

- Hyper-V virtualized lab
- Windows endpoint under investigation
- Virtualized routing and internal network segments
- Windows domain services
- Security-testing workstation
- NetworkMiner
- Microsoft Sysinternals Sysmon
- Microsoft Sysinternals Process Explorer
- Microsoft Sysinternals Autoruns
- Windows Event Viewer
- Metasploit Framework in a controlled simulation

## Objectives

- Analyze packet captures for transferred files and suspicious hosts.
- Generate cryptographic hashes for threat-intelligence comparison.
- Establish a baseline of legitimate endpoint processes.
- Configure persistent endpoint telemetry with Sysmon.
- Identify anomalous process execution and outbound connections.
- Reconstruct a persistence timeline from endpoint logs.
- Locate and remove an unauthorized startup entry.
- Recommend controls that reduce the likelihood and impact of similar activity.

## Scenario

A controlled phishing page presented a modified executable as legitimate software. The lab simulated a user downloading and running the file while a testing system listened for a reverse connection. This demonstrated how an attacker may take advantage of trusted-looking metadata and permissive outbound filtering.

All activity occurred in an authorized, isolated training environment. The sample was not executed outside the lab, and no malicious files are stored in this portfolio.

## Packet Analysis with NetworkMiner

I captured traffic from the Windows endpoint and generated web activity associated with the simulated phishing site. NetworkMiner reconstructed the transferred web content and identified the downloaded executable in the packet stream.

The analysis included:

- Reviewing discovered hosts and basic fingerprinting data.
- Identifying files transferred over the monitored protocol.
- Extracting the suspicious executable from captured traffic.
- Calculating MD5, SHA-1, and SHA-256 hashes.
- Considering how hashes could support reputation and threat-intelligence lookups.

File hashes are useful indicators, but they are not conclusive on their own. Minor changes to a binary produce a different hash, so effective detection also requires behavioral and contextual evidence.

## Configuring Endpoint Logging and Monitoring

I configured Sysmon with a community-maintained monitoring profile to create durable endpoint telemetry. Unlike live-only inspection, Sysmon records selected security-relevant events for later investigation and correlation.

Before executing the sample, I used Process Explorer to establish a known-good baseline. I reviewed legitimate processes, parent-child relationships, user context, and integrity levels. This baseline provided a reference for identifying deviations after the simulated compromise.

## Analyzing the Suspicious Process

After controlled execution, I examined the suspicious process in Process Explorer and correlated its attributes with the testing system. Key observations included:

- An execution chain that differed from the expected launch path of the legitimate application.
- Legitimate-looking description, company, and publisher metadata that did not prove the file was trustworthy.
- Execution in the logged-in user's context at medium integrity.
- A process identifier that could be correlated across tools and logs.
- An active outbound TCP connection to the controlled testing system.

The network connection could also be validated through connection tables, packet capture, firewall telemetry, or other network-monitoring tools. Combining these sources provides stronger evidence than relying on a filename or process description.

## Analyzing the Persistence Mechanism

The scenario then simulated user-level persistence. After restarting the endpoint and signing in again, I used the Sysmon Operational log to reconstruct the relevant sequence of events:

1. The original process created another executable in a temporary location.
2. The new executable started as a separate process.
3. A per-user startup registry value was created.
4. The temporary process terminated.
5. The startup entry caused the program to run when that user signed in.

The evidence showed persistence within a single user context rather than elevated or system-wide persistence. The dropped file and visible application behavior also created discoverable artifacts.

I verified the startup entry with Autoruns and removed it as part of remediation.

## Indicators of Compromise

The investigation identified several behavioral and host-based indicators:

- A suspicious executable delivered from an untrusted web source
- File content reconstructed from network traffic
- Cryptographic hashes associated with the downloaded binary
- An unexpected process ancestry chain
- Legitimate-looking metadata on an untrusted executable
- An outbound connection to an unexpected system
- A newly created executable in a temporary directory
- Creation of a per-user startup registry entry
- Process creation and termination events near the persistence activity
- Program execution after user sign-in

No environment-specific indicator values are published here.

## Security Considerations

Real-time endpoint protection was temporarily adjusted only to permit the controlled exercise. In a production environment, weakening endpoint controls creates significant risk and should require documented authorization, compensating safeguards, strict isolation, and immediate restoration.

Controls that can reduce exposure include:

- Enforcing application allowlisting and reputation-based protection
- Restricting outbound traffic by destination, application, and protocol
- Using protocol-aware inspection instead of trusting port numbers alone
- Monitoring process creation, network connections, and registry changes
- Alerting on executables launched from temporary or user-writable locations
- Detecting suspicious parent-child process relationships
- Applying least privilege and limiting local administrative access
- Segmenting management, server, user, and testing networks
- Centralizing endpoint logs in a SIEM for correlation and retention
- Using modern endpoint detection and response capabilities

## Security Concepts Demonstrated

- Defense in depth
- Behavioral malware analysis
- Endpoint detection and response fundamentals
- Network and host telemetry correlation
- Process ancestry analysis
- Integrity-level and privilege-context assessment
- Persistence detection
- Incident timeline reconstruction
- Indicators of compromise
- Egress filtering
- Application control
- Evidence-based remediation

## Results

- Captured and reconstructed a suspicious file transfer with NetworkMiner.
- Generated multiple cryptographic hashes for the downloaded executable.
- Established a baseline of expected Windows process activity.
- Configured Sysmon to retain security-relevant endpoint events.
- Identified suspicious execution context and an outbound connection.
- Correlated file, process, network, and registry evidence.
- Confirmed user-level persistence after restart and sign-in.
- Located and removed the unauthorized autorun entry.
- Completed the scenario without publishing sensitive lab information or malicious artifacts.

## Key Takeaways

- Trustworthy-looking filenames and metadata do not establish that a binary is safe.
- Hashes are valuable for enrichment, but behavioral evidence remains essential when malware is modified.
- Parent-child relationships, integrity levels, and user context help explain how a process started and what it could access.
- Persistent endpoint logging enables investigation after suspicious activity has occurred.
- Network and endpoint evidence are most effective when correlated into a single timeline.
- Outbound filtering should validate application and protocol behavior rather than relying only on port numbers.
- Persistence mechanisms often leave detectable filesystem, registry, and process artifacts.

## Skills Demonstrated

- Network traffic and packet analysis
- File extraction and hashing
- Windows endpoint monitoring
- Sysmon configuration and event analysis
- Process Explorer investigation
- Autoruns persistence analysis
- Process tree and integrity-level analysis
- TCP connection investigation
- Indicator-of-compromise identification
- Incident timeline reconstruction
- Security control assessment and remediation
- Professional cybersecurity documentation
