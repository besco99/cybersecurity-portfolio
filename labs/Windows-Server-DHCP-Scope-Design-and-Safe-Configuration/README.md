# Windows Server DHCP Scope Design and Safe Configuration

## Overview

This lab explored Dynamic Host Configuration Protocol (DHCP) administration on Windows Server. I installed the DHCP Server role, opened the management console, created an IPv4 scope, defined an address pool and exclusion range, selected a lease duration, configured common scope options, reviewed reservations and active leases, and intentionally left the scope inactive to avoid competing with an existing DHCP service.

The exercise emphasized safe service deployment and address-management planning. This portfolio entry does not include server credentials, hostnames, IP networks, address ranges, gateways, DNS servers, MAC addresses, lease values, scope names, commands, or proprietary lesson instructions.

## Lab Environment

- Windows Server 2022 virtual machine
- DHCP Server role
- DHCP management console
- Isolated virtual-lab design
- Existing external DHCP service on the surrounding network
- Inactive training scope

## Objectives

- Explain the role of DHCP in IPv4 address assignment.
- Install and review the Windows DHCP Server role.
- Design a valid IPv4 scope and address pool.
- Configure exclusions and lease duration.
- Understand router and DNS scope options.
- Compare exclusions with reservations.
- Review lease and reservation views.
- Prevent accidental activation of a competing DHCP server.
- Plan authorization, relay, redundancy, monitoring, and rollback.

## Authorization and Scope

All configuration occurred on a purpose-built Windows Server VM. The scope was not activated, and the lab did not lease addresses to real clients. No production DHCP server, organizational network, public resolver, user account, or device identifier was modified.

An unauthorized DHCP server can disrupt an entire broadcast domain by distributing incorrect addresses, gateways, or DNS settings. Production deployment requires approved network ownership, address-management coordination, change control, and a tested rollback plan.

## DHCP Fundamentals

DHCP automates delivery of network settings to clients. A typical IPv4 lease can include:

- Client IP address
- Subnet mask or prefix
- Default gateway
- DNS resolvers
- DNS domain name
- Lease duration
- Time, boot, voice, or vendor-specific options

DHCP reduces manual configuration and address conflicts, but an incorrect server can affect many systems quickly.

## DHCP Lease Process

The common IPv4 process is summarized as DORA:

1. **Discover:** The client broadcasts a request for configuration.
2. **Offer:** A DHCP server proposes an address and options.
3. **Request:** The client requests the selected offer.
4. **Acknowledge:** The server confirms the lease.

Clients later renew leases before expiration. Negative acknowledgments, declined addresses, release messages, and conflict checks also affect operation.

## Installing the DHCP Server Role

Windows Server does not install every infrastructure service by default. I added the DHCP Server role through Server Manager and confirmed that the DHCP management console became available.

Before installing a production role, administrators should verify:

- The server has a stable address.
- Its hostname and time are correct.
- Required updates are applied.
- Firewall and network paths are understood.
- The server is placed in the correct management and service zones.
- Backup and recovery procedures exist.
- Directory authorization requirements are known.

## Active Directory Authorization

In an Active Directory domain, a Windows DHCP server generally must be authorized before it can issue leases. Authorization helps prevent unapproved Windows DHCP servers from servicing domain networks.

Authorization is not a complete rogue-DHCP defense because non-Windows servers or devices may not participate in that mechanism. Switch protections such as DHCP snooping and network access control remain important.

## Scope Design

A DHCP scope defines the address space and configuration distributed on one logical subnet. A complete design should document:

- Network address and prefix length
- Usable client range
- Excluded addresses
- Reservations
- Default gateway
- DNS resolver order
- DNS suffix
- Lease duration
- VLAN and relay relationship
- High-availability partner
- Owner and change history

The scope range must belong to the same subnet as the intended client network.

## Address Pool

The lab defined a start and end address for dynamic allocation. The pool should be large enough for expected clients, growth, temporary devices, and lease overlap without consuming addresses reserved for infrastructure.

Capacity planning should account for:

