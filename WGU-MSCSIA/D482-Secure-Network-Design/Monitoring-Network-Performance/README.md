# Monitoring Network Performance

## Overview

This lab focused on extending an existing network-monitoring deployment with SNMP-based interface visibility. Using PRTG Network Monitor on a Windows 10 virtual machine, I configured pfSense to expose selected interface statistics and then validated that PRTG could collect and display traffic data. The completed monitoring configuration included two SNMP Traffic sensors covering the WAN and an internal/vmx interface.

## Lab Environment

- Windows 10 virtual machine
- PRTG Network Monitor
- pfSense firewall/router
- Simple Network Management Protocol (SNMP)
- Existing Ping, DNS, and HTTP sensors
- Simulated WAN and internal/vmx network interfaces

## Objectives

- Review the health and status of existing PRTG sensors.
- Enable and securely scope the SNMP service on pfSense.
- Configure PRTG as the destination for SNMP traps.
- Add SNMP Traffic sensors for two pfSense interfaces.
- Generate web traffic and confirm that monitored traffic counters updated.
- Distinguish periodic SNMP polling from event-driven SNMP traps.

## Examining the Monitoring Environment

I began by reviewing the existing PRTG deployment and confirming that the Ping, DNS, and HTTP sensors were operational. This established a known-good monitoring baseline before adding SNMP. It also demonstrated how different sensor types provide complementary visibility: availability checks confirm that a service responds, while interface sensors show traffic volume and utilization trends.

## Configuring SNMP on pfSense

I enabled the SNMP daemon on pfSense and configured it for read-only monitoring. A non-default read community string was used, and the PRTG server was configured as the SNMP trap destination. The SNMP service was bound only to the LAN interface so that management traffic was not exposed through the WAN-facing interface.

No credentials, community strings, or environment-specific secrets are included in this documentation.

## Security Considerations

Restricting SNMP to the LAN reduces management-plane exposure by limiting where the service can be reached. This supports least-exposure and defense-in-depth principles and helps prevent unnecessary access attempts from untrusted networks.

Community-based SNMP versions provide limited security because community strings may be transmitted without strong protection. Although a non-default, read-only community string improves the lab configuration, SNMPv3 would generally be preferable in production because it supports stronger authentication, integrity protection, and encryption. Additional production safeguards should include network access controls, management VLAN segmentation, source restrictions, logging, and secure secret management.

## Adding SNMP Sensors to PRTG

After configuring pfSense, I added SNMP Traffic sensors to the pfSense device in PRTG. One sensor monitored the WAN interface, and the second monitored an internal/vmx interface. The sensors provided interface-level traffic measurements that complemented the existing service-availability sensors.

## Generating and Monitoring Network Traffic

To validate the configuration, I generated web traffic from the lab environment and observed the two SNMP Traffic sensors in PRTG. After the next polling cycles, the sensor values updated to reflect activity on the monitored interfaces. This confirmed network reachability, compatible SNMP settings, correct interface selection, and successful data collection.

## SNMP Polling vs. SNMP Traps

SNMP polling and SNMP traps serve different monitoring purposes:

- **Polling** is initiated by the monitoring server on a schedule. PRTG requests current values, such as interface counters, to build time-series data and identify trends.
- **Traps** are initiated by the managed device when a noteworthy event occurs. They provide asynchronous notification without waiting for the next polling interval.

Using both approaches can improve visibility: polling supplies consistent performance data, while traps can provide faster awareness of discrete events.

## Security Concepts Demonstrated

- Management-plane exposure reduction
- Least privilege through read-only monitoring access
- Interface binding and service scoping
- Defense in depth
- Secure monitoring architecture
- Baseline validation and change verification
- Separation of availability monitoring from performance monitoring

## Results

- Verified the existing Ping, DNS, and HTTP sensors.
- Enabled the pfSense SNMP daemon for read-only monitoring.
- Applied a non-default community value without documenting the secret.
- Configured the PRTG server as the SNMP trap destination.
- Restricted SNMP access to the LAN interface.
- Added one SNMP Traffic sensor for the WAN interface.
- Added one SNMP Traffic sensor for an internal/vmx interface.
- Generated web traffic and confirmed both sensors reported updated values.
- Finished with two operational SNMP Traffic sensors.

## Key Takeaways

- Availability sensors and traffic sensors answer different operational questions and are most useful together.
- Binding management services only to trusted interfaces materially reduces exposure.
- A successful monitoring deployment requires validation with real traffic, not configuration alone.
- SNMP polling supports historical performance analysis, while traps support event-driven awareness.
- SNMPv3 is the preferred production choice when stronger authentication and encryption are required.

## Skills Demonstrated

- Network performance monitoring with PRTG
- pfSense service configuration
- SNMP architecture and troubleshooting
- Interface-level traffic analysis
- Secure management-plane design
- Monitoring validation and baseline assessment
- Technical documentation for security operations
