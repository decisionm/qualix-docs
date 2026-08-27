# Security & Privacy

## Zero data egress

Qualix runs entirely inside the consumer's Snowflake account. It declares no external
access integration and has no network endpoint of its own — no external service is
required for its core data-quality, governance, or cost-analysis workflows.

## Data analyzed

Qualix analyzes only the tables, views, and warehouse you explicitly grant it, plus
Snowflake account-usage metadata (query history, access history, classification tags,
policies, and role grants) where granted. Access is **read-only** in every case.

## Qualix never changes your objects

Every remediation Qualix produces — masking policies, row-access policies,
classification calls — is **SQL for you to read and run yourself**. Qualix never
executes any of it; it holds read-only grants on your objects, so it could not. A
governance tool that can silently alter what it audits is not one you should trust.

## What the AI features do, and do not, send to a model

The AI features run on **Snowflake Cortex, inside your account**. Nothing leaves it.
Even so, what reaches a prompt is deliberately limited:

- **Rule suggestions** send column names, data types, and aggregate statistics (row
  counts, null counts, approximate distinct counts). **No values.**
- **PII detection** samples up to 20 values per column and **masks them before they
  leave the query** — email addresses become `<email>`, long digit sequences become
  `<number>`. Only a column name, category, and match count are stored. **No value is
  ever stored.**
- **Incident explanations** and the executive summary send counts, thresholds, and
  detector names — never a query, a row, or a raw log line.
- **AI-drafted table descriptions** send a table name and its column names/types. No
  row data, comments, or existing metadata.
- **Suggested search terms** send only the phrase you typed into Search.
- **Quarantine explanations** are the one feature that sends row values; they are
  masked first, and the feature has its own separate opt-in for exactly that reason.

Each AI feature can be turned off independently, and each still works without Cortex:
rule suggestions fall back to column statistics, and PII detection to value patterns.

## Where AI is not used

Every number on a dashboard, every pass/fail decision, every anomaly threshold, and
every generated SQL statement is computed in code, not by a model. AI writes
explanations and proposes candidates for a human to approve; it never decides whether
your data is good.

## Optional usage telemetry

Qualix can optionally share **anonymized operational events** with the provider
through Snowflake's own [event sharing](https://docs.snowflake.com/en/developer-guide/native-apps/event-about)
mechanism — no network egress, and you control it. What's shared, when it's on:

- Page views, by page name only.
- Detector and procedure runs: which one, how long it took, whether it succeeded.
- Error codes — never the raw error text.
- Feature usage counts — not what you did with the feature.

**Never shared:** table names, column names, query text, or any value from your data.

This is off unless you turn it on. Snowflake requires event sharing to be explicitly
enabled per installation, and every event definition Qualix declares is optional, so
leaving it alone means nothing is shared. Turning it off again stops sharing
immediately — Qualix works identically either way.

## Privilege model

Snowflake Native Apps operate under Snowflake's application security model.
Consumers control the privileges granted to an installed application. See
[Required Privileges](privileges.md) for the full list Qualix requests.
