# Windows Server 2022 Virtual Machine Deployment and Network Configuration

## Overview

This lab established a Windows Server 2022 virtual machine for use in an isolated Microsoft infrastructure and cybersecurity training environment. I obtained official evaluation media, created a new Oracle VirtualBox guest, selected an edition with Desktop Experience, completed a clean installation, configured the initial administrator account, planned a stable network identity, and shut down the server gracefully.

The resulting VM provides a foundation for future Active Directory, DNS, DHCP, file services, backup, database, logging, and Windows administration labs. This portfolio entry does not include administrator credentials, passwords, IP addresses, product keys, account names, host-specific paths, exact resource allocations, download links, or proprietary lesson instructions.

## Lab Environment

- Windows host computer
- Oracle VirtualBox
- Official Windows Server 2022 evaluation media
- 64-bit Windows Server guest
- Desktop Experience installation
- Dynamically allocated virtual disk
- Isolated virtual network
- Existing Windows client and Kali Linux lab guests

## Objectives

- Obtain trusted Windows Server evaluation media.
- Create and size a Windows Server virtual machine.
- Attach bootable ISO media and select the correct edition.
- Compare Desktop Experience with Server Core.
- Complete a clean server installation.
- Secure the built-in administrative identity.
- Configure a predictable network address appropriately.
- Understand DNS and gateway dependencies.
- Perform a documented, graceful server shutdown.
- Prepare a clean baseline for later infrastructure labs.

## Authorization, Licensing, and Lifecycle

The server was deployed for non-production educational use. Evaluation software is time-limited and governed by Microsoft's current licensing terms. It should not be used to operate a production business or to avoid proper licensing.

Before deployment, verify the current evaluation period, activation requirements, support lifecycle, and conversion options directly through official Microsoft documentation. Lab snapshots do not pause licensing obligations or eliminate the need to use supported software.

## Trusted Media Acquisition

The installation ISO was obtained through Microsoft's official evaluation workflow. Security considerations include:

- Use an official vendor domain.
- Verify the HTTPS connection and avoid third-party repackaged images.
- Compare checksums or signatures when available.
- Record the edition, language, architecture, and download date.
- Protect any registration information submitted to obtain the evaluation.
- Store the ISO in an access-controlled location.
- Do not embed product keys or credentials in shared VM configurations.

## Virtual Machine Creation

I created a new VirtualBox guest, selected the Windows Server family and architecture, attached the evaluation ISO, and chose a manual installation rather than an automated unattended workflow.

A manual setup was appropriate because it allowed direct control over:

- Edition selection
- Installation type
- Disk selection
- Licensing acknowledgment
- Initial administrator configuration
- Regional and keyboard settings

Unattended installation can be useful for repeatable deployment when its answer files, credentials, and keys are generated and protected securely.

## Resource Planning

The VM received sufficient memory, processor capacity, and virtual disk space for the planned training workload without exhausting the host.

### Memory

Desktop Experience, server roles, updates, and security tooling require more memory than a minimal command-line installation. Capacity should also be reserved for the Windows client, Kali system, and host operating system.

### Processor

Multiple virtual processors can improve installation and role performance, but excessive allocation may create host contention. Begin with a conservative assignment and measure actual utilization.

### Storage

A dynamically allocated disk consumes host storage as the guest writes data. Planning should include:

- Operating-system updates
- Directory-service databases
- DNS and DHCP logs
- File shares and database files
- Event logs and packet captures
- Backups and restore points
- VirtualBox snapshots

Production-like server labs should monitor free space because storage exhaustion can interrupt critical services.

## Edition and Interface Selection

The lab selected a standard server edition with Desktop Experience. The graphical interface simplified learning and made management tools readily accessible.

Server Core omits most graphical components and can offer:

- Smaller disk and memory footprint
- Reduced servicing requirements
- Smaller attack surface
- Fewer local interactive-management components

Desktop Experience is useful for introductory labs, while Server Core is often preferable when operational requirements and administrator skills support remote or command-line management.

