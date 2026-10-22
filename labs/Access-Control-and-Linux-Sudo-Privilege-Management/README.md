# Access Control and Linux Sudo Privilege Management

## Overview

This student learning case study reviews access-control concepts and a narrated Linux demonstration of command-specific privilege delegation. It connects single sign-on (SSO), Kerberos authentication, and privileged access management (PAM) with a practical example using sudoers.

This is a review of supplied instructional material, not an independently executed deployment or security assessment. Reported behavior below comes from the demonstration; no original terminal output or configuration file was supplied.

## Environment and Tools Reviewed

- Kali Linux virtual machine.
- An existing administrator and a separate standard user, described here with generic account labels.
- `sudo` and the sudoers authorization policy.
- APT package management and the `htop` process viewer.

## Objectives

- Distinguish identity verification from permission enforcement.
- Explain how SSO and Kerberos relate to authentication.
- Analyze selective delegation of administrative commands.
- Identify the limits of a small positive-and-negative access test.

## Authentication and Authorization

Authentication establishes who a user is; authorization determines which actions that identity may perform. SSO reduces repeated sign-ins across participating services, but each application still needs authorization controls. Centralized sign-in can simplify policy enforcement while also concentrating risk if an account or session is compromised.

Kerberos uses a key distribution center (KDC), ticket-granting tickets, and service tickets. The client obtains a ticket-granting ticket through initial authentication, then requests tickets for individual services. A service validates the presented authentication material; resource permissions remain a separate decision. This ticket flow is described in [MIT's ticket management documentation](https://web.mit.edu/kerberos/krb5-latest/doc/user/tkt_mgmt.html).

Kerberos does not send the user's plaintext password to each service. The protocol should not be reduced to a claim that the entire initial request is encrypted with the password. Ticket protection, authenticators, preauthentication, and key handling have distinct roles. Limited ticket lifetimes also do not make stolen tickets harmless.

SSO and Kerberos were discussed conceptually in the source; neither was configured or tested in the practical demonstration.

## Demonstration Summary

1. **Establish an administrative baseline:** The administrator refreshes package metadata, installs and launches `htop`, and removes it.
2. **Create a standard account:** A separate user is created without the administrator's sudo permissions.
3. **Test the initial restrictions:** Attempts to refresh package metadata or install the package using sudo are reported as denied.
4. **Delegate selected tasks:** The administrator enables a sudoers entry intended to permit metadata refresh and installation of the named package, without permitting its removal.
5. **Retest allowed operations:** The standard user is reported to perform those two delegated tasks and launch the installed application.
6. **Test an excluded operation:** The attempt to remove the package using sudo is reported as denied.

The exact sudoers entry is not included in the supplied text. This case study therefore does not reconstruct it as a tested configuration or claim that its matching rules were independently verified.

## Reported Access Decisions

| Operation by the standard user | Before delegation | After delegation |
| --- | --- | --- |
| Refresh package metadata through sudo | Denied | Allowed |
| Install the specified package through sudo | Denied | Allowed |
| Remove the specified package through sudo | Not separately evidenced | Denied |

These are the demonstration's reported outcomes, not results from a new test run.

## Security Analysis

The example illustrates separating routine user access from selected elevated actions. It does not establish a complete least-privilege boundary. Package management executes highly privileged operations, including package-maintenance scripts. Actual safety depends on the precise command and argument rules, trusted repositories, configuration permissions, environment handling, and any additional privileges available to the account.

The [sudoers manual](https://www.sudo.ws/docs/man/1.9.14/sudoers.man.pdf) explains command and argument matching, including wildcard behavior. Allowing an executable without carefully bounding its arguments can authorize more behavior than intended. The reported denial of one removal attempt is useful evidence of that particular check, not proof that every unintended privileged action is blocked.

A sudo policy is one local privilege-control mechanism. It does not, by itself, demonstrate approval workflows, time-limited activation, credential rotation, session recording, or centralized monitoring. Those broader PAM capabilities require separate implementation. In this document PAM means **privileged access management**, not Linux Pluggable Authentication Modules.

## Recommended Validation and Safeguards

The following are recommendations, not completed work:

- Review the effective permissions and all relevant group-based grants, rather than inspecting one rule in isolation.
- Use `visudo` for checked policy editing and validation, with a recovery path for administrative access.
- Constrain command paths, arguments, target identities, and writable dependencies; avoid broad grants that defeat the intended restriction.
- Evaluate whether delegating package installation is necessary or whether a controlled deployment process is safer.
- Test allowed and denied operations in an isolated environment and review the resulting audit records.
- For wider privileged-access programs, consider approval, expiry, strong authentication, monitoring, and periodic access reviews.

## Results and Limitations

The demonstration provides a clear before-and-after illustration of selective sudo authorization and an explicit denial test. The central learning outcome is that elevated access can be delegated for particular tasks rather than granting an unrestricted administrator role.

No configuration artifact, screenshots, logs, or independent execution evidence accompany the transcript. The review does not confirm resistance to privilege escalation, comprehensive negative-access coverage, log collection, SSO integration, Kerberos configuration, or production readiness. No production system was changed and no remediation outcome is claimed.

## Key Takeaways

- Successful sign-in does not imply permission to perform administrative work.
- Positive and negative tests are both useful, but their conclusions must remain bounded by what was tested.
- Command-specific sudo delegation needs careful review of arguments and indirect privileged behavior.
- Local sudo controls are one part of privileged-access management, not a substitute for its full lifecycle.

## Skills and Concepts Reviewed

- Authentication and authorization analysis.
- Linux account separation and sudoers policy interpretation.
- Least-privilege design and access-test reasoning.
- SSO, Kerberos, and privileged-access management fundamentals.
- Evidence-aware documentation and security limitation reporting.

## Sources

- User-supplied access-control lesson transcript, summarized in original language without account-specific details.
- [Sudo project: sudoers manual](https://www.sudo.ws/docs/man/1.9.14/sudoers.man.pdf) — policy matching and delegation behavior; configuration must be checked against the installed version.
- [MIT Kerberos: Ticket management](https://web.mit.edu/kerberos/krb5-latest/doc/user/tkt_mgmt.html) — ticket use and lifecycle.
