# Pi-hole Network Security & DNS Monitoring Project

## Overview
Deployed and configured a Pi-hole DNS sinkhole on a Raspberry Pi to monitor, block, and analyze DNS traffic across a home network. This project focuses on DNS-level visibility, network threat detection, and blue team monitoring techniques commonly used in SOC environments.

## Objectives
- Monitor DNS traffic and client behavior across the network
- Block malicious, advertising, and tracking domains using threat intelligence
- Identify suspicious or anomalous DNS activity
- Practice log analysis and IOC-based investigations

## Environment
- Raspberry Pi (Raspberry Pi OS)
- Pi-hole
- Home LAN environment
- Client devices (Windows and Linux)

## Pi-hole Dashboard Overview
The dashboard provides high-level visibility into DNS activity, including total queries, blocked queries, active clients, and overall network behavior.

![Pi-hole Dashboard Overview](screenshots/dashboard-overview.png)

## DNS Query Log Analysis
The query log shows real-time and historical DNS requests from internal clients. This data is used to analyze domain behavior, identify unusual patterns, and investigate potentially malicious activity.

![DNS Query Log](screenshots/dns-query-log.png)

## Top Blocked Domains
This view highlights frequently blocked domains, many of which are associated with advertising, tracking, or known malicious infrastructure. These domains can serve as indicators for further investigation.

![Top Blocked Domains](screenshots/top-blocked-domains.png)

## Client Activity Monitoring
Client-level visibility allows analysis of DNS behavior per endpoint. This helps establish normal baselines and detect anomalies such as excessive queries or unusual domain access.

![Client Activity](screenshots/client-activity.png)

## Blocklist Configuration
Multiple threat intelligence blocklists were configured and tuned to reduce false positives while maximizing coverage against unwanted and malicious domains.

![Blocklists Configuration](screenshots/blocklists.png)

## Security Concepts Demonstrated
- DNS traffic monitoring and analysis
- Network-based threat detection
- Indicators of Compromise (IOC) identification
- Baseline versus anomalous behavior analysis
- Defense-in-depth principles
- Blue team monitoring fundamentals

## Tools Used
- Pi-hole
- Linux command line
- DNS
- Threat intelligence blocklists

## SOC Perspective
This project simulates DNS telemetry commonly analyzed in Security Operations Centers. The ability to monitor query logs, identify suspicious domains, and correlate client activity mirrors real-world SOC workflows for early threat detection and incident investigation.
