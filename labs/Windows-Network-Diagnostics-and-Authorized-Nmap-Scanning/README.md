# Windows Network Diagnostics and Authorized Nmap Scanning

## Overview

This lab combined native Windows network-diagnostic commands with an authorized Nmap scan in a controlled environment. I examined interface configuration, DHCP state, DNS caching and resolution, ICMP reachability, ARP mappings, active connections, listening ports, protocol statistics, routing information, host availability, and exposed network services.

The goal was to build a repeatable troubleshooting workflow rather than treat any single command as conclusive. This portfolio entry removes exact addresses, hostnames, public scan targets, timestamps, and copied instructional wording.

## Lab Environment

- Windows workstation
- Administrative Command Prompt
- Authorized virtual network
- Nmap and Zenmap graphical interface
- Npcap packet-capture driver
- Native Windows networking utilities
- Approved test hosts

## Objectives

- Inspect Windows interface and DHCP configuration.
- Release and renew a DHCP lease safely.
- Display and clear the local DNS resolver cache.
- Test reachability and packet behavior with `ping`.
- Query DNS records and resolvers with `nslookup`.
- Review IPv4-to-MAC mappings in the ARP cache.
- Inspect listening ports and active connections with `netstat`.
- Review interface counters and the local routing table.
- Perform an authorized Nmap service-discovery scan.
- Correlate findings without overstating what they prove.

## Authorization and Scope

Port scanning and active discovery generate network traffic and can trigger alerts or affect fragile services. Nmap testing was limited to systems specifically authorized for the exercise. Public websites and third-party infrastructure should not be scanned without explicit permission.

A production scan plan should identify:

- Approved targets and exclusions
- Source addresses
- Permitted scan types and intensity
- Maintenance window
- Rate and concurrency limits
- Operational and security contacts
- Stop conditions
- Data handling and retention requirements

Native diagnostic commands are generally low risk, but actions such as releasing a lease, clearing caches, deleting ARP entries, or changing routes can interrupt connectivity.

## Diagnostic Workflow

The tools in this lab answer different questions:

| Question | Primary tool |
| --- | --- |
| What network settings does this host have? | `ipconfig` |
| Can the host reach a destination? | `ping` |
| Which DNS server and records are involved? | `nslookup` |
| Which local IPv4 neighbors have been resolved? | `arp` |
| Which ports and connections exist locally? | `netstat` |
| Which remote services appear reachable? | Nmap |

A strong investigation starts with local configuration, tests one layer at a time, records evidence, and compares multiple sources.

## Reviewing IP Configuration

The basic `ipconfig` view summarizes addressing for each Windows interface. The detailed view adds information such as:

- Adapter description
- Physical address
- DHCP status
- IPv4 and IPv6 addresses
- Subnet mask or prefix
- Default gateway
- DHCP server
- DNS servers
- Lease acquisition and expiration
- DNS suffixes

Virtualization, VPN, container, and tunneling software may create additional adapters. Their presence is not automatically suspicious; each interface should be interpreted in context.

### Representative Commands

```powershell
ipconfig
ipconfig /all
```

## DHCP Release and Renewal

Releasing a DHCP lease removes dynamically assigned configuration from an interface. Renewing starts a DHCP exchange to obtain configuration again.

```powershell
ipconfig /release
ipconfig /renew
```

These actions may require elevation and will temporarily interrupt connectivity. They should not be used casually on remote systems, production servers, statically addressed interfaces, or during an active remote session.

A renewal may return the same address because the server can offer the prior lease again. That does not indicate failure.

## DNS Resolver Cache

Windows caches DNS answers to reduce latency and query volume. The cache includes positive and negative responses with time-to-live values.

```powershell
ipconfig /displaydns
ipconfig /flushdns
```

Clearing the cache can help after an approved DNS change or when troubleshooting stale local data. It does not repair an incorrect authoritative record, broken delegation, unreachable resolver, or malicious upstream response.

Before clearing evidence during a security investigation, capture the cache contents and relevant timestamps.

## Testing Reachability with Ping

`ping` sends ICMP Echo Requests and reports responses, latency, and packet loss. It can help test name resolution, IP reachability, path stability, and protocol-family selection.

```powershell
ping <approved-host>
ping -n <count> <approved-host>
ping -t <approved-host>
ping -4 <approved-host>
ping -6 <approved-host>
```

Continuous mode is useful while observing a controlled network change and can be stopped with `Ctrl+C`. It should be used responsibly to avoid unnecessary traffic.

### Interpretation Limits

