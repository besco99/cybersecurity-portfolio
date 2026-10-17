# SELinux Security Contexts and Access Control

## Overview

This learning exercise examined operating-system security through Security-Enhanced Linux (SELinux). The supplied demonstration used a specially configured Kali Linux VM to inspect SELinux status, examine a test file's security context, change its type label, and inspect the result.

This entry documents the observed label change and reviews how mandatory access control can constrain services. It does not claim an independently executed test or a confirmed access denial. Proprietary instructions and course-specific file paths are omitted.

## Environment and Tools

- Kali Linux VM described in the lesson as configured with SELinux enforcing
- Disposable test directory and text file
- `sestatus` for status inspection
- `ls -Z` for context inspection
- `chcon` for a direct label change
- Policy-management and audit tools reviewed conceptually

The lesson's prepared configuration should not be assumed to match a default Kali installation. Inspect the actual host's security framework and policy before applying distribution-specific procedures.

## Objectives

- Explain mandatory access control and its relationship to traditional permissions.
- Interpret SELinux context fields.
- Understand process domains, file types, and policy decisions.
- Distinguish enforcing, permissive, and disabled modes.
- Evaluate direct and persistent labeling methods.
- Troubleshoot denials without unnecessarily expanding access.

## Mandatory and Discretionary Access Control

Traditional Linux ownership, permission bits, and ACLs implement discretionary access control. SELinux adds policy checks for mediated operations; it does not replace those permissions. An operation must satisfy all applicable controls.

Type enforcement can limit a compromised service's access to other resources. Its effectiveness depends on the loaded policy, service confinement, configuration, and attack path. SELinux does not guarantee that exploitation is impossible.

## Security Contexts

A context is commonly displayed as:

```text
SELinux-user:role:type:level
```

| Field | Interpretation |
| --- | --- |
| SELinux user | Policy identity, distinct from the ordinary Linux username |
| Role | Permitted role relationships and process-domain authorization |
| Type | File or object type; for processes, commonly called the domain |
| Level | Sensitivity and optional categories used by MLS or MCS policy |

The type field is central to many targeted-policy decisions. A label is interpreted through policy rather than acting as a standalone permission rule. A file's SELinux user field alone does not establish whether the accessing process is confined.

## Modes

- **Enforcing:** Applies policy denials to operations subject to enforcement.
- **Permissive:** Records relevant would-be denials while allowing them at the SELinux layer; ordinary permissions still apply.
- **Disabled:** SELinux enforcement is inactive.

Some configurations also permit individual domains to operate permissively. Status, loaded policy, process context, and audit evidence should be evaluated together.

## Demonstrated Workflow

The demonstration showed:

1. Inspecting SELinux status and mode.
2. Preparing a disposable file.
3. Displaying its existing security context.
4. Changing the file type to a web-content type.
5. Inspecting the context again to confirm the type changed.

The transcript predicted that a terminal read would be blocked, but supplied no executed read test or audit denial. That prediction is not recorded here as a verified outcome.

## Interpreting Web-Content Labels

Under common targeted policies, `httpd_sys_content_t` identifies content that a confined HTTP service may read. It does not mean only HTTP processes can read the file. Other domains, including broadly permitted unconfined domains, may also have access.

The executable name alone does not determine the outcome. A read by `cat`, for example, must be considered in the context of the executing process's domain, the file type, requested permission, ordinary permissions, and policy.

## Direct and Persistent Labeling

`chcon` directly changes a label, but that change may be replaced by relabeling or `restorecon`. For a persistent path-based assignment, administrators generally define an appropriate file-context mapping with `semanage fcontext`, then apply it with `restorecon`.

Changes should use the narrowest appropriate path and type, account for parent-directory traversal, and be tested against the service's required behavior. Record the previous mapping and label for rollback.

## Booleans and Policy Management

Policy booleans expose predefined choices for supported behavior. Review what a boolean permits before changing it; broader service connectivity or filesystem access may affect more than one application.

`semanage` manages policy configuration such as file-context mappings, ports, and identity mappings. It is not a universal replacement for authoring policy rules.

## Investigating Denials

A disciplined investigation should:

1. Reproduce the intended action safely.
2. Confirm ordinary permissions and application configuration.
3. Inspect process and target contexts.
4. Check expected path labels and relevant booleans.
5. Review Access Vector Cache audit records.
6. Identify the source domain, target type, object class, and denied permission.
7. Correct configuration or labeling where appropriate.
8. Retest the required action and an action that should remain blocked.

Do not disable enforcement as the routine response to an application failure.

## Custom Policy Review

`audit2allow` can translate recorded denials into candidate allow rules. A denial may reflect an attack, a mislabeled file, or an incorrect application configuration, so automatically installing generated rules can authorize unwanted behavior.

Investigate the cause first. If custom policy is justified, review its scope, test it in a controlled environment, version it, and define rollback.

## Proposed Validation

A follow-up lab would use a known confined service domain and synthetic files to test both permitted and prohibited operations. It would capture process contexts, labels, mode, policy version, command results, and audit records.

Validation would also check that intended labels survive `restorecon` and that the correction restores only the required application behavior. No such independent validation evidence was supplied with this lesson.

## Results and Limitations

The supplied demonstration showed status inspection and a file-type label change. It illustrated context structure and policy concepts.

It did not demonstrate an actual denied read, a custom policy installation, persistent labeling across relabeling, or containment of a compromised service. Those remain proposed follow-up tests.

## Key Takeaways

- SELinux adds policy enforcement alongside ordinary Linux permissions.
- Process domains and object types must be interpreted together.
- Relabeling a file does not by itself prove an access restriction.
- Direct label changes may not survive relabeling.
- Audit records support evidence-based troubleshooting.
- Generated allow rules require review before installation.

## Skills and Concepts Reviewed

- Linux operating-system hardening
- Mandatory access control
- Security-context interpretation
- Type enforcement and service confinement
- SELinux mode inspection
- Persistent file labeling
- Audit-denial investigation
- Least-privilege policy review

## Technical References

The policy interpretation, labeling persistence, and troubleshooting guidance were checked against Red Hat's documentation:

- [Using SELinux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html-single/using_selinux/index)
- [SELinux User's and Administrator's Guide](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html-single/selinux_users_and_administrators_guide/index)
