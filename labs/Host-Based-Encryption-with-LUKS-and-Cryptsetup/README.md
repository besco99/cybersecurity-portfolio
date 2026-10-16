# Host-Based Encryption with LUKS and Cryptsetup

## Overview

This learning exercise examined host-based encryption and a supplied demonstration of provisioning an encrypted secondary disk on a Kali Linux virtual machine. The demonstration used cryptsetup and Linux Unified Key Setup (LUKS) to initialize encrypted storage, unlock a mapped device, create a filesystem, mount it, and close it after use.

This entry records the observed workflow and its security implications. It does not claim an independently executed encryption or recovery test. The source passphrase, exact device and mapping names, destructive commands, and proprietary course instructions are omitted.

## Environment

- Kali Linux virtual machine
- Dedicated secondary virtual disk
- cryptsetup with LUKS
- Linux device-mapper encryption
- EXT4 filesystem
- Storage inventory and mount-inspection utilities

## Objectives

- Explain protection of stored data through encryption.
- Distinguish volume encryption from file encryption.
- Understand the roles of LUKS, cryptsetup, and device mapping.
- Describe the encrypted-volume lifecycle.
- Identify safe provisioning and recovery requirements.
- Compare the roles of BitLocker, LUKS, and FileVault.

## Data at Rest and the Threat Model

Data at rest is stored data, whether or not the computer is running. Disk encryption is especially valuable against offline access after loss, theft, or removal of a storage device.

When a volume is unlocked, authorized applications can read its contents through the operating system. Malware running with sufficient access may do so as well. Encryption therefore works alongside account security, access controls, patching, endpoint protection, and physical safeguards.

## Platform Comparison

| Platform | Technology | Primary role |
| --- | --- | --- |
| Windows | BitLocker | Volume encryption, often using TPM-backed key protection |
| Linux | cryptsetup with LUKS | Provisioning and unlocking encrypted block devices |
| macOS | FileVault | Protection of startup-volume data with platform-integrated key management |

BitLocker is a volume-encryption technology rather than a general per-file encryption feature. Windows file-level options such as EFS serve a different role. LUKS defines encrypted-volume metadata and key-management structures; cryptsetup administers them, while the underlying encryption layer processes block reads and writes.

Encryption coverage depends on the selected volumes and configuration. Encrypting one secondary disk does not encrypt the operating-system disk, boot components, other disks, backups, or VM snapshots automatically.

## Demonstrated Storage Lifecycle

The supplied demonstration followed these stages:

1. Inventory attached storage and identify the dedicated test disk.
2. Confirm the selected disk is not mounted or in use.
3. Prepare the test device and initialize its LUKS metadata.
4. Configure an unlock passphrase.
5. Open the encrypted device through a mapped block-device interface.
6. Create a filesystem on the unlocked mapping.
7. Mount the filesystem and inspect its availability.
8. Unmount the filesystem and close the encrypted mapping.

The sequence demonstrates provisioning a data volume. It does not demonstrate a complete encrypted operating-system installation.

## Safe Provisioning

Initialization, overwriting, and filesystem creation can destroy existing data. A production plan should confirm the target using multiple attributes, including capacity, model, serial number, partition layout, and current mount state. Device names can change after hardware or VM configuration changes.

Use a disposable test device or verified backups, confirm that no dependent application is using the storage, and document a recovery plan before provisioning. Randomly overwriting a disk is not a universal prerequisite for encryption and should not be presented as a routine safety step. Sanitization requirements depend on the device, prior data, and organizational policy.

## Mapping and Filesystem Layers

Applications interact with a mounted filesystem. That filesystem resides on an unlocked device mapping, which translates storage operations to the encrypted underlying device.

Creating a filesystem on the raw device after initializing LUKS could damage encryption metadata. Correct layer selection is essential during provisioning and troubleshooting.

Mount visibility confirms that a filesystem is available. It does not, by itself, validate cryptographic parameters, recovery readiness, or protection of every storage location.

## Passphrases and Key Management

A strong, unique passphrase protects access to encrypted key material. LUKS keyslots support management of unlock methods without requiring the user passphrase to be the direct data-encryption key.

Operational requirements include:

- Store recovery material in an approved protected location.
- Keep recovery access separate from the protected disk.
- Limit who may add or remove unlock methods.
- Record changes and review access periodically.
- Protect header backups and recovery secrets.
- Test recovery with controlled procedures.
- Avoid embedding secrets in scripts, screenshots, or repositories.

Lost unlock material or damaged encryption metadata can make data unrecoverable. Encryption does not replace backups.

## TPM and Platform Security

A Trusted Platform Module can protect key material and support release policies tied to platform measurements. A TPM is not a substitute for a recovery plan or an appropriate user-authentication policy.

Boot integrity, secure firmware configuration, recovery-key handling, and organizational policy influence the protection provided by a deployment. The supplied Linux demonstration did not configure TPM-based unlocking.

## Closing an Encrypted Volume

Unmounting removes the filesystem from the namespace; closing the encrypted mapping removes that active access path. Applications must release their files before the volume can be closed cleanly.

A locked desktop is not equivalent to closing an encrypted data volume or powering down a system. Sleep and suspended VM states may retain access-related material in memory or saved state, so power-state policy matters for sensitive devices.

## Confidentiality, Integrity, and Availability

Storage encryption primarily addresses confidentiality. It should not automatically be treated as proof of authenticity or protection against all malicious modification. Integrity guarantees depend on the encryption mode and additional controls.

Availability requires separate planning for backups, hardware failure, metadata damage, key loss, recovery access, and incident response. Signed software, integrity monitoring, and protected audit records complement encrypted storage.

## Virtualization Considerations

Guest encryption protects data within the encrypted guest volume, but a privileged hypervisor may observe guest memory while the volume is unlocked. VM snapshots, suspended state, exported disks, and backups need their own protection and retention controls.

Document which layer owns encryption and recovery: guest operating system, hypervisor, storage platform, or several layers together.

## Proposed Validation

A follow-up test would:

1. Verify the target device and configured encryption parameters.
2. Confirm access through the intended mapping.
3. Write synthetic test data and verify it after reopening.
4. Confirm an incorrect passphrase cannot unlock the volume.
5. Confirm the filesystem is unavailable after unmounting and closing.
6. Test approved recovery procedures on a disposable copy.
7. Verify backups and snapshots meet protection requirements.
8. Document results without exposing secrets.

These checks are proposed validation steps; no independent execution or recovery evidence was supplied.

## Results and Limitations

The supplied demonstration showed the secondary volume being initialized, unlocked, formatted, mounted, inspected, unmounted, and closed. It illustrated the lifecycle of encrypted Linux data storage.

It did not establish complete host encryption, benchmark performance, test key recovery, configure TPM integration, or validate a production deployment.

## Key Takeaways

- Volume encryption reduces the risk of offline disclosure from lost or stolen storage.
- Unlocked volumes remain accessible according to operating-system permissions.
- LUKS, cryptsetup, device mapping, and the filesystem have distinct roles.
- Correct device identification is essential before destructive provisioning.
- Recovery keys, metadata backups, and data backups require separate protection.
- Encrypting a secondary disk does not establish full-host coverage.
- VM snapshots and suspended state require explicit security policies.

## Skills and Concepts Reviewed

- Host-based encryption
- Linux encrypted-volume provisioning
- LUKS and cryptsetup architecture
- Device mapping and filesystem layers
- Passphrase and recovery-key governance
- TPM concepts
- Confidentiality and integrity boundaries
- Virtualization and storage protection
- Recovery and validation planning
