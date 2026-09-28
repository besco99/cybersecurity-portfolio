# VLAN Network Segmentation Design and Configuration

## Overview

This lab explored network segmentation through Virtual Local Area Networks (VLANs) using a Cisco small-business device simulator. I reviewed the default LAN configuration, navigated to the VLAN administration interface, defined additional logical segments, associated each segment with an IPv4 network and gateway interface, and analyzed how VLANs create separate Layer 2 broadcast domains.

Because the environment was a simulator, the configuration was not persistent and did not forward live production traffic. This portfolio entry does not include device models, firmware versions, VLAN identifiers, IP subnets, interface assignments, URLs, credentials, commands, or proprietary lesson instructions.

## Lab Environment

- Cisco small-business network-device simulator
- Browser-based administration interface
- Integrated switching, routing, firewall, and VPN concepts
- Simulated IPv4 LAN interfaces
- Non-production learning environment

## Objectives

- Explain the purpose of network segmentation.
- Distinguish VLANs from IP subnets.
- Create multiple logical Layer 2 segments.
- Associate VLAN interfaces with distinct IPv4 networks.
- Understand access ports, trunk ports, and 802.1Q tagging.
- Explain inter-VLAN routing and policy enforcement.
- Identify management-VLAN and default-VLAN risks.
- Develop validation, documentation, and rollback procedures.

## Authorization and Scope

The activity used a vendor-provided simulator and did not change a physical switch, router, production VLAN, wireless network, or organizational firewall. No real addressing plan, device credential, or user data was involved.

Production VLAN changes can disconnect users, expose management services, create loops, or bypass security controls. They require approved change management, configuration backups, console or out-of-band access, maintenance planning, validation, and rollback procedures.

## Why Segment a Network

A flat network places many systems in one trust zone and broadcast domain. Segmentation can:

- Limit Layer 2 broadcast scope
- Separate departments and business functions
- Isolate guests, servers, management systems, and security tools
- Reduce lateral movement
- Apply different firewall and monitoring policies
- Contain incidents
- Improve asset inventory and traffic analysis
- Support compliance boundaries

Segmentation is effective only when communication between zones is controlled. Creating VLANs without access policy may improve organization but provide limited security.

## VLAN Fundamentals

A VLAN is a logical Layer 2 broadcast domain implemented by managed switching infrastructure. Devices in different VLANs do not exchange ordinary Layer 2 traffic directly, even when connected to the same physical switch.

Each VLAN is identified by a tag value within the supported range. The identifier organizes switching behavior; it is not itself an authentication or encryption mechanism.

## VLAN Membership

VLAN membership is determined by the switching configuration, commonly through:

- Assignment of an access port to one VLAN
- 802.1Q tags on a trunk link
- Dynamic assignment through authenticated network access or policy systems

The switch does not normally place a host into a VLAN merely by observing its IPv4 address. If a user changes the endpoint's IP address, the device remains in the VLAN assigned to its switchport or authenticated session, although it may lose connectivity or create an address conflict.

## VLANs and IP Subnets

A common design maps one IPv4 subnet to one VLAN. This simplifies addressing, routing, DHCP, and policy enforcement.

The technologies operate at different layers:

- A VLAN separates Layer 2 broadcast domains.
- An IP subnet defines Layer 3 addressing and local-delivery boundaries.
- A router or Layer 3 switch moves traffic between subnets and VLANs.

Placing multiple unrelated subnets in one VLAN or one subnet across multiple isolated VLANs can create confusing and fragile behavior unless a specialized design requires it.

## Simulated Configuration Workflow

The simulator demonstrated a high-level workflow:

1. Review the existing LAN and default VLAN configuration.
2. Define a new VLAN with a descriptive purpose.
3. Configure the corresponding Layer 3 interface and prefix.
4. Repeat for additional security or business zones.
5. Assign access ports or trunks as required.
6. Configure DHCP, routing, and access policy.
7. Validate same-VLAN and cross-VLAN communication.
8. Save the running configuration on a real device.

The simulator allowed interface exploration but did not preserve or enforce the configuration on live traffic.

## Access Ports

An access port normally carries traffic for one VLAN and presents untagged Ethernet frames to the attached endpoint. Common access-port use cases include:

- User workstations
- Printers
- Cameras
- Ordinary servers
- Dedicated management devices

