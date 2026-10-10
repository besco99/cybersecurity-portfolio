# Security Awareness and Credential-Harvesting Simulation

## Overview

This authorized virtual-lab exercise examined how a simulated credential-harvesting page can support security-awareness training. Using the Social-Engineer Toolkit (SET) in an isolated environment, I observed how a fraudulent sign-in experience could collect test input and redirect a user to a legitimate service, making the deception less obvious.

The exercise was analyzed from a defensive and program-management perspective. It demonstrates why organizations need recurring awareness training, privacy safeguards, operational security, technical controls, and clear reporting procedures.

This write-up does not include the sample username or password, lab address, cloned service, menu sequence, commands, captured values, or proprietary course instructions.

## Lab Environment

- Kali Linux virtual machine
- Social-Engineer Toolkit
- Local web service
- Isolated virtual network
- Browser-based simulated user interaction
- Fictional test credentials

## Objectives

- Explain how social engineering targets human decision-making.
- Observe the behavior of a credential-harvesting simulation.
- Recognize signs of fraudulent sign-in pages and suspicious links.
- Relate phishing simulations to operational security and privacy.
- Identify preventive and detective controls across email, browser, identity, endpoint, and network layers.
- Design ethical and measurable security-awareness exercises.
- Develop an appropriate response to suspected credential submission.

## Authorization and Ethical Scope

The activity occurred only in a purpose-built virtual lab using fictional data. No real account, public website, production identity system, employee, or third-party organization was targeted.

Credential-harvesting simulations require explicit authorization because they can collect sensitive information and affect employee trust. A real program should define:

- Executive and legal approval
- Approved participants and organizational scope
- Prohibited pretexts and protected groups
- Test infrastructure and delivery channels
- Data-collection limits
- Privacy and retention requirements
- Emergency stop procedures
- Incident-response coordination
- Participant support and debriefing
- Metrics and reporting audiences

Real passwords should never be collected as part of an awareness exercise.

## Security Program Management

Awareness and training are ongoing security-program functions rather than one-time compliance events. Effective programs combine:

- Training during onboarding
- Periodic refresher education
- Role-based instruction
- Safe phishing simulations
- Just-in-time coaching
- Accessible reporting mechanisms
- Measurement and continuous improvement

Training should help personnel make better decisions without creating a culture of blame.

## Core Concepts

### Operational Security

Operational security identifies information that could help an adversary and controls how that information is handled. Employees contribute by protecting credentials, validating unusual requests, limiting oversharing, and reporting suspicious activity.

### Physical Security

Physical security protects people, facilities, systems, and media. Awareness topics include tailgating, unattended devices, visitor procedures, badge protection, secure disposal, and reporting unfamiliar individuals in controlled areas.

### Privacy

Privacy governs the appropriate collection, use, retention, and disclosure of personal information. A simulation must minimize data collection and restrict access to results. Capturing genuine credentials would create unnecessary risk and may violate policy or law.

### Social Engineering

Social engineering manipulates trust, urgency, authority, fear, curiosity, or helpfulness to influence behavior. Technical controls are important, but attackers often design messages specifically to persuade users to bypass them.

### Phishing

Phishing uses deceptive communications or websites to induce actions such as opening a file, following a link, authorizing a request, or disclosing information. Variants include targeted spear phishing, voice phishing, text-message phishing, and business-email compromise.

### Situational Awareness

Situational awareness means recognizing the context of a request, noticing inconsistencies, understanding potential consequences, and knowing how to verify or report the event.

## Simulation Summary

The lab demonstrated this controlled sequence:

1. SET was launched in the isolated assessment environment.
2. A lab-only web simulation was prepared.
3. A browser visited the local training page.
4. Fictional credentials were entered.
5. The page redirected the simulated user to a legitimate destination.
6. The test input appeared in the assessment console.
7. The result was used to identify training and control improvements.

Operational reproduction details are intentionally omitted because the portfolio's purpose is to demonstrate analysis, not enable credential theft.

## Why the Technique Can Be Convincing

A fraudulent sign-in page may imitate familiar branding, layout, colors, and prompts. Redirection after submission can reduce suspicion because the user ultimately sees a legitimate page and may assume the initial failure was harmless.

Common warning signs include:

- An unexpected sign-in request
- A mismatched or unfamiliar domain
- Direct-IP or shortened links
- Missing or unusual password-manager behavior
- Browser certificate warnings
- Inconsistent page wording or design
- Urgent instructions to act immediately
- A login prompt reached from an unsolicited message
- Repeated authentication after a supposed successful sign-in

Visual similarity alone does not prove a page is legitimate.

## Password Managers as a Defensive Signal

Password managers associate stored credentials with expected origins. If credentials do not autofill on a familiar-looking page, the user should stop and inspect the domain rather than manually entering a password.

Password managers are not a complete defense, but they reduce reliance on visual recognition and can help resist look-alike sites.

## Identity Security Controls

Recommended identity safeguards include:

- Phishing-resistant multifactor authentication
- Passkeys or hardware-backed security keys
- Conditional access
- Risk-based sign-in detection
- Session and token protection
- Rapid credential revocation
- Monitoring for impossible travel or abnormal access
- Least privilege
- Separate administrator accounts

