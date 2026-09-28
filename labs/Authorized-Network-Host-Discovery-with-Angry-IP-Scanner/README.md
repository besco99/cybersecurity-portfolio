# Authorized Network Host Discovery with Angry IP Scanner

## Overview

This lab explored network host discovery as part of asset inventory and security monitoring. In an isolated virtual environment, I used Angry IP Scanner from a Windows workstation to scan an approved private subnet and identify responsive systems, including Windows and Linux virtual machines.

The exercise demonstrated how active discovery can validate an expected inventory and reveal systems that require investigation. This portfolio entry omits exact IP addresses, hostnames, scan results, download links, and proprietary lesson instructions.

## Lab Environment

- Windows 10 virtual machine
- Linux virtual machine
- Angry IP Scanner
- Required application runtime
- Private virtual IPv4 subnet
- Command-line network configuration tools
- Isolated and authorized lab scope

## Objectives

- Identify the local IPv4 address and subnet boundary.
- Define an explicit, authorized discovery range.
- Perform basic active host discovery.
- Distinguish responsive from nonresponsive addresses.
- Review discovered hostnames and available metadata.
- Compare scan results with an expected asset inventory.
- Explain the limitations of a point-in-time discovery scan.
- Develop safe operational and investigative practices.

## Authorization and Scope

Active scanning sends traffic to many addresses and may trigger alerts, affect fragile systems, or violate policy when performed without permission. This exercise was limited to virtual machines in a purpose-built lab network.

Before scanning any organizational environment, an operator should have written authorization defining:

- Approved source system
- Target networks and exclusions
- Permitted discovery methods
- Allowed ports and protocols
- Scan rate and timing
- Monitoring and escalation contacts
- Data retention requirements
- Stop conditions

Ownership of a scanner does not grant permission to probe a network. Internet-wide or third-party scanning was outside the scope of this lab.

## Tool Preparation

The scanner was run from a Windows workstation after confirming its runtime dependency. In a production environment, security tools should be acquired only from an approved source and validated before execution.

Recommended software-handling controls include:

- Verify the publisher and digital signature.
- Compare the file hash with a trusted release value when available.
- Scan the package with endpoint security controls.
- Review the version and known vulnerabilities.
- Prefer managed installation and update mechanisms.
- Avoid disabling endpoint protection to run the tool.
- Remove temporary installers when retention is unnecessary.

Portable executables reduce installation overhead but are not inherently safer than installed applications.

## Establishing the Scan Range

I reviewed the network configuration on the Windows and Linux VMs and used the address and subnet mask to determine the lab's network boundary. The scanner was then limited to that private range.

Correct subnet calculation matters because an overly broad range can:

- Contact systems outside the authorization boundary
- Generate unnecessary traffic and alerts
- Increase scan duration
- Mix unrelated networks into the results
- Create privacy or compliance concerns

The source address, subnet, gateway, and interface should be confirmed before starting a scan.

## Active Host Discovery

Host-discovery tools test addresses and classify systems based on received responses. Depending on configuration and privileges, probes may use:

- ICMP echo requests
- TCP connection attempts
- UDP probes
- Address Resolution Protocol on a local segment
- Name-resolution queries

The lab used a basic discovery profile to identify responsive virtual hosts. Results represented systems visible to that scanner from that network location at that moment.

## Understanding Scan Results

For each address, the scanner may display information such as:

- Response status
- IP address
- Hostname
- Round-trip time
- Selected port results
- Optional fetched metadata

A responsive address indicates that a probe received a response. It does not by itself establish the device owner, operating system, security posture, or authorization status.

Likewise, a nonresponsive result does not prove that no host exists. A system may be powered off, asleep, filtered by a firewall, isolated by segmentation, rate-limited, or configured not to answer the selected probe.

## Hostname and Identity Limitations

Hostnames may come from DNS, local name-resolution protocols, or reverse lookups. They can be missing, stale, duplicated, or misleading. IP addresses may also change through DHCP.

Reliable asset identification should correlate multiple attributes:

- DHCP lease data
- Switch forwarding and authentication records
- MAC address and vendor information
- Endpoint management inventory
- Directory or identity records
- DNS records
- Network access control data
- Cloud and virtualization inventories

No single scanner field should be treated as definitive identity proof.

## Inventory Reconciliation

The central security use case was comparing observed systems with the expected asset inventory. A simple reconciliation process is:

1. Obtain the approved inventory for the subnet.
2. Normalize addresses, names, owners, and device identifiers.
3. Compare observed hosts with expected active systems.
4. Investigate unexpected devices and missing assets.
5. Record legitimate changes in the system of record.
6. Escalate unauthorized or unexplained systems.

An unexpected host is an indicator requiring validation, not automatic proof of compromise.

## Investigating an Unknown Host

When a scan reveals an unrecognized device, safe follow-up may include:

