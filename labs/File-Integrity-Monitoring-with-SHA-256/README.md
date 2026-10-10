# File Integrity Monitoring with SHA-256

## Overview

This learning exercise examined integrity risks and a demonstration of detecting file changes with SHA-256. The supplied demonstration used a Kali Linux VM and synthetic text files to establish a baseline, verify unchanged content, and detect a single-character edit through a changed aggregate digest.

This entry summarizes the observed demonstration and its security implications. It does not claim an independently executed test or publish proprietary instructions, source filenames, or course-specific output.

## Lab Environment

- Kali Linux virtual machine
- Synthetic text files
- SHA-256 hashing utilities
- Digest comparison with `diff`
- Local test directory and separately stored comparison output

## Objectives

- Explain how unauthorized changes affect data integrity.
- Understand cryptographic hashes and their limitations.
- Compare a baseline with a later measurement.
- Detect a small change across a collection of files.
- Distinguish change detection from proof of authenticity.
- Relate integrity monitoring to remote journaling and anti-tampering controls.

## Demonstration Methodology

The demonstration followed three stages:

1. Compute a digest for each test file, then derive an aggregate digest from the resulting list.
2. Repeat the measurement without modifying the input and compare the results.
3. Edit one character in one file, recompute the measurement, and compare it with the prior baseline.

The unchanged measurements matched. After the edit, the aggregate digest changed, demonstrating that a small content change can affect the final fingerprint.

## How Hashing Supports Integrity

A cryptographic hash produces a fixed-length digest from input bytes. SHA-256 produces a 256-bit digest. Comparing a current digest with a trusted baseline can reveal a content change.

Cryptographic hashes are designed to resist finding an input for a chosen digest, finding a second input with the same digest, and finding two inputs that collide. They are one-way functions; “trapdoor function” is not an accurate description because a trapdoor implies a secret that enables inversion.

No finite digest can guarantee uniqueness for every possible input. SHA-256 offers strong practical collision resistance, rather than a mathematical guarantee that collisions cannot exist. MD5 and SHA-1 should not be selected for security-sensitive collision-resistant verification.

## Reliable Aggregate Measurements

An aggregate digest is useful only when measurements are reproducible. A monitoring design should:

- Define the exact file set and traversal order.
- Use a stable representation of paths and digests.
- Account for files added, removed, or renamed.
- Define how symbolic links and unreadable files are handled.
- Keep generated manifests and digest outputs outside the monitored set.
- Detect hashing failures instead of accepting partial output.
- Control concurrent writes or measure a consistent snapshot.
- Record algorithm and tool versions with the baseline.

Changing file order or accidentally including an earlier output file can change the aggregate even when the intended source content is unchanged.

## Locating the Changed File

An aggregate digest signals that the collection changed but does not identify which file changed. Retaining a per-file manifest allows investigators to compare individual digests and locate additions, deletions, or modifications.

Content hashing alone does not necessarily detect permission, ownership, or timestamp changes. Those attributes need separate monitoring if they matter to the security objective.

## Protecting the Baseline

An attacker who can modify both a file and its baseline can conceal a change. Store baselines in a separately protected location with restricted write access, version history, and audit records.

For authenticity, use a digital signature or a keyed message authentication code where appropriate. A plain hash does not identify the author, prove that a change was unauthorized, or prevent modification.

## Interference and Change Control

Integrity can be affected by malicious tampering, accidental edits, faulty software, or storage errors. Controls include least privilege, authenticated administration, approved change workflows, monitoring, and tested backups.

An alert should be checked against authorized changes before being classified as an incident. Updating a baseline requires approval and evidence that the new state is trusted.

## Remote Journaling

Remote journaling sends transaction records to another location. Depending on the system, these records may support replay or recovery when combined with suitable backups. It is distinct from simply collecting general security logs, and it does not by itself guarantee a complete recoverable copy.

Protect journal transport, destination access, retention, ordering, and monitoring. Replication delay affects the amount of recent activity that could be lost during an outage.

## Anti-Tampering Controls

Physical seals and controlled access can reveal or discourage hardware interference. Software protections include signed updates, protected baselines, secure boot, file integrity monitoring, and tamper-evident audit records.

These mechanisms have different trust boundaries. A seal provides physical evidence; a digital signature validates signed content; an integrity monitor detects deviation from an expected state.

## Investigation and Recovery

When an integrity alert occurs:

1. Preserve the current file, manifest, logs, and baseline.
2. Identify the affected files through per-file comparison.
3. Review approved changes and relevant account activity.
4. Determine scope and cause using endpoint and application evidence.
5. Restore from a verified source if required.
6. Confirm the restored state and document closure.

## Results and Limitations

The supplied demonstration showed matching hashes for unchanged inputs and a different aggregate after a one-character edit. It illustrated change detection using synthetic files.

No independent execution, performance measurement, production deployment, signed-baseline validation, or remote-journaling recovery test was provided. These remain design considerations rather than demonstrated outcomes.

## Key Takeaways

- Hash comparison can detect small content changes when the baseline is trusted.
- Aggregate hashing requires a deterministic file set and ordering.
- Per-file manifests help identify the source of an aggregate mismatch.
- Hashes detect changes but do not establish who changed a file or why.
- Baseline protection is as important as algorithm choice.
- Remote journaling, backups, and tamper-evident logging serve complementary roles.

## Skills and Concepts Reviewed

- SHA-256 and collision resistance
- File integrity monitoring
- Baseline and manifest design
- Digest comparison
- Evidence preservation
- Change-control analysis
- Remote journaling and recovery concepts
- Hardware and software anti-tampering
- Professional security documentation
