# Windows 10 Virtual Machine Deployment with VirtualBox

## Overview

This lab established a Windows 10 virtual machine for use in an isolated cybersecurity training environment. I obtained installation media through an official Microsoft workflow, created and sized a new Oracle VirtualBox guest, attached the ISO as bootable virtual media, installed the Professional edition required for later administrative labs, and completed the initial operating-system setup.

The exercise created a reusable Windows endpoint for security configuration, identity, networking, logging, and testing activities. This portfolio entry does not include product keys, account names, passwords, host-specific paths, exact hardware allocations, download links, or proprietary lesson instructions.

## Lab Environment

- Windows host computer
- Oracle VirtualBox
- Official Windows 10 installation media
- 64-bit Windows 10 Professional guest
- Dynamically allocated virtual disk
- Isolated or controlled virtual networking
- Local non-production lab account

## Objectives

- Obtain trusted Windows installation media.
- Create a correctly typed 64-bit virtual machine.
- Allocate appropriate memory, processor, and storage resources.
- Attach an ISO as virtual optical media.
- Troubleshoot a VM that cannot find bootable media.
- Perform a clean Windows installation.
- Select an edition that supports required administrative features.
- Prepare the guest for later cybersecurity labs.

## Authorization, Licensing, and Lifecycle

The VM was created for non-production educational use. Installation media should be obtained only from Microsoft or another authorized source, and Windows must be used in accordance with applicable licensing and activation terms.

A legacy operating system can be useful for isolated compatibility or security education, but it should not be exposed to untrusted networks or used as a production endpoint when it no longer receives the security support required by the organization. Lab images should be segmented, monitored, and replaced with supported versions when the learning objective allows.

## Obtaining Trusted Installation Media

The lab used Microsoft's official media-creation workflow to produce a 64-bit ISO suitable for a virtual machine. An ISO is a disk-image file that VirtualBox can mount as if it were physical installation media.

Security considerations include:

- Use an official vendor source.
- Verify the download domain and TLS connection.
- Record the selected edition, language, and architecture.
- Validate a published checksum when the vendor provides one.
- Store the ISO in a protected, clearly named location.
- Avoid modified images from unknown third parties.
- Do not embed credentials or product keys in publicly shared images.

## Virtual Machine Creation

I created a new VirtualBox guest and selected the appropriate Windows family, version, and architecture. The VM configuration included virtual memory, processor resources, and a new virtual disk.

Resource selection was based on the host's available capacity and the intended lab workload. The guest must receive enough resources to operate reliably without starving the host or other virtual machines.

## Resource Planning

### Memory

Memory allocation should leave sufficient capacity for the host operating system and concurrent lab systems. Excessive assignment can cause host paging or prevent other VMs from starting, while insufficient memory can make installation and security tooling unstable.

### Processor

Additional virtual processors can improve installation and analysis performance, but they should not exceed a sensible share of the host's available logical processors. More assigned processors do not always improve a lightly loaded guest.

### Storage

A dynamically allocated virtual disk reserves a maximum logical capacity but consumes host storage as data is written. This conserves space initially, though the disk can still grow substantially.

Capacity planning should account for:

- Windows updates
- Security tools
- Packet captures and logs
- Memory dumps
- Snapshots
- Temporary lab artifacts
- Free space required for reliable operation

## Boot Media Configuration

The first start did not find a bootable operating system because the Windows ISO had not yet been attached to the virtual optical drive. I selected the trusted ISO, mounted it, and retried the boot.

This troubleshooting step demonstrated the virtual equivalent of inserting installation media into a physical computer. A VM with an empty virtual disk needs bootable media or a preinstalled disk image before it can start an operating system.

## Windows Installation

The VM successfully booted from the mounted ISO and entered Windows Setup. I selected a clean custom installation using the virtual disk rather than attempting to upgrade an existing operating system.

The Professional edition was selected because later labs may require features commonly used in managed or security-focused Windows environments, such as:

- Advanced local user and group management
- Group Policy administration
- Remote Desktop hosting
- Domain-join capabilities
- Additional security and management controls

Edition selection should reflect the lab's requirements and the licensing available to the user or organization.

## Initial Setup

After installation and several automatic restarts, I completed regional, keyboard, and local lab-account setup. The account and any authentication information remain private and are not included in this repository.

Before using a Windows VM for security exercises, the baseline should also include:

- Correct time zone and clock synchronization
- Current supported updates where the lab permits them
- Appropriate network profile and firewall settings
- Endpoint protection state
- A known administrative-account strategy
- Removal of unnecessary services and software
- Documented snapshot and rollback procedures

## VirtualBox Guest Integration

Guest integration tools can improve display resizing, pointer behavior, time synchronization, and device support. They should be installed only from the trusted hypervisor distribution and kept compatible with the host version.