The configured access VLAN should match the connected device's business and security role. Unused ports should be disabled or placed in an isolated unused VLAN according to organizational standards.

## Trunk Ports

A trunk carries multiple VLANs across one physical or logical link using 802.1Q tags. Trunks commonly connect:

- Switches to other switches
- Switches to routers or firewalls
- Switches to virtualization hosts
- Switches to wireless access points
- Switches to servers with multiple tagged networks

Trunks should allow only required VLANs. Permitting every VLAN unnecessarily increases exposure and complicates troubleshooting.

## Native VLAN Considerations

On an 802.1Q trunk, the native VLAN may carry untagged frames depending on platform configuration. Mismatched native VLANs can cause traffic leakage, loops, or confusing behavior.

Defensive practices include:

- Use an intentionally selected, otherwise unused native VLAN when the platform design supports it.
- Configure both ends of a trunk consistently.
- Avoid using the native VLAN for user or management traffic.
- Monitor mismatch alerts and unexpected untagged frames.
- Tag native traffic where supported and appropriate.

## Default VLAN Risk

Many devices begin with all access ports in a default VLAN. Leaving users, management interfaces, and infrastructure services together in that VLAN weakens segmentation.

Recommended practices include:

- Move user and server ports to purpose-specific VLANs.
- Use a dedicated management segment.
- Avoid assigning ordinary endpoints to the default VLAN.
- Restrict or disable unused ports.
- Review platform-specific control protocols before changing defaults.
- Document necessary exceptions.

## Management VLAN

Network-device management should be separated from general user traffic. A management VLAN can carry administrative HTTPS, SSH, SNMP, syslog, authentication, and orchestration traffic.

Security controls should include:

- Access only from approved administrative systems
- Strong authentication and role-based authorization
- Encrypted management protocols
- Multifactor authentication where supported
- Firewall or access-control restrictions
- Centralized logging and time synchronization
- No direct guest or public access
- Out-of-band recovery access for failed changes

A management VLAN alone is not sufficient without routing and identity controls.

## Inter-VLAN Routing

Hosts in different VLANs require a Layer 3 device to communicate. Inter-VLAN routing may be provided by:

- A router with tagged subinterfaces
- A multilayer switch with switched virtual interfaces
- A firewall with interfaces or subinterfaces for each zone
- A virtual router or cloud network appliance

The default gateway for each subnet is typically the corresponding routed VLAN interface.

## Security Policy Between VLANs

Inter-VLAN traffic should be permitted according to explicit business need. Example policy principles include:

- Users may reach approved application services but not server management interfaces.
- Guest devices may reach the internet but not internal networks.
- Internet of Things devices may reach only required controllers and services.
- Administrative workstations may reach management interfaces.
- Backup systems may reach protected workloads on limited protocols.
- Sensitive departments may communicate only through approved applications.

A default-deny policy with documented exceptions provides stronger control than unrestricted routing.

## DHCP and Address Management

Each client VLAN commonly requires its own DHCP scope. If the DHCP server resides in another VLAN, the Layer 3 gateway may relay requests.

Address planning should document:

- VLAN purpose and owner
- Subnet and prefix
- Default gateway
- DHCP range and exclusions
- DNS and time services
- Static assignments and reservations
- Route and firewall dependencies
- IPv6 prefix and router-advertisement policy

Duplicate or overlapping networks can prevent correct routing and policy enforcement.

## IPv6 Segmentation

IPv6 requires the same segmentation attention as IPv4. A network is not isolated merely because only IPv4 policy was configured.

Review:

- IPv6 prefixes per VLAN
- Router advertisements
- DHCPv6 where used
- IPv6 firewall and access-control policy
- Link-local communication
- Neighbor Discovery protections
- Tunneling mechanisms
- Monitoring and asset inventory

Unmanaged IPv6 can create an unintended path around IPv4-only controls.

## Layer 2 Security Controls

VLAN design can be strengthened with:

- Port security
- 802.1X network access control
- DHCP snooping
- Dynamic ARP inspection
- IP source guard
- Spanning Tree protections
- Broadcast and storm control
- MAC-address monitoring
- Private VLANs where appropriate

These controls address threats that VLAN separation alone does not prevent.

## VLAN Hopping Considerations

Misconfigured trunks, dynamic trunk negotiation, or native-VLAN handling may create opportunities for traffic to reach unintended VLANs.

Defensive practices include:

