# Features

What Qualix can do. For how to actually do it, see [Using Qualix](usage.md).

## Data quality

Define rules for your monitored tables — built-in checks, custom SQL, or
referential-integrity checks. Each rule run produces a pass/fail result and rolls up
into a quality-score trend over time.

- **Rule suggestions** — Qualix profiles your columns and proposes rules, each with a
  reason drawn from that column's own numbers, so a new install has something to
  review instead of a blank page. Nothing is enabled until you approve it.
- **Quarantine** — rows that fail a rule can be moved aside so bad data doesn't spread
  downstream, instead of only being flagged.

## Observability

Qualix automatically watches your monitored tables for unexpected changes in:

- Volume
- Freshness
- Schema

When something looks wrong it raises an **incident**, so you can investigate before it
reaches downstream consumers. Each incident records what changed and when, tracks
whether it is still open or has recovered, and can carry an AI-generated explanation
of the likely cause.

## Discovery

Browse a catalog of your monitored data: table lineage, your most-used tables, and
stale or unused tables that may no longer be needed.

## Governance

Review data classification, policies, access grants, and tag coverage across your
monitored objects.

- **Sensitive data scan** — samples row values to find personal data nobody has
  classified yet.
- **Compliance** — generates masking SQL for the sensitive columns you confirm.
  Qualix never runs that SQL for you; it's yours to review and apply.

## Cost

See warehouse spend, burn rate, idle warehouses, query spilling, and your most
expensive queries, so you can keep Snowflake costs in check.

## Pipeline gating

Set a rule's **On failure** action to **Block downstream tasks**, then call
`core.gate` as a step in your own task graph. It reads Qualix's own rule and
incident state for a table and raises an exception — stopping the DAG — when a
gating rule currently has an open failure. It is strictly read-only.

## AI-assisted, human-approved

Rule suggestions, PII detection, incident explanations, and table descriptions are all
powered by Snowflake Cortex, running inside your account. Every number on a dashboard,
every pass/fail decision, and every generated SQL statement is computed in code, not
by a model — AI proposes candidates for a human to approve; it never decides whether
your data is good.

Every AI feature has a non-AI fallback and can be turned off independently. See
[Security & Privacy](security.md) for exactly what each feature sends to a model.

## Snowflake-native architecture

Qualix is designed as a Snowflake Native App. Its core workflow executes entirely
inside the consumer's Snowflake environment, reducing the need for external
infrastructure and data movement.
