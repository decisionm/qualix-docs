# Features

## Data quality

Define rules for your tables, track pass/fail results, and follow a quality-score
trend over time. Rows that fail can be quarantined so bad data doesn't spread
downstream. Qualix can also **suggest rules for you** by profiling your columns,
so a new install has something to review instead of a blank page.

## Observability

Qualix automatically watches your tables for unexpected changes in:

- Volume
- Freshness
- Schema

When something looks wrong, it raises an incident so you can investigate before it
reaches downstream consumers.

## Discovery

Browse a catalog of your data, see table lineage, find your most-used tables, and
surface stale ones that may no longer be needed.

## Governance

Review data classification, policies, access grants, and tag coverage. Qualix also
**looks at your column values** to find personal data nobody has classified yet, and
**generates the masking SQL** to protect it — it never applies that SQL for you.

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

## Snowflake-native architecture

Qualix is designed as a Snowflake Native App. Its core workflow executes entirely
inside the consumer's Snowflake environment, reducing the need for external
infrastructure and data movement.