## Clean Installation

The server was installed to a new virtual disk using a clean custom installation rather than an in-place upgrade. Setup copied the operating-system files, completed several automatic restarts, and prompted for initial administrative configuration.

The installation media should be detached or placed after the virtual disk in the boot order once setup is complete to avoid returning to the installer unexpectedly.

## Administrator Account Security

The initial built-in administrator account was assigned a lab-specific password. The supplied lesson password is intentionally omitted and should never be reused.

Recommended controls include:

- Use a long, unique password generated and stored securely.
- Do not use common substitutions to make a dictionary word appear complex.
- Create named administrative accounts for routine accountability.
- Use a standard account for nonadministrative work.
- Restrict where privileged identities may sign in.
- Enable appropriate lockout, auditing, and multifactor controls where supported.
- Rotate credentials after lab sharing or suspected exposure.

## Network Configuration

Server roles often require a stable address so clients and other services can locate the host consistently. The lab planned a fixed address on an isolated private subnet.

A stable server address can be provided through either:

- A manually configured static address outside the DHCP pool, or
- A DHCP reservation that consistently assigns the same address.

Servers do not universally require manual static addressing. The correct method depends on the environment's availability, change management, network automation, and recovery design.

## Address Planning

Before configuring a server address, document:

- IP address and prefix length
- Subnet and VLAN
- Default gateway, if routing is required
- Preferred and alternate DNS resolvers
- DHCP scope and exclusions
- Hostname and planned DNS records
- Other systems using the same address range

Duplicate addresses and incorrect prefixes can produce intermittent, difficult-to-diagnose failures.

## Gateway Considerations

The isolated lab server did not require internet access. A default gateway should be configured only when the server must reach systems outside its local subnet.

Omitting a gateway can strengthen isolation, but it also prevents communication with routed management, update, logging, time, backup, or authentication services. The choice should be intentional and validated against the lab architecture.

## DNS Configuration Considerations

The lab intended to add the DNS Server role later. Pointing a server to itself as a DNS resolver is appropriate only after the DNS service and required zones or forwarders are configured correctly.

Before that role exists, a self-referencing resolver entry can break name resolution. A safer sequence is:

1. Use an approved working resolver during initial setup when name resolution is required.
2. Install and configure the DNS role.
3. Create or validate zones and forwarders.
4. Change the server's resolver settings according to the final design.
5. Test internal and external resolution as applicable.

For domain controllers, DNS client settings require additional Active Directory-specific planning.

## Network Profile and Firewall

The active Windows network profile affects firewall behavior. The profile should be selected based on network trust and domain status rather than convenience.

- Keep Windows Defender Firewall enabled.
- Permit only the ports required by installed roles.
- Scope inbound rules to approved source networks.
- Remove temporary installation and troubleshooting rules.
- Review outbound access for sensitive or isolated servers.
- Document every intentional exception.

Changing a profile should not be used as a substitute for precise firewall rules.

## Initial Server Baseline

Before installing additional roles:

- Rename the server according to a documented convention.
- Configure time and time-zone settings.
- Apply supported updates through an approved path.
- Verify firewall and endpoint protection status.
- Review local users, groups, and privileges.
- Disable unnecessary services and remote access.
- Configure event-log sizes and forwarding where appropriate.
- Establish backup and snapshot procedures.
- Record the baseline configuration.

## Future Lab Roles

The VM can support later authorized exercises involving:

- Active Directory Domain Services
- DNS
- DHCP
- Group Policy
- File and print services
- Windows Server Backup
- Certificate services
- Web services
- Database platforms
- Centralized logging and monitoring

Roles should be added one at a time with a clean snapshot and documented network dependencies.

## Server Core vs. Desktop Experience Security

Choosing Server Core can reduce locally installed components and patching surface, but it does not automatically make the server secure. Both installation types require:

- Secure identity and privilege management
- Timely updates
- Host firewall configuration
- Role-specific hardening
- Logging and monitoring
- Network segmentation
- Backup and recovery testing

