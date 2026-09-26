# Scapy Network Discovery and SYN Flood Traffic Analysis

## Overview

This authorized lab demonstrated how Python and Scapy can create and analyze custom network traffic in an isolated virtual environment. The first exercise used Address Resolution Protocol (ARP) requests to identify active devices on a local broadcast domain. The second generated a tightly controlled burst of TCP connection-initiation traffic so its denial-of-service characteristics could be observed with a packet-capture utility on a separate Linux host.

The portfolio entry emphasizes defensive traffic analysis and safe test design. It does not include packet-crafting source code, target addresses, subnet details, interface names, service ports, packet counts, commands, or reusable flood instructions.

## Lab Environment

- Isolated Kali Linux assessment workstation
- Separate Linux observation host
- Private virtual network
- Python 3
- Scapy packet manipulation library
- Command-line packet capture and filtering
- Controlled local services

## Objectives

- Explain how ARP supports IPv4 communication on a local network segment.
- Use packet-level automation to identify responding lab hosts.
- Correlate network-layer addresses with link-layer addresses.
- Explain the TCP three-way handshake and half-open connections.
- Observe the traffic pattern created by a controlled SYN burst.
- Identify defensive indicators of scanning and denial-of-service activity.
- Recommend rate, network, host, and monitoring controls.

## Authorization and Scope

All packets were generated within an isolated virtual lab using approved source and destination systems. No public networks, production services, third-party devices, or real customer data were involved.

Denial-of-service testing can degrade systems and networks even at unexpectedly low rates. It is commonly excluded from penetration tests unless separately authorized. Any such validation requires explicit approval, precise rate and duration limits, health monitoring, a tested stop mechanism, recovery procedures, and coordination with system owners.

## Scapy Fundamentals

Scapy is a Python library for creating, sending, receiving, decoding, and analyzing network packets. It allows an analyst to work directly with protocol fields and combine layers into custom traffic.

Useful defensive and testing applications include:

- Protocol education and validation
- Asset discovery in controlled networks
- Detection-rule testing
- Reproduction of malformed or unusual traffic
- Packet-capture analysis
- Network control validation
- Creation of limited proof-of-concept traffic in a lab

Raw packet access requires elevated operating-system capabilities. Scripts should be reviewed carefully because errors can affect more systems than intended.

## ARP Discovery Exercise

ARP maps IPv4 addresses to Media Access Control (MAC) addresses within a local broadcast domain. The discovery script created an ARP request covering the approved lab range, placed it in a broadcast Ethernet frame, transmitted it at Layer 2, and collected replies.

Each response associated a responding IPv4 address with a MAC address. The script then displayed a concise inventory of observed lab devices.

The executable script, broadcast address, target range, and library calls are intentionally omitted.

## ARP Discovery Characteristics

ARP scanning is effective on a local segment because hosts generally need to answer ARP before normal IPv4 communication can occur. It does not depend on ICMP echo responses and does not traverse routers as an ordinary broadcast.

Limitations include:

- Visibility is restricted to the local Layer 2 domain.
- Sleeping, isolated, or protected devices may not respond.
- Duplicate addresses and spoofed replies can produce misleading results.
- Virtual networking can alter observed MAC information.
- A response confirms network presence, not device identity or authorization.

Results should be correlated with switch tables, DHCP records, asset inventory, endpoint management, and network access-control data.

## ARP Security Risks

Unusual ARP behavior may support reconnaissance, spoofing, interception, or disruption. Defensive controls can include:

- Segmentation of user, server, management, and guest networks
- Dynamic ARP inspection where supported
- DHCP snooping and trusted port configuration
- Port security and network access control
- Static mappings only for limited high-value use cases
- Alerts for duplicate addresses and MAC movement
- Asset inventory reconciliation
- Monitoring for unusually broad or rapid ARP requests

## TCP SYN Traffic Exercise

TCP normally begins a session through a three-step handshake. A client sends an initial synchronization request, the server acknowledges and responds, and the client completes the exchange. Until completion, the server may retain temporary state for the pending connection.

The lab script generated a bounded series of initial TCP connection requests to an approved service. A packet-capture filter on the observation host displayed the arriving requests in real time, confirming that the custom packets reached the destination.

The test demonstrated traffic appearance and monitoring behavior; it was not intended to exhaust the host or validate production-scale denial of service.

## SYN Flood Risk

A SYN flood attempts to consume connection-tracking resources or network capacity by sending more initial connection requests than a service can handle. If requests remain incomplete, a server may accumulate half-open sessions until queues, memory, CPU, or upstream capacity are constrained.

Potential impact includes:

- Delayed or failed legitimate connections
- Increased CPU and memory use
- Exhausted connection queues or state tables
- Load balancer, firewall, or proxy saturation
- Alert fatigue and logging pressure
- Cascading effects on dependent services

The practical impact depends on traffic volume, source distribution, server configuration, network controls, and service architecture.

## Packet Capture and Validation

The observation system used a narrow capture filter to focus on initial TCP connection requests destined for the approved lab service. A targeted filter reduced unrelated traffic and made the pattern easier to confirm.

