# Access Decisions

## Implemented Assignment Matrix

Generic names below replace the supplied accounts and generated identifiers.

| Principal | Role or access policy | Scope | Purpose |
| --- | --- | --- | --- |
| Accounting user | Reader | Accounting resource group | View departmental resource configuration |
| Marketing user | Reader | Marketing resource group | View departmental resource configuration |
| IT user | Reader | IT resource group | View IT resource configuration |
| Accounting user | Key Vault Contributor | Accounting Key Vault | Manage the departmental vault resource |
| Marketing user | Key Vault Contributor | Marketing Key Vault | Manage the departmental vault resource |
| IT user | Key Vault Contributor | IT Key Vault | Manage the departmental vault resource |
| IT user | Backup Operator | Backup-Vault only | Perform permitted backup operations and review status |
| Each matching departmental user | List keys, secrets, and certificates | Its departmental vault access policy | Enumerate the listed object categories |

The earlier eligible Backup Operator assignment at IT resource-group scope was removed. This narrowed that role assignment to the backup vault; it did not remove the IT user's separate Reader and vault-management assignments.

## Why These Scopes Were Chosen

Reader was selected for departmental inventory visibility because the scenario did not establish a general infrastructure-administration duty for the supplied departmental users. Reader does not grant VM operating-system sign-in or business-application access.

Vault-management assignments were placed on individual vault resources. Backup Operator was placed on the central Recovery Services vault rather than the entire IT resource group. Policy creation was an administrative activity performed in the lab; it was not evidence that the departmental IT user's Backup Operator role could manage policies.

Microsoft describes Backup Operator as excluding backup-policy management and backup removal. Restore operations may also need permissions on destination resources. See [Azure Backup role guidance](https://learn.microsoft.com/en-us/azure/backup/backup-rbac-rs-vault).

## Key Vault Control Plane and Data Plane

Key Vault Contributor manages the vault resource; it does not directly supply secret-value retrieval or cryptographic operations. The configured legacy access policies separately granted List for keys, secrets, and certificates.

However, a vault contributor can edit access policies under that legacy model and grant additional data access. The saved List-only policy is therefore **not an enforceable ceiling for that vault manager**. The lab narrowed role scopes but did not establish strict separation of administrative and data-access duties. This limitation is documented by [Microsoft's comparison of RBAC and access policies](https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-access-policy).

The policy entries did not explicitly grant Get, signing, decryption, creation, deletion, or purge. That describes the entries saved at the time, not every operation the principal could authorize later.

## What Was Checked

The report records reviewing current assignments on the departmental vaults and Backup-Vault. The selected screenshots show:

- Vault-scoped Key Vault Contributor alongside inherited resource-group Reader
- Vault-scoped Backup Operator alongside inherited Reader
- A saved departmental access policy with List in all three permission categories

These views support assignment configuration. They do not prove successful data-plane operations, a complete inventory of group-based or inherited access, or denial from every other department.

The source report described inherited lab-administration and service assignments that were left unchanged. This case study does not attribute the scenario's reported cross-department access to a proven cause.

## Production Recommendation — Not Implemented

Migrate vaults to the Azure RBAC permission model after inventorying dependent callers and planning equivalent data roles. Separate resource administration from permission administration and data access. Use managed groups, justified role scopes, privileged activation where appropriate, and recurring access review.

The migration needs testing and rollback planning because changing the permission model can interrupt existing callers. It was not performed in this lab. [Microsoft's RBAC guidance](https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-guide) explains the model and migration considerations.

List policies and vault placement are access decisions, not implementation of disk encryption, customer-managed keys, certificate integration, or application TLS.