SMS codes and push approvals can still be captured or manipulated in some attacks. Phishing-resistant authentication provides stronger protection.

## Email and Messaging Controls

- Enforce SPF, DKIM, and DMARC.
- Inspect links at delivery and click time.
- Detect impersonation and look-alike domains.
- Mark messages originating outside the organization.
- Quarantine high-risk content.
- Restrict automatic forwarding.
- Provide an easy report-phishing button.
- Correlate reports from multiple recipients.

Legitimate authentication messages should use predictable, documented channels.

## Browser and Web Controls

- Use DNS and web reputation filtering.
- Block known credential-phishing infrastructure.
- Restrict newly registered or uncategorized destinations where appropriate.
- Enforce certificate validation.
- Keep browsers current.
- Use isolation for high-risk browsing.
- Warn on look-alike domains.
- Prevent users from bypassing important security warnings without justification.

## Endpoint and Network Detection

Defenders can correlate:

- Browser history and navigation events
- DNS queries
- Proxy and firewall logs
- TLS certificate information
- Endpoint process telemetry
- Identity-provider sign-in events
- Email message and click records
- Newly observed destinations
- Connections immediately preceding suspicious authentication

No single signal is definitive. Correlation establishes sequence, affected users, and likely impact.

## Response to Suspected Credential Submission

If a user enters credentials into a suspected phishing page:

1. Report the event immediately through the approved channel.
2. Record the time, message, URL, and affected account.
3. Reset the exposed password from a trusted device.
4. Revoke active sessions and refresh tokens.
5. Review multifactor authentication methods and recent approvals.
6. Search identity logs for abnormal access.
7. Check for mailbox rules, forwarding, or account changes.
8. Identify other recipients and affected endpoints.
9. Block confirmed indicators across relevant controls.
10. Preserve evidence and document remediation.

If the password was reused, every affected account requires remediation.

## Awareness Program Design

Training should be relevant to employee roles and the threats they encounter. Useful topics include:

- Verifying sign-in page origins
- Recognizing unusual requests
- Protecting passwords and MFA prompts
- Reporting suspicious messages
- Handling sensitive information
- Physical access and tailgating
- Remote-work privacy
- Removable media
- Help-desk impersonation
- Business-email compromise

Short, recurring lessons are often more effective than a single annual presentation.

## Ethical Simulation Principles

An ethical exercise should:

- Use inert or purpose-built training pages.
- Avoid collecting genuine secrets.
- Minimize personal data.
- Avoid humiliating participants.
- Exclude traumatic or discriminatory pretexts.
- Provide prompt education after interaction.
- Protect individual results.
- Measure organizational resilience rather than blame.
- Remove infrastructure and test data after completion.

The goal is behavior improvement and control validation.

## Metrics

Useful measurements include:

- Message delivery and filtering rate
- Interaction rate
- Reporting rate
- Median time to report
- Security-operations triage time
- Containment time
- Repeat trends by scenario type
- Control failures discovered
- Completion of follow-up education

Raw click rates should not be treated as the sole measure of program success.

## Cleanup and Data Handling

After the simulation:

- Stop the local web service and assessment tool.
- Remove temporary pages and logs that are no longer needed.
- Confirm no real credentials were stored.
- Restrict retained evidence to authorized personnel.
- Apply the approved retention schedule.
- Revert or destroy temporary lab systems where appropriate.
- Document lessons learned and control improvements.

## Security Concepts Demonstrated

- Security-awareness program management
- Social-engineering risk
- Credential-harvesting indicators
- Operational security
- Physical security awareness
- Privacy and data minimization
- Phishing-resistant authentication
- Layered detection
- Incident response
- Ethical simulation design

## Results

- Reviewed SET as an authorized awareness-training tool.
- Observed a credential-harvesting simulation in an isolated network.
- Confirmed that a deceptive page can capture fictional input and redirect the browser.
- Identified visual, domain, browser, and behavioral warning signs.
- Mapped preventive and detective controls across identity, email, web, endpoint, and network layers.
- Developed a response workflow for suspected credential exposure.
- Defined ethical safeguards, metrics, cleanup, and privacy requirements.

## Key Takeaways

- Familiar branding does not establish website authenticity.
- Redirection to a legitimate page can conceal the initial deception.
- Password managers can provide a useful origin-validation signal.
- Phishing-resistant MFA reduces the value of captured passwords.
- Users need simple, trusted ways to report suspicious activity quickly.
- Simulations should never require the collection of real passwords.
- Awareness, technical controls, monitoring, and incident response work best together.
- Training should improve systems and behavior without shaming individuals.

## Skills Demonstrated

- Security-awareness analysis
- Social-Engineer Toolkit concepts
- Credential-phishing risk assessment
- Operational security and privacy evaluation
- Phishing indicator identification
- Identity and email control mapping
- Detection and log-correlation planning
- Credential-compromise response
- Ethical simulation design
- Security-program metrics
- Professional cybersecurity documentation