- Configure access ports explicitly as access ports.
- Disable dynamic trunk negotiation where unnecessary.
- Restrict allowed VLANs on trunks.
- Avoid carrying unused VLANs.
- Use consistent native-VLAN configuration.
- Review unexpected tagged traffic and trunk-state changes.

## Validation Strategy

A production validation should test:

- Devices receive the expected address and gateway.
- Same-VLAN communication works when required.
- Broadcast traffic remains inside the intended segment.
- Inter-VLAN traffic follows firewall and ACL policy.
- Disallowed paths are denied.
- DHCP relay and DNS resolution work correctly.
- Management interfaces are reachable only from approved systems.
- Trunks carry only authorized VLANs.
- Monitoring and logging identify the correct source zone.
- Configuration persists after reboot.

Negative tests are as important as successful connectivity tests.

## Configuration Backup and Rollback

Before changing VLANs on a real device:

- Export and protect the current configuration.
- Record port assignments, trunks, routing, and dependencies.
- Establish console or out-of-band access.
- Define automatic or manual rollback conditions.
- Schedule an approved maintenance window.
- Confirm a replacement or recovery method.
- Save the new configuration only after validation.

A VLAN mistake can remove the administrator's own management access.

## Monitoring and Detection

Potentially significant events include:

- Unexpected access-port VLAN changes
- New or unauthorized trunk ports
- Native-VLAN mismatches
- Previously unused VLANs becoming active
- MAC addresses appearing in unexpected zones
- Excessive broadcast or unknown-unicast traffic
- DHCP servers appearing on user ports
- Inter-VLAN connections denied by policy
- Management traffic originating from user or guest networks
- Configuration changes outside approved windows

Switch logs, configuration monitoring, network access control, flow telemetry, DHCP logs, and firewall events should be correlated.

## Documentation Requirements

The network source of truth should include:

- VLAN identifier and descriptive name
- Business owner and purpose
- IPv4 and IPv6 networks
- Gateway and DHCP details
- Access and trunk port assignments
- Allowed trunk VLANs
- Routing and security policy
- Critical dependencies
- Change history
- Decommission date where applicable

Documentation prevents identifier reuse, overlapping networks, and orphaned policy.

## Simulator Limitations

The browser simulator demonstrated interface navigation and configuration concepts but did not validate:

- Actual frame tagging
- Physical port behavior
- Routing or firewall enforcement
- DHCP assignment
- Spanning Tree
- Performance or failure recovery
- Configuration persistence
- Vendor CLI commands

These functions require a packet-level simulator, virtual network appliance, lab switch, or authorized physical environment for full validation.

## Security Concepts Demonstrated

- VLANs and broadcast domains
- IPv4 subnetting
- Access and trunk ports
- 802.1Q tagging
- Native and default VLANs
- Inter-VLAN routing
- Network segmentation
- Firewall and ACL policy
- Management-plane isolation
- DHCP relay
- Layer 2 security
- Change and rollback planning

## Results

- Reviewed VLAN configuration in a Cisco device simulator.
- Defined additional logical network segments.
- Associated simulated Layer 3 interfaces with distinct IPv4 networks.
- Distinguished VLAN membership from IP addressing.
- Analyzed access, trunk, native, default, and management VLAN concepts.
- Developed inter-VLAN policy, validation, monitoring, and rollback recommendations.
- Documented the limitations of simulator-only configuration.

## Key Takeaways

- VLANs separate Layer 2 broadcast domains; subnets provide Layer 3 addressing.
- Switchport assignment or 802.1Q tagging determines VLAN membership, not the host's IP address.
- Inter-VLAN routing must be paired with firewall or ACL policy to provide meaningful isolation.
- Management, guest, user, server, and device networks should have distinct trust boundaries.
- Trunks should carry only required VLANs and use consistent native-VLAN settings.
- IPv6 policy must match the intended IPv4 segmentation model.
- Simulator work demonstrates concepts but does not prove live forwarding or enforcement.

## Skills Demonstrated

- VLAN design and configuration analysis
- Layer 2 and Layer 3 differentiation
- IPv4 subnet and gateway planning
- Access and trunk port reasoning
- 802.1Q and native-VLAN analysis
- Inter-VLAN routing and policy design
- Management-plane segmentation
- DHCP and IPv6 considerations
- Layer 2 security recommendations
- Connectivity and negative testing
- Change, backup, and rollback planning
- Professional cybersecurity documentation
