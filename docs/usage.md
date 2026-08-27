# Using Qualix

## Data quality

Define rules for your monitored tables (built-in checks, custom SQL, or
referential-integrity checks). Each rule run produces a pass/fail result and rolls up
into a quality-score trend over time.

- **Suggestions** — profile your granted tables and propose rules, each with a reason
  drawn from that column's own numbers. Nothing is enabled until you approve it.
- **Quarantine** — rows that fail a rule can be quarantined so bad data doesn't spread
  downstream, instead of only being flagged.
- **Gating** — set a rule's **On failure** action to **Block downstream tasks** and
  call `core.gate` from your own task graph to stop a DAG when a gating rule has an
  open failure.

## Observability

Qualix automatically watches monitored tables for unexpected changes in volume,
freshness, and schema, and raises an **incident** when something looks wrong. Use
incident views to:

- See what changed and when
- Read an AI-generated explanation of the likely cause (optional, Cortex-powered)
- Track whether an incident is still open or has recovered

## Discovery

Browse a catalog of your monitored data:

- Table lineage
- Most-used tables
- Stale or unused tables that may no longer be needed

## Governance

Review data classification, policies, access grants, and tag coverage across your
monitored objects.

- **Sensitive data scan** — samples rows to find personal data nobody has classified
  yet.
- **Compliance** — generates masking SQL for confirmed sensitive columns. Qualix never
  runs this SQL for you; it's yours to review and apply.

## Cost

See warehouse spend, burn rate, idle warehouses, query spilling, and your most
expensive queries, so you can keep Snowflake costs in check.

## Recommended first-ten-minutes workflow

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

## Admin

- **Connections** — grant or revoke tables, views, and the warehouse Qualix uses.
- **Schedules** — resume or pause automatic checks; nothing runs, and no credits are
  spent, until this is turned on.
- **Setup & health** — see the name the app was installed under and its current
  health status.
