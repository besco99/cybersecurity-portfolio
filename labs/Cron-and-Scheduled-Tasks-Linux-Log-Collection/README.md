# Cron and Scheduled Tasks: Linux Log Collection

## Overview

This student case study reviews a narrated Linux cron demonstration that schedules collection of NetworkManager journal entries. Windows Task Scheduler is covered as a comparison, not as a configured or tested system.

## Environment and Objectives

The demonstration uses a Linux user crontab, a Bash script, and the systemd journal. The goals are to interpret a schedule, distinguish job registration from execution, and identify evidence that a scheduled job produced usable output.

## Demonstration

1. The script uses `journalctl` to retrieve NetworkManager entries, processes selected fields with `cut`, and writes the result to a text file.
2. The instructor enables a user crontab entry using `crontab -e`. Its timing expression, `0 7 * * *`, means minute zero of hour seven on every day, in the scheduler's applicable time zone.
3. The instructor uses `crontab -l` to verify that the entry is present. Listing the entry does not prove the daemon ran it.
4. To test promptly, the instructor changes the schedule to the next minute and waits for the job. The lesson reports that a new, populated output file appears at the scheduled time.

Changing only the hour and minute still defines a recurring schedule, not a one-shot trigger. The transcript does not show restoration of the original schedule after the test.

## Linux and Windows Comparison

| Area | Linux cron | Windows Task Scheduler |
| --- | --- | --- |
| When to run | Five time fields in a user crontab | Configured triggers, such as a daily schedule |
| What to run | Command or script | Configured action, such as launching PowerShell |
| Execution identity | Owner of the user crontab | Configured task account |
| What to verify | Job execution and resulting output | Task history or results and resulting output |

## Security and Operational Considerations

- Use a dedicated or least-privileged execution identity with only the required journal and output permissions.
- Use explicit executable and output paths. Scheduled environments can differ from interactive shells.
- Protect scripts from unauthorized changes and restrict access to exported logs.
- Capture errors and exit status. A file can exist even when a job failed or wrote incomplete data.
- Set an explicit collection window and retention policy if turning the example into recurring monitoring. Preserve full event details where needed for investigations.
- Confirm the intended time zone and restore or remove temporary test schedules. Monitor missed runs rather than assuming registration guarantees execution.

## Results and Limitations

The lesson reports one successful timed execution under the temporary schedule. It does not verify recurring 7 a.m. runs, complete log collection, failure alerting, or Windows Task Scheduler execution. This is a review of the supplied lesson, not independently executed work. No script, journal data, screenshots, or terminal output is published.

## Skills and Takeaways

- Interpret cron time fields and reason about execution identity.
- Validate scheduled work using its output, not just its configuration.
- Separate automation of log collection from detection, alerting, and incident response.

Source: User-supplied Cron and Scheduled Tasks lesson, summarized in original language.
