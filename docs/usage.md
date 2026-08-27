# Using Qualix

How to operate Qualix day to day. For what each capability *is*, see
[Features](features.md).

Before you start, make sure you have granted tables, views, and a warehouse under
**Admin → Connections**. See [Installation](installation.md) and
[Required Privileges](privileges.md).

## Your first ten minutes

All three of these are **on demand** — none of them runs on a schedule, and each one
tells you what it will cost before you press it.

1. **Admin → Assessment** → *Run assessment*. Runs the detectors once and reports
   quality issues, unmasked personal data, unused tables, and estimated monthly waste
   as a downloadable, self-contained HTML report.
2. **Data quality → Suggestions** → *Generate*. Profiles your granted tables and
   proposes rules. Approved suggestions are created as alert-only — they never
   quarantine rows until you turn that on.
3. **Governance → Sensitive data** → *Scan for PII*. Samples rows per table and
   reports which columns look personal, then **Governance → Compliance** generates the
   masking SQL for the ones you confirm.

## Set up data-quality rules

1. Go to **Data quality → Suggestions** and select *Generate* to have Qualix propose
   rules from your column statistics. Review each one and approve the rules you want.
2. Add any further rules by hand — built-in checks, custom SQL, or
   referential-integrity checks.
3. Watch results build up as a pass/fail history and a quality-score trend.

Approved rules start as **alert-only**. To quarantine failing rows instead of only
flagging them, turn quarantine on for that rule.

### Block a pipeline on failure

1. Set the rule's **On failure** action to **Block downstream tasks**.
2. Call `core.gate` as a step in your own task graph.

The call raises an exception and stops your DAG whenever that gating rule has an open
failure. Qualix only reads state here — it never modifies your tasks.

## Investigate an incident

When Qualix detects an unexpected change in volume, freshness, or schema, it raises an
incident. Open the incident to:

- See what changed and when
- Read an AI-generated explanation of the likely cause (optional, Cortex-powered)
- Track whether the incident is still open or has recovered

## Explore your data

Use **Discovery** to browse the catalog of your monitored tables, follow lineage,
find your most-used tables, and surface stale ones that may no longer be needed.

## Find and mask sensitive data

1. **Governance → Sensitive data** → *Scan for PII*. Qualix samples row values and
   reports which columns look personal.
2. Confirm the columns that really are sensitive.
3. **Governance → Compliance** generates the masking SQL for them.
4. Review that SQL and run it yourself.

Qualix never executes remediation SQL for you — it holds read-only grants on your
objects. See [Security & Privacy](security.md).

## Review cost

Use **Cost** to check warehouse spend, burn rate, idle warehouses, query spilling, and
your most expensive queries.

## Admin

- **Connections** — grant or revoke tables, views, and the warehouse Qualix uses.
  Revoking a grant immediately stops monitoring for that object.
- **Schedules** — resume or pause automatic checks. Nothing runs, and no credits are
  spent, until this is turned on.
- **Setup & health** — see the name the app was installed under and its current
  health status.