Shared clipboard, drag-and-drop, shared folders, and USB pass-through can create paths between the guest and host. For malware or exploitation labs, these features should be disabled unless explicitly required.

## Network Isolation

The network mode should match the learning objective:

| Mode | Typical use | Security consideration |
| --- | --- | --- |
| NAT | Limited outbound access through the host | Guest can still reach external services unless further restricted. |
| Host-only | Communication between host and lab guests | Useful for isolated labs without ordinary internet access. |
| Internal network | Communication only among selected guests | Strong separation from the host network when configured correctly. |
| Bridged | Guest appears directly on the physical network | Highest exposure and generally unsuitable for vulnerable lab systems. |

Vulnerable or malware-analysis guests should not use bridged networking unless the design has been explicitly risk-assessed and authorized.

## Snapshot Strategy

Snapshots provide useful recovery points but are not backups. A practical lab strategy includes:

1. A snapshot after clean installation.
2. A snapshot after updates and core security configuration.
3. A snapshot before each destructive or high-risk exercise.
4. Clear names, dates, and descriptions.
5. Periodic cleanup to control storage growth.

Important VM files should also be backed up separately when recovery matters.

## Security Baseline Recommendations

- Apply supported security updates.
- Enable the host firewall for all applicable profiles.
- Keep endpoint protection active except during explicitly designed, isolated tests.
- Use standard-user privileges for routine activity.
- Enable useful event logging and forward logs where appropriate.
- Disable unnecessary services, sharing, and remote access.
- Use unique test credentials and never reuse real passwords.
- Apply application-control and attack-surface-reduction rules where supported by the lab.
- Record intentional deviations from the secure baseline.

## VM Template and Cloning Considerations

A clean VM can become a reusable template for future exercises. Before cloning:

- Remove temporary files and sensitive test artifacts.
- Do not preserve shared credentials or private keys.
- Ensure each clone receives a unique identity where necessary.
- Avoid duplicate static network addresses.
- Regenerate security-agent identifiers according to vendor guidance.
- Document the template version and patch level.
- Validate activation and licensing for cloned instances.

## Verification Checklist

- The VM boots without the installation ISO after setup.
- The expected Windows edition and architecture are installed.
- Assigned memory, processor, and disk resources match the lab design.
- The virtual disk has sufficient free space.
- Display and input integration function correctly.
- Time, locale, and keyboard settings are correct.
- Network connectivity matches the intended isolation mode.
- Firewall and endpoint protection states are documented.
- No product keys, passwords, or personal accounts appear in the image or notes.
- A known-clean recovery snapshot exists.

## Troubleshooting Lessons

### No Bootable Medium

Confirm that the ISO is mounted to the virtual optical drive and appears before the empty disk in the effective boot sequence.

### 64-Bit Guest Option Missing

Verify that hardware virtualization is enabled in firmware and that another host hypervisor is not preventing VirtualBox from using the required features.

### Poor Performance

Review host resource pressure, guest memory, processor allocation, disk availability, background updates, and competing VMs.

### Display Scaling Problems

Use appropriate VirtualBox display settings and trusted guest integration tools rather than relying only on host-window scaling.

### Network Exposure

Confirm the selected adapter mode, disable unnecessary adapters, and test that the guest cannot reach networks outside the intended lab boundary.

## Security Concepts Demonstrated

- Virtualization fundamentals
- Trusted installation media
- VM resource planning
- Bootable ISO configuration
- Clean operating-system installation
- Windows edition selection
- Network segmentation
- Host and guest isolation
- Secure baseline configuration
- Snapshot and recovery planning
- Template hygiene
- Licensing awareness

## Results

- Obtained official Windows installation media.
- Created a 64-bit Windows virtual machine in VirtualBox.
- Allocated processor, memory, and dynamically growing storage.
- Diagnosed and resolved missing boot-media configuration.
- Completed a clean Windows 10 Professional installation.
- Finished initial regional and local-account setup.
- Established recommendations for isolation, hardening, snapshots, and future lab use.

## Key Takeaways

- Trusted installation media is the foundation of a reliable security lab.
- VM resources should be sized for both guest performance and host stability.
- An empty virtual disk cannot boot until installation media is attached.
- Edition selection affects which administrative and security features are available.
- Shared folders, clipboard integration, and bridged networking can weaken lab isolation.
- Snapshots support rollback but do not replace backups.
- Licensing, activation, and operating-system support requirements still apply in virtual labs.

## Skills Demonstrated

- Oracle VirtualBox administration
- Windows installation-media preparation
- Virtual hardware configuration
- ISO mounting and boot troubleshooting
- Clean Windows deployment
- Resource and storage planning
- Virtual network isolation design
- Windows lab baseline planning
- Snapshot and recovery strategy
- Template security and hygiene
- Technical troubleshooting
- Professional cybersecurity documentation
