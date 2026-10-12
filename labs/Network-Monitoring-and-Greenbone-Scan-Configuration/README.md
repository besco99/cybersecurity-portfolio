# Network Monitoring and Greenbone Scan Configuration

## Overview

This learning exercise reviewed network monitoring and detection, followed by a demonstration of customizing a Greenbone vulnerability scan configuration. The supplied demonstration used a Kali Linux VM to clone a scan profile, inspect vulnerability-test families, review an individual test, and change the selected test set.

This entry documents the demonstrated configuration workflow and its defensive implications. It does not claim an independently executed scan, measured performance improvement, or verified vulnerability finding. Proprietary instructions, exact test counts, and course-specific identifiers are omitted.

## Environment and Tools Reviewed

- Kali Linux virtual machine
- Greenbone management interface
- OpenVAS scanning engine
- Scan configurations and vulnerability-test families
- Network TAP and SPAN concepts
- Packet, flow, and log collectors

## Objectives

- Explain how network visibility supports detection.
- Compare TAPs, mirrored ports, and virtual traffic mirroring.
- Distinguish packet capture from flow and log collection.
- Understand scan profiles, test families, and individual vulnerability tests.
- Evaluate the efficiency and coverage tradeoffs of customization.
- Interpret test metadata without treating it as proof of an affected asset.

## Network TAPs

A network TAP copies traffic at a selected observation point for delivery to monitoring equipment. It can support packet analysis, troubleshooting, and intrusion detection.

Visibility is limited to traffic crossing that point. Encrypted payloads remain encrypted unless separately decrypted through an authorized mechanism. Port layout, directional outputs, power requirements, aggregation, and failure behavior depend on the TAP design.

Passive optical TAPs split a signal and require attention to optical power budgets. Active or powered designs may regenerate signals or aggregate traffic. Neither design guarantees visibility through every network failure; bypass behavior and loss of power must be evaluated for the actual device.

## TAPs, SPAN, and Virtual Mirroring

| Method | Typical role | Main considerations |
| --- | --- | --- |
| Physical TAP | Dedicated observation of a link | Placement, capacity, signal budget, power, and bypass behavior |
| SPAN or mirrored port | Switch-based traffic copying | Oversubscription, packet loss, direction selection, and switch configuration |
| Virtual traffic mirroring | Observation of supported virtual interfaces | Platform support, filtering, routing, cost, and capture capacity |

Capture completeness should be validated using packet-loss counters and expected traffic. A monitoring device cannot detect activity that never reaches its observation point.

## Collectors and Data Sources

Collectors receive or retrieve telemetry and deliver it for analysis. Different data types answer different questions:

- **Packets:** Protocol behavior and visible request or response content.
- **Flows:** Connections, volumes, timing, and communicating endpoints.
- **Logs:** Authentication, process activity, policy decisions, and application events.
- **Metrics:** Utilization, latency, error rates, and service health.

Wireshark supports packet capture and protocol analysis. Log platforms can ingest and correlate events from endpoints, servers, and security devices. A complete monitoring design defines collection separately from storage, analysis, and alert handling.

## Telemetry Quality

Useful monitoring requires:

- Appropriate sensor placement
- Sufficient capture and storage capacity
- Consistent timestamps
- Protected transport and access
- Source identification and normalization
- Retention aligned with investigation needs
- Detection of collector failures and missing data
- Privacy controls for captured content

Bandwidth spikes or unusual connections justify investigation, but do not by themselves establish malicious activity.

## Vulnerability Scanning and Detection

Vulnerability scanning identifies potential weaknesses through active checks or authenticated inspection. Traffic monitoring observes activity and behavior. The two functions complement each other: scanner findings supply exposure context, while monitoring may reveal attempts to exploit that exposure.

Findings depend on feed freshness, target availability, credentials, selected checks, and detection quality. A scan with no reported vulnerabilities is not proof that a system is secure.

## Demonstrated Configuration Workflow

