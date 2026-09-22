# Process Injection and Migration Analysis

## Overview

This authorized lab explored how code can execute within the context of another process and how process migration is used during post-exploitation activity. I reviewed several process-injection concepts, enumerated processes on a deliberately vulnerable Windows target, evaluated execution contexts, observed a migration failure caused by access controls, and then validated a successful migration into a compatible long-running process.

The portfolio entry focuses on defensive understanding and detection. Exploit identifiers, payload settings, network targets, credentials, and reusable attack procedures are intentionally excluded.

## Lab Environment

- Isolated virtual network
- Kali Linux security-testing system
- Deliberately vulnerable Windows target
- Metasploit Framework
- Controlled Meterpreter session
- Windows process and security-context data

## Objectives

- Explain common library and process-injection techniques.
- Understand why malicious code may execute within a legitimate process.
- Enumerate processes and compare ownership, paths, and privilege contexts.
- Assess the stability and security implications of an active session's host process.
- Observe how Windows access controls can prevent process migration.
- Validate process-context changes after an authorized migration.
- Identify endpoint telemetry associated with injection and migration behavior.
- Recommend controls that reduce injection and post-exploitation risk.

## Authorization and Scope

All activity was performed against a controlled lab target after an authorized foothold had already been established. No third-party systems, production endpoints, real accounts, or personal data were involved.

Process injection and migration testing can destabilize applications or expose sensitive memory. It should be performed only with written authorization, a defined scope, monitoring, recovery procedures, and appropriate data-handling controls.

## Injection Techniques Reviewed

### DLL Injection

DLL injection causes a dynamic-link library to be loaded into another process. The injected code then operates within that process's address space and security context. Defenders may observe unusual module loads, cross-process memory access, or a module originating from an unexpected path.

### Process Hollowing

Process hollowing begins with a legitimate process whose expected code is replaced in memory before execution continues. The process image may appear legitimate while its in-memory behavior differs from the file on disk. Detection can involve comparing memory mappings, image metadata, thread start addresses, and expected process behavior.

### Reflective DLL Injection

Reflective loading maps a library directly from memory rather than relying on the normal disk-backed loading workflow. This can reduce traditional file indicators, making memory telemetry, behavioral monitoring, and endpoint detection especially important.

### Remote-Thread Injection

Remote-thread injection creates execution inside an existing process after memory or code has been introduced into its address space. Cross-process handle access, memory allocation, memory writes, protection changes, and thread creation can form a useful detection sequence.

## Process Enumeration

I enumerated the Windows process list and reviewed several attributes relevant to post-exploitation analysis:

- Process name and executable path
- Process identifier and parent relationship
- Architecture and session
- Owning user or built-in service identity
- Privilege and integrity context
- Expected process lifetime and stability

This information helps explain what a compromised session can access and whether its host process is likely to terminate, restart, or receive enhanced monitoring.

## Evaluating the Initial Execution Context

I identified the process hosting the active session and verified that it ran as a privileged Windows service. Although this provided substantial local access, a service-related process may still be unsuitable for maintaining a stable session because it can restart, terminate, or be closely monitored.

This reinforced that initial access and durable access are separate concerns. Defenders should monitor not only the process involved in exploitation but also any subsequent movement into other processes.

## Testing Security Boundaries

The first controlled migration attempt was blocked with an access-denied result. This demonstrated that an active foothold does not guarantee unrestricted access to every process. Windows applies boundaries based on tokens, integrity levels, process protections, architecture, session context, and access-control decisions.

Failed injection or migration attempts are valuable detection opportunities. Repeated handle requests, denied process access, and unusual cross-process operations may reveal malicious intent even when execution is prevented.

## Validating Process Migration

A later migration into a compatible long-running process succeeded. I verified that the session's host process changed and evaluated why the selected context was more stable than the original.

From a defensive perspective, the important lesson is not which process was selected, but how the behavior appears in telemetry: one process accesses another, modifies memory or execution state, and causes activity that does not match the target process's normal role.