- Peak concurrent clients
- Multiple devices per user
- Guest and transient devices
- Virtual desktops and containers
- Lease duration
- Standby or failover allocation
- Planned growth

## Exclusion Range

An exclusion removes addresses inside the scope range from dynamic allocation. Exclusions are useful for addresses that are managed manually or reserved for infrastructure but remain numerically inside the overall pool.

Examples may include:

- Routers and firewalls
- Servers with manual addresses
- Network appliances
- Printers managed outside DHCP
- Monitoring systems
- Temporary migration blocks

An exclusion prevents DHCP from leasing the address, but it does not assign that address to any device.

## DHCP Reservations

A reservation maps a specific client identifier, commonly a network-interface hardware address, to a consistent IP address. The client still uses DHCP but receives the reserved value.

Reservations are useful for:

- Printers and scanners
- Appliances
- Cameras
- Specialized workstations
- Systems that benefit from central address management

Reservations should be documented and protected because hardware addresses can change, be spoofed, or be entered incorrectly.

## Exclusions vs. Reservations

| Feature | Exclusion | Reservation |
| --- | --- | --- |
| Main purpose | Prevent dynamic assignment | Consistently assign an address to one client |
| Client configuration | Often manual or managed elsewhere | Client remains configured for DHCP |
| Identity binding | None | Associated with a client identifier |
| Address tracking | External documentation required | Visible in DHCP management |

Using both for the same address may be unnecessary depending on platform behavior and design. The chosen method should be consistent and documented.

## Lease Duration

Lease duration balances address reuse with renewal traffic and operational stability.

Shorter leases can suit:

- Guest networks
- High-turnover wireless environments
- Address-constrained pools
- Temporary labs

Longer leases can suit:

- Stable office devices
- Large pools with predictable membership
- Networks where renewal traffic or intermittent server reachability matters

The correct duration should be based on client behavior and capacity analysis rather than a universal value.

## Default Gateway Option

The router option tells clients where to send traffic destined outside their local subnet. An incorrect gateway can isolate clients or redirect traffic through an unauthorized device.

The configured gateway must:

- Be reachable in the client subnet
- Route the intended networks
- Enforce the appropriate security policy
- Be redundant if availability requirements demand it
- Match the VLAN and subnet design

## DNS Server Options

DHCP commonly distributes approved DNS resolver addresses and a domain suffix. Clients should receive resolvers that can answer required internal names and forward or resolve external names according to policy.

Important considerations include:

- Do not advertise a DNS server that is not configured and reachable.
- Use organization-approved resolvers.
- Define primary and secondary behavior intentionally.
- Protect internal namespace data.
- Monitor unauthorized resolver use.
- Coordinate DNS registration and scavenging settings.

The lab did not require internet connectivity or validate external resolvers.

## WINS and Legacy Options

The lesson reviewed but did not configure legacy name-resolution options. These settings should be omitted unless a documented application dependency requires them.

Unused legacy services increase complexity and attack surface. Migration plans should remove dependencies rather than preserving obsolete options indefinitely.

## Why the Scope Remained Inactive

Activating the lab scope on a network already served by another DHCP system could have caused clients to accept incorrect configuration. Potential consequences include:

- Address conflicts
- Loss of gateway or DNS access
- Traffic redirection
- Intermittent connectivity
- Security monitoring gaps
- Widespread support incidents

Leaving the scope inactive was the correct safety decision for a lab not fully isolated from the host network.

## Rogue DHCP Risk

A rogue or misconfigured DHCP server may intentionally or accidentally advertise malicious infrastructure. Defenses include:

- DHCP snooping on managed switches
- Trusted-uplink configuration
- Network access control
- Port security
- Segmentation
- Monitoring for unexpected offers
- Asset and configuration inventory
- Restricting who can install or activate DHCP services

DHCP snooping bindings can also support Dynamic ARP Inspection and IP Source Guard.

## DHCP Relay

DHCP discovery begins as a local broadcast and does not normally cross routers. A DHCP relay on the Layer 3 gateway can forward client requests to a centralized server.