- A successful response shows that ICMP reached a responding endpoint.
- A failed response does not prove the host is offline.
- Firewalls may block ICMP while allowing applications.
- Name-based ping tests DNS and reachability together.
- An IP-based test helps isolate DNS from network-path issues.
- Low latency does not prove application health.

## Querying DNS with Nslookup

`nslookup` can show the configured resolver and query records for a name or address.

```powershell
nslookup <approved-name>
nslookup -type=A <approved-name>
nslookup -type=AAAA <approved-name>
nslookup -type=MX <approved-domain>
nslookup <approved-name> <specific-dns-server>
```

Useful troubleshooting questions include:

- Which resolver answered?
- Was the answer authoritative or cached?
- Does the expected record type exist?
- Do multiple resolvers return consistent results?
- Is the returned time to live reasonable?
- Does reverse resolution agree with the expected hostname?

A DNS answer confirms the data returned by that resolver, not the legitimacy or health of the destination service.

## Reviewing the ARP Cache

Address Resolution Protocol maps IPv4 addresses to link-layer addresses on the local network. Windows stores recently learned mappings in a neighbor cache.

```powershell
arp -a
```

Dynamic entries are learned through network activity; static entries are manually or system-defined mappings. Broadcast and multicast entries can appear and should not be treated as ordinary endpoint records.

### ARP Security Considerations

ARP has no built-in authentication. An attacker on the same Layer 2 segment may attempt ARP spoofing to redirect or intercept traffic. Potential indicators include:

- A gateway address unexpectedly mapping to a new MAC address
- Multiple IP addresses mapping to one unrecognized MAC
- Rapid changes in important neighbor entries
- Duplicate-address warnings
- Connectivity changes correlated with ARP updates

An ARP-cache entry alone is not proof of spoofing. Corroboration may include switch tables, DHCP snooping bindings, packet captures, gateway information, and endpoint logs.

Static ARP entries do not scale as a general enterprise defense. Controls such as Dynamic ARP Inspection, DHCP snooping, segmentation, and authenticated network access are usually more manageable.

## Inspecting Connections with Netstat

`netstat` displays local listeners, established connections, counters, and routing information depending on its options.

```powershell
netstat -ano
netstat -e
netstat -r
```

The combined listener and connection view can show:

- Local address and port
- Remote address and port
- Transport protocol
- Connection state
- Process identifier

The process identifier can be correlated with Task Manager, Process Explorer, or approved endpoint tooling. Elevation may be needed for some process details.

### Ethernet Statistics

Interface counters show bytes and packet totals. Unexpectedly rapid growth can justify further investigation, but background services, updates, cloud synchronization, monitoring, and normal application traffic also increase counters.

Traffic volume alone does not establish malware. A defensible conclusion requires process attribution, destination context, timing, packet or flow evidence, and endpoint telemetry.

### Routing Table

The routing view identifies how Windows selects an interface and next hop. Important fields include:

- Destination prefix
- Netmask or prefix length
- Gateway
- Interface
- Metric

Unexpected routes can cause loss of connectivity or traffic redirection. Virtual adapters and VPN clients often add legitimate routes, so changes should be validated against the system's intended design.

## Installing Nmap Safely

Nmap and its packet-capture dependency should be obtained from the official project or an approved software repository. Before installation:

- Verify the download source.
- Check the digital signature or published hash.
- Review the package version.
- Retain only necessary components.
- Follow endpoint-management policy.
- Do not disable security controls simply to complete installation.

Npcap enables packet capture and certain low-level network operations. Promiscuous mode allows an interface to receive frames not addressed to it when the network makes those frames visible, but it is not required for every Nmap scan and does not bypass switched-network segmentation.

## Authorized Nmap Scanning

Nmap can perform host discovery, port-state analysis, service detection, operating-system estimation, and script-based checks. The lab used Zenmap to run a basic approved scan and interpret the output.

Key result fields included:

- Host status
- Resolved name
- Port number and protocol
- Port state
- Probable service
- Service version estimate
- Operating-system estimate
- Network distance estimate

## Understanding Port States

Nmap commonly reports states such as:

| State | Meaning |
| --- | --- |
| Open | An application accepted or responded on the port |
| Closed | The host responded, but no application was listening |
| Filtered | A firewall or network condition prevented classification |
| Unfiltered | The port was reachable, but open or closed was not established |
| Open or filtered | Responses did not distinguish between the two states |
| Closed or filtered | Responses did not distinguish between the two states |