## Defensive Indicators

Potential indicators of process injection or migration include:

- One process opening another with unusual access rights
- Cross-process memory allocation or writes
- Executable memory created in a process that does not normally generate code
- Memory protection changes from writable to executable
- Threads beginning at addresses outside expected loaded modules
- Unsigned or unexpected modules loaded into trusted processes
- Mismatch between the on-disk image and in-memory execution
- Network connections originating from processes that do not normally communicate externally
- A process behaving inconsistently with its name, path, or parent
- Repeated access-denied events followed by a successful cross-process operation
- Endpoint alerts for injection, token manipulation, or suspicious memory access

These events are strongest when correlated as a sequence rather than treated as isolated findings.

## Recommended Mitigations

### Endpoint Hardening

- Maintain supported operating systems and current security updates.
- Enable exploit-protection features and attack-surface-reduction rules.
- Use application control to restrict untrusted code and libraries.
- Apply least privilege to users and services.
- Reduce unnecessary administrative sessions on endpoints.
- Disable unused services and legacy protocols.

### Detection and Monitoring

- Deploy endpoint detection and response tooling with memory and behavioral visibility.
- Monitor process-access events, remote-thread creation, and suspicious memory operations.
- Alert on unexpected modules, anomalous thread start locations, and executable private memory.
- Baseline normal process ancestry, network behavior, and module loading.
- Correlate endpoint events with authentication, firewall, and network telemetry.
- Preserve volatile evidence when injection is suspected.

### Network Controls

- Restrict outbound connections from system services and sensitive endpoints.
- Segment administrative and user networks.
- Limit management protocols to trusted systems.
- Inspect unusual encrypted or long-lived outbound sessions where policy permits.

## Incident Response Considerations

When process injection is suspected, responders should:

1. Isolate the endpoint while preserving evidence.
2. Capture process, network, and volatile-memory information when authorized.
3. Review the suspected source and destination processes.
4. Identify suspicious handles, memory regions, threads, and modules.
5. Determine the initial access path and current privilege context.
6. Hunt for credential access, persistence, and lateral movement.
7. Remediate the exploited weakness or rebuild the system.
8. Reset potentially exposed credentials from a trusted endpoint.
9. Validate monitoring coverage before restoring service.

## Security Concepts Demonstrated

- DLL injection
- Process hollowing
- Reflective loading
- Remote-thread injection
- Process migration
- Windows access tokens and security contexts
- Process stability and session continuity
- Cross-process memory behavior
- Endpoint detection and response
- Behavioral detection
- Least privilege
- Incident response
- Authorized penetration-testing scope

## Results

- Reviewed four common process-injection concepts.
- Enumerated processes and compared ownership, paths, and execution contexts.
- Identified the process hosting the controlled session.
- Evaluated the tradeoffs between privilege, stability, and detectability.
- Observed an access-control failure during a migration attempt.
- Completed and verified a migration into a compatible process.
- Identified telemetry that can reveal cross-process manipulation.
- Developed endpoint, network, and incident-response recommendations.

## Key Takeaways

- Trusted process names do not guarantee trustworthy in-memory behavior.
- Process injection can reduce obvious file indicators but creates behavioral evidence.
- A foothold does not automatically bypass every Windows process boundary.
- Failed migration attempts can be as valuable to defenders as successful ones.
- Detection should correlate process access, memory modification, thread creation, and network behavior.
- Supported systems, least privilege, exploit protection, and EDR materially reduce risk.
- Volatile-memory evidence is often critical when malicious code never touches disk.

## Skills Demonstrated

- Process-injection concepts
- Windows process enumeration
- Process ownership and privilege analysis
- Security-context assessment
- Process-migration validation
- Endpoint indicator identification
- Behavioral detection strategy
- Exploit mitigation planning
- Incident-response analysis
- Ethical penetration-testing practices
- Professional cybersecurity documentation