The supplied demonstration showed:

1. Reviewing available scan configurations.
2. Cloning an existing profile to create an editable copy.
3. Inspecting groups of vulnerability tests.
4. Opening a family to review individual tests.
5. Examining a test's description and severity metadata.
6. Deselecting one test to illustrate selective coverage.

The demonstration ended at configuration review. No completed scan or resulting host findings were supplied.

## Test Families and Individual Tests

Vulnerability tests are grouped into families to simplify selection and management. An individual test may identify a product, inspect a configuration, infer a vulnerable version, or perform a specific detection check.

A test catalog describes what the scanner can check. Opening a test's details does not establish that a target has the corresponding vulnerability. That requires a scan result and supporting evidence.

Family names and test counts can change as feeds evolve, so configurations should be recorded with their feed state and version.

## Customization and Coverage

Targeted profiles can reduce irrelevant checks, but narrowing a profile can also omit important dependencies and vulnerabilities. A web server still relies on its operating system, network services, identity controls, and supporting software.

For each exclusion, document:

- The reason and approving owner
- Affected targets
- Checks or dependencies that may be lost
- Alternative assessment coverage
- Expiration or review date

Removing a command-injection test from a profile does not remediate command injection. It prevents that selected check from contributing evidence to the assessment.

## Severity and Risk

A CVSS score summarizes technical severity using defined metrics. Remediation priority also depends on asset criticality, exposure, exploit prerequisites, known exploitation, and compensating controls.

Review the actual finding's detection evidence and confidence. Version-based inference and authenticated package inspection offer different levels of assurance, and both may need validation against vendor guidance.

## Authorized Scan Planning

Before running a customized profile, define the approved target scope, scan window, credentials, concurrency limits, operational contacts, and stop conditions. Test configuration changes on representative lab systems before broader use.

Avoid assuming that a profile name guarantees harmless behavior. Inspect selected checks and scanner settings, particularly for fragile services or tests that could affect availability.

## Validation Plan

A follow-up evaluation would:

1. Record the original profile and feed version.
2. Document the cloned profile's selection changes.
3. Verify required detection dependencies remain available.
4. Compare baseline and custom scans on the same authorized targets.
5. Review authentication success, errors, duration, and coverage.
6. Investigate missing findings before accepting the custom profile.
7. Approve and version the configuration.
8. Review it after feed, asset, or risk changes.

These are proposed validation steps, not completed outcomes from the supplied demonstration.

## Results and Limitations

The demonstration illustrated profile cloning, test-family navigation, test-detail review, and selective exclusion. The lesson also reviewed monitoring architecture and collector roles.

No physical TAP deployment, independent scan execution, packet capture, measured time saving, or detection-quality comparison was provided. The documentation therefore distinguishes observed configuration behavior from proposed operational validation.

## Key Takeaways

- Sensor placement determines which activity is visible.
- Packet, flow, log, and metric data provide complementary evidence.
- Capture loss and collector outages create detection gaps.
- Custom scan configurations need documented coverage decisions.
- Excluding a test does not remove the underlying risk.
- Test metadata is different from a confirmed target finding.
- Technical severity informs prioritization alongside business context.

## Skills and Concepts Reviewed

- Network monitoring architecture
- TAP, SPAN, and virtual mirroring
- Telemetry collection and quality
- Greenbone scan-profile customization
- Vulnerability-test family analysis
- Coverage and dependency review
- Severity interpretation
- Scan governance and validation planning

## Related Portfolio Entries

- [Nmap and Greenbone Vulnerability Assessment](../Nmap-and-Greenbone-Vulnerability-Assessment/)
- [Greenbone Vulnerability Scan Scheduling and Reporting](../Greenbone-Vulnerability-Scan-Scheduling-and-Reporting/)
- [Monitoring Network Performance](../../WGU-MSCSIA/D482-Secure-Network-Design/Monitoring-Network-Performance/)
