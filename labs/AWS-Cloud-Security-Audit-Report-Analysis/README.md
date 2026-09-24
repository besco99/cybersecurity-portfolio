# AWS Cloud Security Audit Report Analysis

## Overview

This lab focused on analyzing security audit reports from an intentionally vulnerable Amazon Web Services (AWS) training environment. I reviewed Scout Suite and Prowler output to identify cloud misconfigurations, distinguish passed controls from warnings and failures, investigate affected resources, and translate technical findings into prioritized remediation work.

The exercise emphasized report interpretation rather than deploying the environment or running a live scan. This portfolio entry does not include account identifiers, user names, role names, resource identifiers, access-key details, exact finding counts, commands, or proprietary lesson instructions.

## Lab Environment

- Intentionally vulnerable AWS training environment
- Infrastructure defined through Terraform
- Scout Suite cloud security audit report
- Prowler security and compliance report
- AWS Identity and Access Management (IAM)
- Amazon Elastic Compute Cloud (EC2)
- Amazon Elastic Block Store (EBS)
- Amazon Simple Storage Service (S3)
- AWS logging and configuration services

## Objectives

- Interpret cloud security posture reports from multiple tools.
- Navigate findings from summary dashboards to affected resources.
- Analyze IAM, compute, storage, network, and logging risks.
- Distinguish informational, passed, warning, and failed checks.
- Prioritize findings by exposure, privilege, data sensitivity, and business impact.
- Develop actionable remediation and validation steps.
- Explain the value and limitations of automated cloud auditing.

## Authorization and Scope

The reports represented a purpose-built vulnerable cloud environment intended for security education. No production AWS accounts, real identities, customer information, or active credentials were used or published.

Cloud auditing should be performed only with authorization and a least-privilege assessment role. Reports may contain account metadata, resource names, trust relationships, network exposure, and credential information, so they must be stored and shared as sensitive security artifacts.

## Assessment Approach

The analysis followed a structured process:

1. Review the overall dashboard and service-level finding counts.
2. Separate passed controls from warnings and high-risk failures.
3. Open each finding to understand the rule and affected resources.
4. Validate whether the condition represents real exposure.
5. Identify the owner, data sensitivity, and operational purpose of each resource.
6. Prioritize remediation using likelihood, impact, and blast radius.
7. Define evidence and retest criteria for closure.

Automated severity was used as an initial signal, not as a substitute for contextual risk analysis.

## Scout Suite Report Analysis

Scout Suite presented cloud posture information through a service-oriented dashboard. The report summarized resources, rules, findings, and checks across AWS services and allowed results to be filtered by status.

The visual layout supported rapid triage, while expandable details and resource views provided the context needed to determine why a rule was flagged. I used this hierarchy to move from broad service-level risk to specific configuration conditions.

## Prowler Report Analysis

Prowler organized checks into a consolidated security and compliance report. Results were categorized as informational, passed, or failed, with supporting evidence for each evaluated control.

The report format was useful for systematic control review and remediation tracking. It also reinforced the importance of verifying that a passing check covered every applicable resource and that an informational event could still warrant investigation.

## Tool Comparison

| Area | Scout Suite | Prowler |
| --- | --- | --- |
| Primary strength | Interactive service and resource exploration | Control-oriented security and compliance checks |
| Presentation | Dashboard with drill-down views | Consolidated list or report of evaluated checks |
| Triage use | Visual identification of concentrated risk | Structured pass/fail review and tracking |
| Best combined value | Resource context | Control coverage and remediation evidence |

Using multiple tools can improve coverage, but duplicated findings should be consolidated into one remediation record.

## EC2 and Network Exposure

The compute review identified security-group configurations that permitted broader network access than necessary. Open ports and permissive source ranges increase the reachable attack surface of cloud workloads.

Recommended controls include:

- Restrict inbound rules to approved sources, ports, and protocols.
- Remove unused security-group rules.
- Keep default security groups empty where practical.
- Use private subnets for systems that do not require direct internet access.
- Prefer controlled administrative paths over exposing management services.
- Review outbound access and service-to-service flows.
- Continuously monitor for newly introduced public exposure.

## EBS Encryption and Snapshot Security

The report highlighted storage resources and snapshots that required encryption or exposure review. EBS volumes and snapshots may contain operating-system data, credentials, application files, and sensitive business information.

Defensive measures include:

- Enable encryption by default for newly created EBS volumes.
- Use approved AWS Key Management Service keys and access policies.
- Encrypt copies when migrating legacy unencrypted snapshots or volumes.
- Prevent snapshots from being shared publicly.
- Restrict cross-account sharing to documented recipients.
- Monitor changes to snapshot permissions and encryption state.
- Test restoration procedures after remediation.

Encryption reduces data-exposure risk but does not replace access control or secure key management.

## IAM Findings

IAM findings represented a major concentration of risk because identity permissions determine which actions can be performed throughout the cloud environment. The review included:

- Missing multifactor authentication
- Inactive or stale credentials
- Access-key lifecycle concerns
- Root-account activity
- Cross-account role trust conditions
- Overly broad policies
- Role trust relationships
- Unused users, roles, and permissions

The report also demonstrated how credential reports can help identify accounts with console access, MFA status, key status, and recent use without exposing secret key material.

## IAM Remediation

