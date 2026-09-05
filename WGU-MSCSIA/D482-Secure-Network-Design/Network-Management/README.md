# Network Management

## Overview

This lab explored core network and system-management activities: establishing performance baselines, monitoring resource utilization, analyzing Windows event logs, and reviewing patch-management controls. I used built-in Windows tools to compare idle and active workloads, correlate processes with network consumption, investigate a failed sign-in event, and assess operating-system update settings.

All work was completed in an authorized, isolated virtual environment. Credentials, internal addressing, challenge answers, exact traffic-generation commands, and proprietary course instructions are intentionally excluded.

## Lab Environment

- Isolated virtual network
- Windows Server system
- Linux security-testing system
- Windows Performance Monitor
- Windows Resource Monitor
- Windows Event Viewer
- Windows Update configuration interface
- Controlled CPU and network workloads

## Objectives

- Establish an idle CPU-utilization baseline.
- Compare baseline performance with an active workload.
- Add and scale a network-interface performance counter.
- Correlate network utilization with responsible processes.
- Review Windows application, security, and system logs.
- Generate and identify a failed sign-in event.
- Interpret common event severity and audit categories.
- Review update classifications and installation controls.
- Apply monitoring and patch management as security-hardening practices.

## Network Management Functions

Effective network management combines several operational disciplines:

- **Administration:** Tracking devices, infrastructure, and configuration.
- **Operations:** Monitoring performance, availability, logs, and security state.
- **Maintenance:** Repairing, patching, and upgrading systems and network resources.
- **Provisioning:** Expanding capacity and deploying resources as organizational needs change.

These activities support both reliability and security. Performance anomalies can reveal operational problems or hostile activity, logs provide evidence for investigation, and timely patching reduces exposure to known vulnerabilities.

## Establishing a CPU Baseline

I used Performance Monitor to observe processor utilization while the Windows server was idle. The initial measurements established a baseline representing expected resource use under minimal load.

I then generated a controlled disk-analysis workload and observed the resulting processor spike. Comparing the active workload with the idle baseline demonstrated how sustained resource contention can affect system responsiveness and how baselines help distinguish expected changes from abnormal behavior.

## Monitoring Network Utilization

I replaced the default processor counter with a network-interface counter that measured received bytes per second. After generating controlled network traffic from another system in the isolated environment, I observed a substantial increase in inbound utilization.

Because the measurement exceeded the graph's initial display range, I adjusted the counter scale so the activity remained visible and interpretable. This reinforced an important monitoring principle: visualization settings must match the magnitude and units of the collected data, or meaningful activity may be misread or disappear from view.

## Using Resource Monitor

Resource Monitor provided a complementary view of CPU, memory, disk, and network utilization. In the Network tab, I reviewed:

- Processes consuming network resources
- Process identifiers
- Remote endpoints
- Bytes sent and received
- Protocol usage
- Listening ports
- Firewall status associated with listening ports

Filtering by process made it easier to focus on a suspected source of utilization. Performance Monitor was useful for customized time-series counters, while Resource Monitor offered faster process-level attribution across several resource categories.

## Analyzing Windows Event Logs

I used Event Viewer to review administrative events and the primary Windows logs. The investigation included common event classifications:

- **Critical:** A severe failure from which the application or system could not recover.
- **Error:** A significant service, application, or operating-system problem.
- **Warning:** A condition that is not immediately critical but warrants investigation.
- **Information:** Routine confirmation of an operation or service state.
- **Audit Success:** A security operation that completed successfully.
- **Audit Failure:** A security operation that was attempted but did not succeed.

I examined the Application, Security, and System logs to understand how each supports troubleshooting and security monitoring.

## Investigating a Failed Sign-In

To generate a known event for analysis, I performed one controlled failed sign-in attempt and then returned to Event Viewer. After refreshing the event data, I located Windows Security event ID 4625 and reviewed its properties.

The event provided evidence about the attempted sign-in, including time, target account context, and failure details. This demonstrated how known test activity can validate audit coverage and how authentication logs support incident investigation, account monitoring, and detection engineering.

No usernames or environment-specific event details are published in this portfolio.

## Reviewing System Events

I sorted System log entries by severity and examined warning-event properties. This showed how Event Viewer can provide source, event ID, timestamps, descriptions, and supporting context for troubleshooting.

Online event assistance was unavailable in the isolated environment, reinforcing the need to interpret local evidence and use trusted vendor documentation when external research is permitted.

## Patch and Update Management

I reviewed the Windows Update interface and its controls for important and recommended updates. The lab system did not have Internet access, so no updates were downloaded or installed; the activity focused on configuration and administrative decision-making.

Key considerations included:

- Prioritizing critical security updates that address known vulnerabilities.
- Distinguishing security and operating-system updates from optional driver updates.
- Testing updates for compatibility before broad deployment.
- Scheduling installations to reduce disruption.
- Restricting update approval and installation rights appropriately.
- Allowing administrators to defer a problematic update when a documented compatibility issue exists.
- Tracking update status and exceptions rather than allowing indefinite postponement.

In production, centralized patch management should combine timely deployment targets, staged testing, rollback planning, asset inventory, compliance reporting, and risk-based exception handling.

## Security Considerations

- Establish baselines before interpreting performance deviations.
- Alert on sustained or unexplained changes rather than isolated spikes alone.
- Correlate utilization with processes, endpoints, logs, and change records.
- Protect event logs from unauthorized modification and centralize important telemetry.
- Monitor repeated authentication failures and other suspicious account activity.
- Retain logs according to operational, legal, and incident-response requirements.
- Test patches promptly and deploy security fixes according to risk.
- Document deferred updates, compensating controls, ownership, and deadlines.
- Limit administrative privileges for monitoring and update configuration.
- Generate test traffic only in authorized, isolated environments.

## Security Concepts Demonstrated

- Performance baselining
- Resource and capacity monitoring
- Network-utilization analysis
- Process-level attribution
- Windows security auditing
- Authentication-failure investigation
- Event severity classification
- Log correlation
- Vulnerability mitigation through patching
- Change and configuration management
- Defense in depth
- Operational resilience

## Results

- Established an idle processor-utilization baseline.
- Observed and interpreted a workload-driven CPU spike.
- Added and scaled a received-bytes network counter.
- Confirmed that controlled traffic changed network utilization.
- Used Resource Monitor to associate activity with processes and connections.
- Reviewed Application, Security, and System event data.
- Generated and identified a failed sign-in event.
- Examined event ID 4625 and its investigative context.
- Reviewed update classifications and installation settings.
- Identified production considerations for safe, timely patch deployment.

## Key Takeaways

- A baseline provides the context needed to distinguish normal variation from anomalies.
- A monitoring graph is only useful when counters and scales are configured correctly.
- Process-level context helps translate resource spikes into actionable findings.
- Event logs are central to troubleshooting, security monitoring, and incident reconstruction.
- Controlled test events can verify whether important activity is being logged as expected.
- Patch management requires both urgency and disciplined compatibility testing.
- Monitoring, logging, and patching are interconnected parts of secure network operations.

## Skills Demonstrated

- Windows Performance Monitor
- Windows Resource Monitor
- Performance counter configuration
- CPU and network baselining
- Process and connection analysis
- Windows Event Viewer
- Security log investigation
- Authentication-event analysis
- Event ID interpretation
- Patch and update management
- Security control validation
- Network troubleshooting
- Professional cybersecurity documentation
