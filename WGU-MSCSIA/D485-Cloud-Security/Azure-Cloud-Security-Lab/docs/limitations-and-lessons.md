# Limitations, Recommendations, and Lessons Learned

## Demonstrated Limits

| Area | What the lab established | What remains unverified |
| --- | --- | --- |
| Resource organization | Specific misplaced resources were relocated | Network isolation and application authorization |
| Access | Scoped assignments and saved access policies | Comprehensive negative-access and cross-department denial testing |
| Vault design | List policies under the legacy permission model | Strict separation from vault managers who can edit those policies |
| Encryption | Vault organization and access configuration | Workload encryption integration, customer-managed keys, or application TLS |
| Backup | Saved schedule, retention, assignments, and failed attempts | Successful recovery points, restores, and achieved RPO/RTO |
| Network controls | Screenshots raised configuration questions | Comprehensive network review, exposure testing, or remediation |
| Compliance | A scenario-based discussion of responsibilities and controls | Certification, authorization, audit acceptance, or production compliance |

The exercise ran in a training environment. It was not a production deployment or a complete cloud-security assessment.

## Recommendations — Not Implemented

1. Resolve the inherited policy blocker through its authorized owner, then validate backups and timed recovery.
2. Replace the legacy vault permission model with Azure RBAC and separate management, permission administration, and data roles after planning caller migration.
3. Validate cross-department denial cases using approved test identities and documented expected results.
4. Review actual workload encryption and TLS settings. Consider customer-managed-key integration only when justified by requirements, compatibility, and recovery planning.
5. Review network exposure and controls. Absence of an NSG association in a screen does not prove public reachability or justify claiming a completed network fix.
6. Establish recurring access reviews, MFA and privileged-access governance, current asset inventory, vulnerability coverage, and centralized monitoring.
7. Evaluate backup soft delete, immutability, and separation of destructive privileges before any production design is accepted.

These recommendations were part of analysis and portfolio documentation. They were not additional Azure changes.

## Compliance Context

The fictional scenario identifies federal-contracting and payment-card concerns. Actual obligations would require confirmation of contracts, system boundaries, and the cardholder data environment. NIST SP 800-53 provides a control catalog rather than a certification. PCI DSS does not become satisfied by moving a workload to Azure.

Useful planning mappings include AC-2/AC-6 for account management and least privilege; CP-9/CP-10 for backup and recovery; and SC-8/SC-28 for transmission and stored-information protection. These mappings explain areas needing evidence, not completed control implementations or audit findings. See [references](references.md).

## Lessons Learned

- A resource group's name is not a security guarantee; placement and assignments need inspection.
- Moving resources changes administrative organization, not network topology or application permissions by itself.
- Control-plane authority can undermine apparently narrow data policies under the legacy vault model.
- Saved settings, positive assignment views, and operational success are different evidence types.
- A passed pre-check can precede a later failed backup.
- Inherited policy can block a managed service even when its configuration appears correct.
- Backup protection is not established until jobs succeed and restore behavior is tested.
- Good documentation preserves unsuccessful outcomes instead of converting them into implied success.

## Portfolio Preparation

The original documents and archive were inspected privately and were not copied into this repository. The known editing artifact in the later text was interpreted as “assignments”; no malformed citation text was retained. References were checked against official pages and rewritten using organization names and descriptive titles rather than contributor handles or unverified publication dates.

AI assisted drafting, organization, diagram preparation, evidence sanitization, and reference checking for this portfolio adaptation. The repository does not contain deployable infrastructure, commands presented as executed, fabricated tool output, or reconstructed Azure screenshots.