The interface choice should support the required workload and administration model.

## Snapshot and Backup Strategy

Useful recovery points include:

1. Clean installation before customization.
2. Patched and hardened baseline.
3. Stable network configuration.
4. Pre-role installation.
5. Pre-change snapshot before risky labs.

Snapshots are not backups and should not be retained indefinitely. Important server data requires an independent, tested backup strategy.

## Graceful Shutdown

The lab used the operating system's shutdown process rather than abruptly powering off the VM. Windows Server records a shutdown reason to support operational accountability and troubleshooting.

Graceful shutdown allows services to stop, buffers to flush, and file systems to close cleanly. Forced power-off should be reserved for recovery situations and followed by integrity checks.

## Host and Guest Isolation

- Use host-only or internal networking for isolated infrastructure labs.
- Avoid bridged networking for vulnerable guests unless explicitly required.
- Disable unnecessary shared clipboard, drag-and-drop, USB, and shared folders.
- Do not use personal credentials inside lab servers.
- Keep lab backups separate from production data.
- Prevent untrusted guests from reaching host management services.
- Revert or rebuild systems after destructive testing.

## Verification Checklist

- Installation media came from an official source.
- The expected server edition and architecture are installed.
- Virtual hardware settings match the lab design.
- The server boots from its virtual disk.
- Administrator credentials are unique and protected.
- Hostname and system time are correct.
- Network address configuration is documented and conflict-free.
- Gateway and DNS settings match actual dependencies.
- Firewall rules permit only required traffic.
- Evaluation and support status are recorded.
- A clean recovery snapshot exists.

## Troubleshooting Lessons

### Unattended Installation Failure

Use a correctly secured answer file or perform a manual installation when evaluation media, edition selection, or credential requirements do not match the automated workflow.

### No Network Access

Check the VirtualBox adapter mode, guest interface state, prefix length, gateway, firewall, and virtual-network membership.

### Name Resolution Failure

Confirm that the configured DNS resolver is reachable and actually running a correctly configured DNS service. Do not point to an inactive local resolver.

### Address Conflict

Review the DHCP pool, reservations, static assignments, and neighbor tables. Move manual server addresses outside dynamically assigned ranges unless reservations coordinate them.

### Slow Performance

Check host contention, guest resources, storage latency, background updates, and concurrent virtual machines before increasing allocations.

## Security Concepts Demonstrated

- Windows Server deployment
- Evaluation licensing awareness
- Trusted installation media
- Virtual hardware planning
- Server Core and Desktop Experience
- Administrative credential security
- Static addressing and DHCP reservations
- DNS and gateway dependencies
- Windows Firewall profiles
- Server baselining
- Snapshot and backup planning
- Graceful shutdown and accountability

## Results

- Obtained official Windows Server 2022 evaluation media.
- Created and configured a VirtualBox server guest.
- Selected and installed the Desktop Experience edition.
- Completed initial administrative configuration.
- Planned a stable isolated-network address.
- Evaluated gateway and DNS requirements.
- Performed a graceful, documented shutdown.
- Established a baseline for future Microsoft infrastructure labs.

## Key Takeaways

- Evaluation software remains subject to time, activation, licensing, and support constraints.
- Server Core reduces installed components, while Desktop Experience can simplify learning and local management.
- Common-looking complex passwords remain weak if they are predictable or reused.
- Stable server addressing can use either a static assignment or a managed DHCP reservation.
- Self-referencing DNS works only after the server is intentionally configured to resolve queries.
- Isolated servers may omit a gateway, but all routed dependencies must be considered.
- Graceful shutdown protects service and file-system integrity.

## Skills Demonstrated

- Windows Server 2022 deployment
- Oracle VirtualBox administration
- Installation-media and edition selection
- Server resource planning
- Administrative credential hardening
- IPv4 address planning
- DNS and gateway dependency analysis
- Windows firewall and network-profile reasoning
- Server baseline design
- Snapshot and backup strategy
- Technical troubleshooting
- Professional cybersecurity documentation