An open port identifies an exposed service endpoint, not automatically a vulnerability. Risk depends on service necessity, version, configuration, authentication, network exposure, and compensating controls.

## Service and Operating-System Detection

Service detection sends probes and compares responses with known signatures. Operating-system detection uses characteristics of the target's network stack. Results are estimates and may be affected by:

- Firewalls and proxies
- Load balancers
- Network address translation
- Limited probe responses
- Customized service banners
- Packet loss
- Virtualization
- Middleboxes altering traffic

Findings should be verified with configuration management, authenticated inventory, or system-owner confirmation.

## Port-to-Process Correlation

Local and remote observations complement one another:

1. Nmap identifies a remotely reachable port.
2. `netstat` identifies the local listener and process ID.
3. Process tools identify the executable and owner.
4. Service configuration explains why it is listening.
5. Firewall policy confirms intended exposure.
6. Vulnerability data helps assess version and configuration risk.

This correlation distinguishes legitimate services from unexpected exposure more reliably than a scan alone.

## Troubleshooting Methodology

A structured workflow for connectivity problems is:

1. Confirm the physical or virtual interface is connected.
2. Review local address, prefix, gateway, and DNS settings.
3. Test the local TCP/IP stack.
4. Test the local gateway by approved address.
5. Test a remote address to isolate routing from DNS.
6. Test a hostname to evaluate DNS.
7. Query the configured resolver directly.
8. Inspect ARP mappings and the routing table.
9. Review local listeners and connections.
10. Use an authorized remote scan if required.
11. Correlate results with firewall, DHCP, DNS, and endpoint logs.

Only one controlled variable should be changed at a time, with results recorded before and after.

## Monitoring and Detection

Defenders may detect active scanning through sequential probes, high destination counts, unusual ICMP volume, repeated connection attempts, or Nmap-specific patterns. Authorized scanner addresses and windows should be documented without broadly suppressing their activity.

Useful endpoint and network evidence includes:

- Windows Firewall events
- DNS client events
- DHCP client logs
- Process creation records
- IDS or IPS alerts
- Network flow data
- Switch and router logs
- Endpoint detection telemetry

## Validation Strategy

I would validate the lab by confirming:

1. Interface data matches the authorized network design.
2. DHCP release and renewal behavior is understood before use.
3. DNS cache changes are recorded and reproducible.
4. Name-based and address-based reachability tests are compared.
5. DNS answers are checked against the intended resolver.
6. ARP mappings are corroborated with network infrastructure.
7. Listening ports are mapped to known processes.
8. Routes match the intended path.
9. Nmap targets remain inside the approved range.
10. Remote scan results agree with local service configuration.

## Security Concepts Demonstrated

- Layered network troubleshooting
- IPv4 and IPv6 configuration analysis
- DHCP lease management
- DNS cache and resolver analysis
- ICMP reachability testing
- ARP inspection and spoofing awareness
- Connection and listener analysis
- Routing-table interpretation
- Authorized port and service discovery
- Evidence correlation and validation

## Results

- Reviewed Windows interface, DHCP, gateway, and DNS configuration.
- Practiced controlled DHCP lease release and renewal.
- Displayed and cleared the local DNS resolver cache.
- Tested finite, continuous, IPv4, and IPv6 ping behavior.
- Queried an approved DNS resolver and interpreted returned records.
- Reviewed dynamic and static ARP entries.
- Examined local listeners, connections, traffic counters, and routes.
- Installed Nmap with its approved packet-capture dependency.
- Performed an authorized scan and interpreted port and service results.
- Correlated remote exposure with local diagnostic evidence.

## Key Takeaways

- No single diagnostic command provides complete proof of cause or compromise.
- Releasing a DHCP lease can disrupt connectivity and requires planning.
- Clearing a DNS cache treats local state, not upstream DNS faults.
- Ping failure may reflect filtering rather than host failure.
- ARP and DNS results require corroboration before being trusted as identity evidence.
- Large traffic counters alone do not prove malware activity.
- Open ports should be evaluated for necessity, ownership, configuration, and exposure.
- Nmap results are estimates that should be confirmed with authenticated sources.
- Scanning must stay within a clearly documented authorization boundary.

## Skills Demonstrated

- Windows command-line network diagnostics
- `ipconfig`, `ping`, `nslookup`, `arp`, and `netstat`
- DHCP and DNS troubleshooting
- ARP cache and routing-table analysis
- Port-to-process correlation
- Nmap and Zenmap operation
- Port-state and service interpretation
- Scan scoping and authorization
- Evidence-based security analysis
- Professional cybersecurity documentation