- Require MFA for human identities, especially privileged users.
- Avoid routine use of the root account and protect it with strong controls.
- Disable and remove credentials that are no longer required.
- Rotate credentials according to policy and when exposure is suspected.
- Prefer temporary credentials and role assumption over long-lived access keys.
- Apply least privilege and remove wildcard permissions where possible.
- Review cross-account trust and require appropriate external or identity conditions.
- Use separate roles for administration, workloads, and auditing.
- Regularly analyze unused permissions and right-size policies.
- Monitor changes to trust policies, permissions, and credential state.

## S3 Access-Control Findings

The storage review identified bucket policies or access controls that granted actions too broadly. Public or broadly shared buckets can expose data, permit unauthorized uploads, or enable destructive changes.

Recommended controls include:

- Enable S3 Block Public Access at appropriate account and bucket levels.
- Remove public grants and overly broad principals.
- Apply least-privilege bucket policies.
- Restrict access using identity, organization, network, and resource conditions.
- Enable default encryption and protect key permissions.
- Use versioning and recovery controls for important data.
- Review access points, ACLs, and cross-account permissions.
- Log data access where justified by risk and privacy requirements.

## Logging and Configuration Visibility

Cloud posture assessment depends on reliable evidence. Important visibility controls include:

- Organization-wide API activity logging
- Centralized, protected log storage
- Configuration history and change tracking
- Identity and access events
- Network-flow visibility where appropriate
- Storage access logging for sensitive resources
- Security-service alerts and anomaly detection
- Time synchronization, retention, and log-integrity controls

Monitoring should alert on public-access changes, disabled logging, policy modifications, new credentials, anomalous role assumption, and root-account use.

## Risk Prioritization

Findings were prioritized using more than the scanner's displayed severity. Relevant factors included:

- Public reachability
- Data classification
- Identity privilege
- Cross-account trust
- Credential age and activity
- Exploitability and required access
- Number of affected resources
- Existing detective and preventive controls
- Business criticality
- Potential blast radius

A publicly reachable service with broad identity permissions and unencrypted sensitive data would take priority over an isolated low-impact resource, even if both appeared in the same tool category.

## False Positives and Context

Not every failed check is an exploitable vulnerability, and not every passed check proves that a resource is secure. Validation questions include:

- Is the resource active and business-required?
- Is it reachable from an untrusted network?
- Does another control limit the exposure?
- Is the policy inherited or managed elsewhere?
- Does the check reflect the current architecture and risk policy?
- Is an exception documented, approved, and time-limited?
- Did the assessment role have sufficient visibility to evaluate the resource?

Incomplete permissions can cause blind spots, so audit-role coverage must also be validated.

## Remediation Workflow

1. Preserve the original report and assessment metadata.
2. Confirm the finding against the current cloud configuration.
3. Identify the resource owner and business purpose.
4. Evaluate dependencies and change risk.
5. Apply the smallest effective configuration change.
6. Test functionality and monitor for unintended impact.
7. Rerun the relevant check.
8. Record evidence, residual risk, and closure approval.

Infrastructure as code should be updated alongside the live environment so insecure settings are not redeployed.

## Infrastructure-as-Code Validation

The training environment demonstrated how Terraform can create repeatable cloud configurations. The same approach supports defensive validation:

- Define secure defaults in reusable modules.
- Scan infrastructure code before deployment.
- Require peer review for IAM, networking, storage, and logging changes.
- Apply policy-as-code checks in continuous integration.
- Detect drift between approved code and deployed resources.
- Recreate test conditions safely and verify remediations consistently.

## Cost and Resource Governance

Cloud lab environments can generate ongoing charges and create exposure if left running. Safe lab governance includes:

- Establishing a budget and billing alerts
- Limiting permitted regions and services
- Tagging resources with owner and expiration information
- Using isolated accounts with service-control guardrails
- Scheduling or automating teardown
- Verifying that resources, snapshots, addresses, and logging destinations are removed when no longer needed

Cost controls are part of secure cloud operations because abandoned resources often become unmonitored assets.

## Security Concepts Demonstrated

- Cloud security posture management
- AWS shared-responsibility model
- IAM least privilege
- Multifactor authentication
- Credential lifecycle management
- Cross-account trust
- Security-group hardening
- Storage encryption
- Public-access prevention
- Cloud logging and monitoring
- Infrastructure as code
- Risk-based remediation

## Results

- Analyzed Scout Suite and Prowler reports from a vulnerable AWS training environment.
- Navigated from summary findings to resource-level context.
- Evaluated network exposure, storage encryption, IAM, and S3 access controls.
- Distinguished passed, informational, warning, and failed checks.
- Prioritized findings using exposure, privilege, data sensitivity, and blast radius.
- Developed remediation and retest guidance.
- Identified infrastructure-as-code and cost-governance considerations.

## Key Takeaways

- Cloud audit tools provide broad visibility, but findings require contextual validation.
- Identity misconfigurations can create a larger blast radius than an isolated host weakness.
- Public access, broad permissions, and unencrypted data are especially dangerous when combined.
- Passed controls should be verified for coverage rather than accepted blindly.
- Remediation is incomplete until infrastructure code is corrected and the control is retested.
- Audit reports contain sensitive architectural information and must be protected.
- Repeatable labs, automated teardown, and budget controls support safe cloud-security learning.

## Skills Demonstrated

- AWS security posture analysis
- Scout Suite report interpretation
- Prowler report interpretation
- IAM and credential-lifecycle review
- EC2 security-group assessment
- EBS encryption and snapshot analysis
- S3 permissions review
- Cloud finding validation and prioritization
- Infrastructure-as-code security analysis
- Remediation and retest planning
- Cloud governance and cost-awareness
- Professional cybersecurity documentation
