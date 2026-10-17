# Sources and Technical References

## Lab Sources

This case study was adapted from my completed D485 report, its later edited text, a 31-page screenshot evidence PDF, and the original screenshot archive. The lab work was completed September 24, 2026. The originals are retained privately and are not linked or distributed here.

The malformed phrase in the later text was corrected to “assignments” in the adaptation. Contributor handles were not treated as publication authors, and inconsistent source dates were not copied into a new academic reference list. The references below use organization names, official titles, and direct links; live documentation can change after the lab date.

## Microsoft Documentation

1. Microsoft. [Best practices for Azure RBAC](https://learn.microsoft.com/en-us/azure/role-based-access-control/best-practices). Microsoft Learn. Supports narrow assignment scope and privileged-access governance.
2. Microsoft. [Azure role-based access control (Azure RBAC) vs. access policies (legacy)](https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-access-policy). Microsoft Learn. Explains the policy-editing risk for vault contributors.
3. Microsoft. [Provide access to Key Vault keys, certificates, and secrets with Azure role-based access control](https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-guide). Microsoft Learn. Supports separation of management and data access, and migration planning.
4. Microsoft. [Use Azure role-based access control to manage Azure Backup recovery points](https://learn.microsoft.com/en-us/azure/backup/backup-rbac-rs-vault). Microsoft Learn. Describes Backup Operator restrictions and operation-specific role requirements.
5. Microsoft. [Troubleshooting backup failures on Azure virtual machines](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-troubleshoot). Microsoft Learn. General backup troubleshooting context; it is not a substitute for the recorded lab error.
6. Microsoft. [Resolve errors for request disallowed by policy](https://learn.microsoft.com/en-us/azure/azure-resource-manager/troubleshooting/error-policy-requestdisallowedbypolicy). Microsoft Learn. Supports investigating the policy assignment behind a denied resource deployment.
7. Microsoft. [Shared responsibility in the cloud](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility). Microsoft Learn. Explains customer responsibilities for IaaS workloads.
8. Microsoft. [Azure encryption overview](https://learn.microsoft.com/en-us/azure/security/fundamentals/encryption-overview). Microsoft Learn. Background for recommended encryption validation; no encryption integration is claimed here.

## Standards Context

9. Joint Task Force. (2020). [Security and Privacy Controls for Information Systems and Organizations, NIST SP 800-53 Revision 5](https://doi.org/10.6028/NIST.SP.800-53r5). National Institute of Standards and Technology. Used for conceptual control mapping, not a claim of certification or assessment completion.
10. PCI Security Standards Council. [PCI Data Security Standard](https://www.pcisecuritystandards.org/standards/pci-dss/). Used for the scenario's payment-security context. Scope and obligations would require confirmation in an actual environment.

These references were checked while preparing the portfolio. Detailed implementation claims are grounded in the student's lab evidence and stated limitations, rather than inferred from product documentation.