- Confirming the result from an approved second data source
- Reviewing DHCP and DNS history
- Locating the connected switch port or wireless access point
- Checking network access control and authentication logs
- Identifying the asset owner and business purpose
- Reviewing segmentation and policy compliance
- Capturing relevant timestamps and evidence
- Following incident-response procedures if risk is confirmed

Operators should avoid immediately logging in, exploiting services, or disconnecting equipment without the necessary authority and impact assessment.

## False Positives and False Negatives

Discovery accuracy is affected by network and endpoint behavior.

### Potential False Positives

- Reused DHCP addresses
- Stale DNS or ARP entries
- Proxy or gateway responses
- Duplicate addressing
- Misinterpreted name-resolution data

### Potential False Negatives

- Host firewalls blocking probes
- Devices in sleep or power-saving states
- Network segmentation
- Packet loss or rate limiting
- Unsupported IPv6 discovery
- Short scan timeouts
- Systems connected after the scan

Repeated observation and data correlation improve confidence.

## Operational Safety

Even lightweight discovery can affect sensitive environments. Safe practices include:

- Start with the smallest useful range.
- Use conservative timing and concurrency.
- Exclude fragile operational technology where required.
- Coordinate with the security operations center.
- Schedule scans within approved windows.
- Stop when unexpected instability occurs.
- Preserve scan settings with the results.
- Protect the resulting asset data.

Scan output can expose infrastructure details and should be handled according to organizational data-classification policy.

## Security Monitoring Use Cases

Host discovery can support:

- Asset inventory validation
- Rogue-device detection
- Vulnerability-management scoping
- Network segmentation reviews
- Incident-response triage
- Change verification
- Lab and test-environment administration
- Identification of unmanaged systems

Scheduled discovery is most effective when integrated with DHCP, DNS, endpoint, switching, wireless, cloud, and identity telemetry.

## IPv4 and IPv6 Considerations

Scanning an IPv4 subnet does not provide a complete view of an IPv6-enabled environment. IPv6 address spaces are too large for simple exhaustive scanning, and systems may use temporary or privacy addresses.

IPv6 visibility should incorporate:

- Neighbor Discovery data
- Router and switch telemetry
- DHCPv6 logs where used
- DNS AAAA records
- Endpoint management data
- Network access control
- Flow and firewall logs

Asset-discovery programs should explicitly account for both protocol families.

## Monitoring and Detection

Defenders can detect scanning through:

- Repeated probes across sequential addresses
- One source contacting many destinations
- Unusual ICMP volume
- Short-lived connections across multiple ports
- Endpoint firewall events
- IDS or IPS signatures
- Network flow analytics

Authorized scan schedules and source addresses should be documented so defenders can distinguish expected activity from suspicious reconnaissance without creating blind spots.

## Validation Strategy

I would validate a discovery exercise by confirming:

1. The source and target range match the authorization.
2. The calculated subnet boundaries are correct.
3. Known online lab hosts appear in the results.
4. Known offline systems are not incorrectly treated as active.
5. Hostnames are corroborated before use as identity evidence.
6. Unexpected systems receive documented follow-up.
7. Scan timing and settings are retained with the report.
8. Results are stored securely and removed when no longer needed.

## Security Concepts Demonstrated

- Active network reconnaissance
- Asset discovery and inventory
- IPv4 subnet scoping
- Endpoint and network visibility
- Rogue-device identification
- Evidence correlation
- False-positive and false-negative analysis
- Least-privilege tooling
- Change and incident escalation
- Security data protection

## Results

- Confirmed the network configuration of Windows and Linux lab systems.
- Calculated and entered an approved private scan range.
- Performed host discovery from the Windows VM.
- Identified responsive systems within the virtual network.
- Reviewed available address and hostname metadata.
- Compared observed systems with the small expected lab inventory.
- Developed a defensible process for validating unexpected hosts.
- Documented authorization, safety, monitoring, and IPv6 considerations.

## Key Takeaways

- Accurate asset inventory is foundational to security operations.
- Active scans must stay within an explicitly authorized scope.
- A response indicates visibility, not verified device identity or trust.
- A missing response does not prove that an address is unused.
- Scanner results become more reliable when correlated with DHCP, DNS, switching, identity, and endpoint data.
- Unknown devices should be investigated methodically before containment action.
- Scan results are sensitive infrastructure data and require protection.
- IPv4-only discovery is incomplete in a dual-stack environment.

## Skills Demonstrated

- Angry IP Scanner operation
- Windows and Linux network configuration review
- IPv4 subnet and scan-range selection
- Active host discovery
- Asset inventory reconciliation
- Network visibility analysis
- Unknown-device investigation planning
- Scan authorization and safety controls
- False-positive and false-negative evaluation
- Security monitoring integration
- Professional cybersecurity documentation
