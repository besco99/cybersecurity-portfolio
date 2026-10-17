# Backup Configuration and Troubleshooting

## Saved Configuration

| Setting | Lab value |
| --- | --- |
| Vault | Backup-Vault, a Recovery Services vault |
| Policy | SWBTL |
| Policy type | Standard Azure VM backup |
| Schedule | Daily at 7 p.m. |
| Selected time zone | Eastern Time (US & Canada) |
| Instant snapshot retention | 3 days |
| Daily recovery-point retention | 45 days |
| Assigned VMs | Accounting, Marketing, and IT |

The selected time-zone entry was retained rather than converted into a fixed UTC schedule. Snapshot and recovery-point values were configured retention settings; the failed jobs did not create a demonstrated recovery history.

## Observed Sequence

1. The initial evidence showed no Azure VM backup items and no SWBTL policy.
2. The Standard policy was created and saved.
3. All three VMs were assigned to it.
4. An earlier screen showed pre-check Passed and initial backup pending.
5. Initial backup attempts were made.
6. A later screen showed Critical and Failed for every VM, with no latest restore point displayed.

The original archive filename for the final status screen suggests jobs in progress. Its visible contents show failures, so the gallery uses the accurate filename `07-initial-backup-failures.png`.

## Recorded Failure

The private evidence PDF includes a written transcription of captured pre-check and Activity log details:

| Field | Sanitized recorded value |
| --- | --- |
| Pre-check error | `UserErrorRequestDisallowedByPolicy` |
| Activity log error | `RequestDisallowedByPolicy` |
| Attempted operation | Create or Update Restore Point Collection |
| Blocked resource type | `Microsoft.Compute/restorePointCollections` |
| Initiator | Backup Management Service |
| Policy scope | Inherited management-group policy |
| Effect | Deny |

The source note reports that the restore-point resource type did not match the policy's allowed resource types. Policy display names, definition and assignment identifiers, management-group names, and resource IDs are omitted.

**Evidence distinction:** the gallery directly shows three failed VM backup items. The detailed denial is supported by the source report and its written evidence note, not by a screenshot showing the expanded error. The Activity log overview alone does not display that message.

## Interpretation

The recorded denial identifies an inherited resource-creation restriction. It does not show that the selected schedule or retention values caused the failure. The inherited lab policy was left unchanged; no exemption or bypass was implemented.

All three attempts failed. The captured detailed denial documents the policy blocker; a separate expanded error for each VM is not available in the selected gallery. The case study does not invent additional logs.

## Proposed Resolution and Verification — Not Completed

The authorized policy owner would first confirm the deny assignment and determine whether the required resource type can be permitted within the training boundary. No broad role escalation or unauthorized policy change is proposed as a workaround.

After an approved resolution:

1. Retry initial backups for all three VMs.
2. Confirm successful jobs and usable recovery points.
3. Check retention behavior and monitor subsequent scheduled jobs.
4. Restore into an approved isolated destination.
5. Validate files and application behavior.
6. Record elapsed recovery time and data age against agreed objectives.

These are follow-up recommendations, not successful outcomes from the lab. Microsoft's [Azure VM backup troubleshooting guidance](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-troubleshoot) and [Azure Policy deny troubleshooting](https://learn.microsoft.com/en-us/azure/azure-resource-manager/troubleshooting/error-policy-requestdisallowedbypolicy) provide general context; the specific cause here comes from the lab evidence.

## Recovery Objectives

The report discusses a one-day RPO and a 36-hour RTO as scenario targets. A schedule alone does not prove either objective. Failed jobs can exceed the permitted data-loss window, and recovery duration requires timed restore and application validation.

**Neither objective was demonstrated as achieved.** No successful VM restore, file recovery, or application recovery was recorded.
