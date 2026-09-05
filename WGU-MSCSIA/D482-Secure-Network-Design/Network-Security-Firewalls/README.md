# Network Security - Firewalls

## Overview

This lab explored host-based firewall administration across Windows and Linux systems. I enabled Windows Firewall, compared network profiles, configured narrowly scoped service exceptions, used Windows Firewall with Advanced Security (WFAS) to manage an inbound ICMP rule, and applied a Linux firewall rule to block an insecure outbound protocol.

The work demonstrated how firewall policy supports defense in depth by protecting individual hosts even when perimeter controls are bypassed or a threat originates inside the network. All activities were completed in an authorized, isolated environment. Credentials, internal addressing, challenge answers, and proprietary course instructions are intentionally excluded.

## Lab Environment

- Isolated virtual network
- Windows Server systems
- Windows Firewall through Control Panel
- Windows Firewall with Advanced Security
- Linux security-testing system
- Linux packet-filtering tools
- Remote Desktop Protocol (RDP)
- Internet Control Message Protocol (ICMP)
- Telnet used only to demonstrate insecure legacy traffic

## Objectives

- Enable host-based firewall protection on Windows Server.
- Compare domain, private, and public firewall profiles.
- Review application exceptions in the basic Windows Firewall interface.
- Configure and validate a Remote Desktop exception.
- Examine inbound and outbound policy in WFAS.
- Enable and validate an inbound ICMP echo-request rule.
- Inspect a Linux firewall ruleset.
- Block insecure outbound Telnet traffic and verify the result.
- Apply least-exposure principles to firewall configuration.

## Firewall Concepts

A network-based firewall filters traffic moving between network segments, while a host-based firewall protects an individual system. Using both provides layered protection: perimeter controls reduce traffic entering or leaving a network, and endpoint controls limit what can reach each host.

Stateful firewalls track active conversations and use connection state when evaluating traffic. This allows legitimate response traffic associated with an established session while unsolicited inbound traffic remains blocked unless a rule explicitly permits it.

## Enabling Windows Firewall

I enabled Windows Firewall on a Windows Server and reviewed its behavior across private and public network locations. The firewall was configured to remain active for its profiles even when a corresponding network type was not currently connected.

The default inbound behavior blocked unsolicited connections unless an applicable allow rule existed. This demonstrated why host-based protection remains important after a threat crosses the network perimeter or originates from another internal system.

## Reviewing Windows Firewall Profiles and Exceptions

Using the Control Panel interface, I reviewed installed applications and the profiles on which their exceptions were permitted. Windows uses three primary profiles:

- **Domain:** Applied when the system can authenticate to an organizational domain.
- **Private:** Intended for trusted, controlled networks.
- **Public:** Intended for untrusted networks and generally configured most restrictively.

Different interfaces can use different profiles simultaneously. Rules should therefore be scoped to the network context in which a service is genuinely required.

I also examined how an application can be added to the allowed-programs list and limited to selected network locations. I did not create an unnecessary application exception during this review.

## Configuring a Remote Desktop Exception

I first verified that a Remote Desktop connection between two Windows servers failed while the destination service was not permitted through the firewall. I then enabled the predefined Remote Desktop exception on the destination and restricted it to the private profile.

After enabling remote access, I repeated the test and confirmed that the connection succeeded. The before-and-after test showed that the firewall rule, rather than general network connectivity, controlled access to the service.

### Production Security Considerations

The lab used a legacy compatibility setting in an isolated environment. A production Remote Desktop deployment should instead use Network Level Authentication, trusted certificates, strong authentication, restricted management networks, source-address limitations, and centralized monitoring. Remote access should never be exposed more broadly than operationally necessary.

## Windows Firewall with Advanced Security

I verified that the Windows Firewall service was configured for automatic startup and then reviewed WFAS. Unlike the basic Control Panel interface, WFAS supports granular inbound and outbound rules based on programs, services, protocols, ports, addresses, and profiles.

The profile configuration reflected a common baseline:

- Block unsolicited inbound connections.
- Allow outbound connections unless a rule explicitly denies them.
- Apply different rules according to the active domain, private, or public profile.