Useful evidence for defensive analysis includes:

- Packet timestamps and arrival rate
- Source and destination distribution
- TCP flag combinations
- Completed versus incomplete handshakes
- Retransmissions
- Server response behavior
- Queue and connection-state metrics
- CPU, memory, and network utilization
- Firewall, load balancer, and application logs

Packet counts alone do not establish impact; service health must also be measured.

## Detection Opportunities

### ARP Discovery Indicators

- One host requesting mappings for many addresses
- Sequential sweeps across a local address range
- ARP traffic from devices that rarely perform discovery
- Sudden increases in unresolved or duplicate mappings
- A single MAC claiming multiple unexpected addresses
- MAC-address movement between switch ports

### SYN Flood Indicators

- Sudden spikes in initial TCP connection requests
- A growing ratio of incomplete to completed handshakes
- Increasing half-open connection queues
- Large numbers of sources or rapidly changing source values
- Repeated requests to one service without application data
- Firewall or load-balancer state-table pressure
- Increased retransmissions, latency, or connection failures
- Endpoint resource exhaustion correlated with network volume

Network telemetry, packet capture, flow records, infrastructure metrics, and host logs should be correlated before classifying an event.

## Defensive Mitigations

### Network and Edge Controls

- Apply rate limits and connection limits appropriate to the service.
- Use upstream denial-of-service protection for internet-facing applications.
- Filter spoofed traffic at network boundaries.
- Segment high-value services and restrict unnecessary reachability.
- Tune firewall, proxy, and load-balancer protections.
- Use distributed and elastic architectures where business requirements justify them.

### Host and Service Controls

- Enable operating-system protections for incomplete TCP handshakes.
- Tune queue sizes and timeouts based on tested workloads.
- Keep network stacks, services, and appliances patched.
- Remove unnecessary listening services.
- Monitor resource use, connection states, and error rates.
- Maintain graceful degradation and recovery procedures.

Protective tuning should be load-tested because overly aggressive limits can block legitimate traffic.

## Safe Test Design

A controlled availability test should define:

- Exact source and destination systems
- Maximum packet rate and total duration
- Permitted protocol and service
- Baseline service-health metrics
- Automatic and manual stop conditions
- Monitoring owners and communication channels
- Rollback and recovery steps
- Evidence retention and deletion requirements

Start at the lowest practical rate and increase only when authorized. Production denial-of-service testing should never be inferred from general penetration-testing approval.

## Secure Script Design

- Require an explicit allowlist of destinations.
- Reject public, broadcast, multicast, and out-of-scope addresses unless specifically approved.
- Set conservative hard limits for rate, count, and duration.
- Default to a non-sending validation mode.
- Require clear operator confirmation before transmitting.
- Handle interruption and cleanup reliably.
- Log scope and timing without retaining unnecessary sensitive data.
- Drop elevated privileges when raw packet access is no longer required.
- Prevent untrusted input from controlling packet fields or interfaces.
- Include a visible emergency stop mechanism.

## Validation and Analysis Limits

- ARP results indicate local presence but do not establish device ownership.
- Packet arrival does not prove application-layer impact.
- A lab-scale SYN burst may be detectable without degrading the target.
- Virtual network behavior may differ from physical networks and cloud platforms.
- Capture loss can distort measured packet volume.
- Host protections may handle incomplete connections differently across operating systems.

These limitations should be stated clearly in any assessment report.

## Security Concepts Demonstrated

- Packet crafting and analysis
- Scapy and Python networking
- Ethernet broadcast behavior
- ARP-based asset discovery
- IPv4-to-MAC address mapping
- TCP three-way handshake
- Half-open connection analysis
- SYN flood detection
- Packet capture and filtering
- Network segmentation
- Availability protection
- Safe security-test design

## Results

- Reviewed the structure and purpose of two Scapy automation scripts.
- Identified active devices within the authorized local lab segment.
- Correlated responding IPv4 and MAC addresses.
- Generated a bounded series of TCP connection-initiation packets.
- Confirmed packet arrival through a filtered capture on the observation host.
- Analyzed defensive indicators of ARP discovery and SYN-flood behavior.
- Developed network, host, monitoring, and test-safety recommendations.

## Key Takeaways

- Scapy provides granular packet control and therefore requires strict safeguards.
- ARP discovery is effective only within the local broadcast domain.
- Asset-discovery results should be correlated with authoritative inventory sources.
- A SYN flood abuses connection establishment and may consume state before sessions complete.
- Detection requires both network patterns and service-health context.
- Denial-of-service testing needs separate authorization, hard limits, and immediate stop controls.
- Understanding attack traffic helps defenders design better telemetry and mitigations without publishing reproduction details.

## Skills Demonstrated

- Python and Scapy concepts
- ARP and Ethernet analysis
- Local network asset discovery
- TCP handshake analysis
- Packet capture and filter interpretation
- Denial-of-service traffic recognition
- Network anomaly detection strategy
- Availability-control recommendations
- Safe test planning and scoping
- Risk and limitation documentation
- Ethical security-testing practices
- Professional cybersecurity documentation