Relay design should document:

- Client VLANs and gateway interfaces
- Approved DHCP server addresses
- Redundant relay targets
- Firewall rules
- Relay-agent information and trust
- Return routing
- Monitoring and failure behavior

## High Availability

Production DHCP should avoid a single point of failure. Windows Server supports failover designs that can synchronize leases and configuration between partners.

High-availability planning should consider:

- Load balance or standby behavior
- State synchronization
- Communication security
- Split-brain prevention
- Partner maintenance
- Recovery after extended outage
- Backup and restore interaction
- Testing from each client VLAN

## Monitoring and Logging

Useful DHCP telemetry includes:

- Lease allocation and renewal failures
- Scope utilization
- Address conflicts and declines
- Server authorization events
- Service start and stop events
- Failover state changes
- Configuration changes
- Unusual client identifiers
- Offers from unexpected servers
- Exhaustion trends

Logs should be centralized and correlated with switch, firewall, DNS, endpoint, and IP address management data.

## Validation Strategy

In a fully isolated test network, validation should confirm:

1. The server is authorized where required.
2. Only the approved server responds.
3. Clients receive addresses from the intended pool.
4. Excluded addresses are never leased.
5. Reservations receive the expected addresses.
6. Gateway and DNS options are correct.
7. Lease renewal and release work.
8. Relay functions from each client VLAN.
9. Logs and monitoring receive events.
10. Scope deactivation or rollback stops new leases safely.

## Change and Rollback Planning

Before activating a production scope:

- Back up the existing DHCP configuration.
- Confirm ownership of the subnet and VLAN.
- Review for overlapping scopes and static addresses.
- Validate gateway, DNS, and relay values.
- Coordinate with switching and routing teams.
- Establish a maintenance and communication plan.
- Define rollback and emergency deactivation steps.
- Monitor client behavior after activation.

## Troubleshooting Approach

When a client fails to obtain correct configuration:

- Verify the client interface and VLAN.
- Confirm the scope is active and has available addresses.
- Check exclusions, reservations, and policy filters.
- Validate relay configuration and firewall paths.
- Review server authorization and service status.
- Inspect DHCP logs and packet exchanges.
- Check for competing DHCP offers.
- Confirm gateway and DNS values.

Randomly activating additional servers can worsen the problem.

## Security Concepts Demonstrated

- DHCP server administration
- IPv4 scope design
- Address pools
- Exclusions and reservations
- Lease lifecycle
- DHCP options
- Active Directory authorization
- DHCP relay
- Rogue DHCP prevention
- DHCP snooping
- High availability
- Change and rollback planning

## Results

- Installed the Windows DHCP Server role.
- Opened and reviewed the DHCP management console.
- Created an IPv4 scope with a bounded address pool.
- Added an exclusion range.
- Selected a lease duration.
- Configured gateway and DNS scope options conceptually.
- Reviewed lease and reservation management.
- Intentionally left the scope inactive to prevent network disruption.
- Developed authorization, relay, high-availability, monitoring, and validation recommendations.

## Key Takeaways

- DHCP automates critical network configuration and can affect every client on a subnet.
- Scope ranges, exclusions, reservations, options, and lease duration serve different purposes.
- A reservation assigns a consistent address; an exclusion only prevents leasing.
- A DNS address should not be distributed unless the resolver is configured and reachable.
- Windows domain authorization helps but does not stop every rogue DHCP implementation.
- DHCP snooping and network segmentation strengthen protection at Layer 2.
- An unisolated lab scope should remain inactive to avoid competing with the legitimate server.
- Production activation requires backup, validation, monitoring, and rollback planning.

## Skills Demonstrated

- Windows Server role installation
- DHCP management console navigation
- IPv4 scope and pool design
- Exclusion and reservation planning
- Lease-duration analysis
- Gateway and DNS option configuration
- Active Directory DHCP authorization concepts
- Relay and high-availability planning
- Rogue DHCP detection and prevention
- DHCP monitoring and troubleshooting
- Safe change and rollback design
- Professional cybersecurity documentation