## Configuring and Validating an ICMP Rule

I established a baseline by confirming that one Windows server could not initially receive an ICMP echo reply from another. On the destination server, I enabled the predefined inbound ICMPv4 echo-request rule for the selected profiles and repeated the test. The second test succeeded, confirming that the intended rule was active.

No separate rule was required on the initiating host for the associated echo reply because the stateful firewall recognized it as response traffic belonging to the established exchange.

Enabling ICMP on a public profile was limited to the isolated lab scenario. In production, diagnostic rules should be restricted to required profiles, trusted source networks, and documented operational needs.

## Configuring Linux Firewall Rules

On the Linux system, I inspected the active packet-filtering rules and established that an outbound Telnet connection was initially permitted. I then added an outbound rule that dropped TCP traffic destined for the Telnet service port and repeated the connection test. The connection timed out, validating that the rule was enforced.

The exercise demonstrated why simply enabling a firewall is not sufficient: effective security depends on an intentional policy and correctly scoped rules. It also reinforced that Telnet transmits data without modern cryptographic protection and should be replaced with a secure remote-administration protocol such as SSH.

## Validation Approach

Each material configuration change followed a simple verification cycle:

1. Test and record the initial behavior.
2. Apply one scoped firewall change.
3. Repeat the same test.
4. Confirm that the observed difference matches the intended policy.
5. Review the rule's profile, direction, protocol, and exposure.

This approach reduces ambiguity and produces evidence that a specific control caused the result.

## Security Considerations

- Follow default-deny principles for unsolicited inbound traffic.
- Permit only the services required for legitimate business functions.
- Scope rules by direction, protocol, port, profile, and trusted source where possible.
- Treat public networks as untrusted and apply the most restrictive appropriate policy.
- Review application exceptions regularly and remove obsolete rules.
- Prefer secure protocols over legacy plaintext services.
- Ensure host firewalls start automatically and cannot be casually disabled.
- Log firewall decisions and centralize events for monitoring and investigation.
- Document rule ownership, justification, testing, and expiration.
- Use host and network firewalls together as complementary controls.

## Security Concepts Demonstrated

- Defense in depth
- Host-based and network-based filtering
- Stateful firewall behavior
- Inbound and outbound traffic control
- Default-deny security posture
- Network profile awareness
- Application and service exceptions
- Protocol and port filtering
- Attack-surface reduction
- Least privilege and least exposure
- Secure remote administration
- Control validation and troubleshooting

## Results

- Enabled Windows host-based firewall protection.
- Reviewed domain, private, and public firewall profiles.
- Examined existing application exceptions and profile scope.
- Enabled a private-profile Remote Desktop exception and validated connectivity.
- Reviewed advanced inbound and outbound policy through WFAS.
- Enabled an inbound ICMPv4 rule and confirmed successful echo requests.
- Inspected the Linux firewall ruleset.
- Blocked outbound Telnet traffic and confirmed that the connection failed.
- Demonstrated the security impact of narrowly scoped firewall changes.

## Key Takeaways

- Perimeter firewalls do not eliminate the need for host-based protection.
- Firewall profiles allow policy to adapt to the trust level of each network connection.
- Every exception expands the attack surface and requires a clear justification.
- Stateful inspection permits legitimate response traffic without broadly opening inbound access.
- Advanced firewall interfaces provide the granularity needed for least-exposure policies.
- A firewall must have an intentional ruleset; an enabled firewall with permissive policy provides limited protection.
- Before-and-after testing is essential for proving that a firewall rule works as intended.
- Legacy plaintext protocols should be blocked or replaced with secure alternatives.

## Skills Demonstrated

- Windows Firewall administration
- Windows Firewall with Advanced Security
- Firewall profile and exception management
- Inbound and outbound rule configuration
- ICMP connectivity testing
- Remote Desktop access-control validation
- Linux packet-filtering administration
- Protocol and port-based traffic filtering
- Firewall troubleshooting and verification
- Defense-in-depth design
- Attack-surface reduction
- Security control documentation
