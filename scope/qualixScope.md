# Qualix Documentation

## Overview

Qualix (SnowQualIx) is a 100% native Snowflake application by Decision Minds Inc.
that helps teams trust their data — tracking data quality, observability,
discovery, governance, and cost — without moving data outside Snowflake.

Qualix analyzes the tables, views, and warehouse you grant it, plus Snowflake
account usage metadata, to help you:

- Define data-quality rules, track pass/fail results, and a quality-score trend
  over time
- Quarantine rows that fail a rule so bad data doesn't spread downstream
- Watch tables for unexpected changes in volume, freshness, and schema, and
  raise incidents when something looks wrong
- Browse a catalog of your data, see table lineage, and find your most-used and
  stale tables
- Review data classification, policies, access grants, and tag coverage — and
  find personal data nobody has classified yet
- See warehouse spend, burn rate, idle warehouses, query spilling, and your
  most expensive queries

This turns manual data-trust investigation into a fast, guided workflow.

Marketplace Listing: not yet published — installation is currently by direct
package share from Decision Minds Inc. Update this line once the Marketplace
listing goes live.

## Why Qualix?

Qualix helps data engineers, data platform teams, governance/compliance teams,
and FinOps teams quickly answer questions such as:

- Which tables have failing or degrading data-quality rules?
- Which tables changed unexpectedly in row count, freshness, or schema?
- What data do we have, who uses it, and what's gone stale?
- Which columns hold personal data we haven't classified or masked yet?
- Which warehouses, users, or queries are driving cost?
- Is our data quality trend getting better or worse over time?

## Native by Design

Qualix runs entirely within your Snowflake account:

- **Zero data egress for business data** — No table data, column values, query
  text, or credentials are sent to external services.
- **No external APIs** — Qualix declares no external access integration and has
  no network endpoint of its own.
- **No third-party credentials** — No API keys or tokens from external services
  are required.
- **AI features run on Snowflake Cortex**, inside your account — nothing leaves
  it for those either.
- **Optional anonymized usage telemetry** — page views by page name, detector
  run duration/status, error codes, and feature-usage counts only. Never table
  names, column names, query text, or a data value. Shared only through
  Snowflake's own event-sharing mechanism, and only if you turn it on
  (`ALTER APPLICATION ... SET SHARE_EVENTS_WITH_PROVIDER = TRUE`). Off by
  default.

## Installation

### Prerequisites

Before installing Qualix, ensure that you have:

- A Snowflake account
- Sufficient privileges to install a Snowflake Native App
- One or more tables or views you want Qualix to monitor
- A Snowflake virtual warehouse Qualix can use to run scheduled checks and
  power the app
- Permission to grant the application access to the Snowflake system database
  where required (for cost/usage and governance pages)

An XS or S warehouse is sufficient for most accounts. Larger or high-volume
Snowflake environments may benefit from a larger warehouse.

### Install from Snowflake Marketplace

1. Sign in to Snowsight.
2. Open Marketplace.
3. Search for QUALIX.
4. Open the Qualix listing.
5. Select Get.
6. Follow the Snowflake installation prompts.
7. Complete any requested privilege grants.

(Until the Marketplace listing is live, install from the package share your
Decision Minds Inc. contact provides instead.)

### Post-Installation Configuration

After installing, open the app and go to **Admin → Connections** to grant:

- **Tables to monitor** — the tables you want Qualix to discover, profile, and
  monitor. Read-only (SELECT) access.
- **Views to monitor** — the views you want Qualix to profile and monitor.
  Read-only (SELECT) access.
- **Warehouse for Qualix** — the warehouse Qualix uses to run scheduled checks
  and to power the app.

Then, in Snowsight under *Data Products → Apps → Qualix → Privileges*, grant:

```sql
-- Lets Qualix run its data-quality checks on a schedule.
-- Without it, checks never run and pages stay empty. The on-demand
-- assessment and scans do not need it.
GRANT EXECUTE TASK ON ACCOUNT TO APPLICATION <app_name>;

-- Read-only Snowflake account usage: cost and usage insights, governance
-- analysis (classification tags, policies, role grants), and the column
-- metadata behind rule and PII suggestions.
GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO APPLICATION <app_name>;
```

**Warning:** Replace `<app_name>` with the name you installed Qualix under
(Admin → Setup & health shows it).

**Optional — for AI features:** Qualix's AI features (rule suggestions, PII
detection narratives, incident explanations, AI-drafted table descriptions,
suggested search terms) run on Snowflake Cortex. `manifest.yml` cannot request
a database role at install time, so grant it by hand if you want these
features:

```sql
GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO APPLICATION <app_name>;
```

Without this grant, every AI feature shows a one-line notice with this exact
command instead of failing, and the non-AI half of each feature keeps working
(rule suggestions fall back to column statistics; PII detection falls back to
value patterns).

Note: The exact privileges available to a Snowflake Native App depend on the
application manifest, requested references, the Snowflake Native App security
model, and the privileges approved during installation.

### Verify Installation

1. Open Apps in Snowsight.
2. Select Qualix.
3. Launch the application.
4. Confirm the dashboard loads.
5. Go to **Admin → Assessment** and select **Run assessment**.
6. Quality, PII, and cost findings should begin appearing once the app can
   access the granted tables/warehouse and the required Snowflake metadata.

If the dashboard remains empty, see the Troubleshooting section.

## Required Privileges

Qualix follows a least-privilege approach and only requires privileges
necessary to perform data-quality, observability, discovery, governance, and
cost analysis.

| Privilege | Purpose |
|---|---|
| SELECT on granted tables/views (via references) | Lets Qualix profile, monitor, and run quality rules against the specific tables and views you choose. Read-only. |
| USAGE ON WAREHOUSE (via reference) | Allows Qualix to execute scheduled checks and power the app using the selected warehouse. |
| EXECUTE TASK | Lets Qualix run its data-quality checks on a schedule. Without it, checks never run and pages stay empty; the on-demand assessment and scans do not need it. |
| IMPORTED PRIVILEGES ON SNOWFLAKE DB | Read-only Snowflake account usage: cost and usage insights, governance analysis (classification tags, policies, role grants), and the column metadata behind rule and PII suggestions. |
| DATABASE ROLE SNOWFLAKE.CORTEX_USER (optional, granted by hand) | Powers the AI features (rule suggestions, PII narratives, incident explanations, AI-drafted descriptions, search suggestions). Every AI feature degrades to a notice, not an error, without it. |

## Qualix Does NOT Require

- Access to tables you have not explicitly granted
- Write access to any customer table, view, or object — every remediation
  Qualix produces (masking policies, row-access policies, classification
  calls) is SQL for you to read and run yourself; Qualix never executes it
- External network access
- External access integrations
- Third-party API credentials
- Export of business data, column values, or query text to an external service
- Continued use of ACCOUNTADMIN after administrative setup

## Security and Privacy

### Zero Data Egress for Business Data

Qualix's core data-quality, observability, discovery, governance, and cost
analysis runs entirely inside the consumer Snowflake account. No
Qualix-managed external service is required for any of it.

### Data Analyzed

Qualix analyzes the specific tables/views granted to it, plus Snowflake
account usage metadata (query history, warehouse metering, access history,
classification/tag/policy metadata) made available to the application.

### Business Data

Qualix requires read-only (SELECT) access only to the tables and views you
explicitly grant through Admin → Connections. It does not require unrestricted
access to the rest of your account.

### External Network Access

Qualix does not require external network access for any of its core
functionality. It declares no external access integration and has no network
endpoint of its own.

### Credentials

Qualix does not require customers to provide third-party API credentials for
any of its functionality.

### AI Features

AI features run on Snowflake Cortex, inside your account. What reaches a
prompt is deliberately limited — see the README's "What the AI features do,
and do not, send to a model" section for the exact field-level breakdown per
feature (rule suggestions, PII detection, incident explanations, AI-drafted
descriptions, search suggestions, quarantine explanations). Each AI feature
can be turned off independently and still works without Cortex.

### Privilege Model

Snowflake Native Apps operate under the Snowflake application security model.
Consumers control the privileges granted to an installed application.
Administrators should review all requested privileges during installation and
grant only those required for the intended functionality.

## Troubleshooting

### No data appears in the dashboard

Check the following:

- Confirm Qualix has the required privileges (EXECUTE TASK, IMPORTED
  PRIVILEGES ON SNOWFLAKE DB).
- Confirm at least one table or view is granted under Admin → Connections.
- Confirm the assigned warehouse exists and can resume.
- Confirm scheduled checks have been resumed under Admin → Schedules — nothing
  runs, and no credits are spent, until you turn this on.
- Try **Admin → Assessment → Run assessment** for an immediate, on-demand
  result instead of waiting for the next scheduled cycle.
- Snowflake account-usage views can have ingestion latency, so very recent
  activity may not immediately appear on cost/usage pages.

### Recent activity is missing from cost/usage or governance pages

Some Snowflake account-usage metadata is not real-time. Allow for the
documented latency of the underlying Snowflake views Qualix reads.

### Historical data is missing

The amount of historical query/usage data available depends on Snowflake's
retention for the account-usage views Qualix reads. If the requested date
range exceeds available history, older records will not be returned.

### AI features show a notice instead of a result

This means `SNOWFLAKE.CORTEX_USER` has not been granted to the application.
The notice includes the exact `GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO
APPLICATION <app_name>;` command to run. The non-AI half of the feature (rule
suggestions from column statistics, PII detection from value patterns) keeps
working either way.

### Qualix feels slow

Try the following:

- Narrow the selected time range on cost/usage and history views.
- Reduce the number of tables/views monitored, or review your rule count on
  the Rules page.
- Use a larger virtual warehouse for Qualix.
- Check Admin → Schedules for tasks that are drifted or overlapping.

### Warehouse error

If Qualix cannot use the selected warehouse:

- Confirm the warehouse name under Admin → Connections.
- Confirm the warehouse has not been dropped.
- Verify the application still holds the USAGE grant on that warehouse.
- Confirm the warehouse can auto-resume, or manually resume it.

### Permission error

Ask a Snowflake administrator to review the privileges granted to the
application (Data Products → Apps → Qualix → Privileges) and compare them with
the privileges requested in `manifest.yml` and this document.

### Rules or scheduled checks not running

- Confirm Admin → Schedules shows tasks as resumed, not suspended.
- Confirm EXECUTE TASK has been granted to the application.
- Confirm the warehouse Qualix uses can resume and has not been dropped.

## Support

For questions, installation assistance, or issues with Qualix, use the support
contact provided on Qualix's Snowflake Marketplace listing page (once
published), or contact your Decision Minds Inc. representative directly.

When reporting an issue, include:

- A short description of the problem
- The Qualix page or feature affected
- Approximate time the issue occurred
- Relevant Snowflake error message, if any
- Browser and Snowsight details when the issue is UI-related

**Important:** Do not include passwords, private keys, authentication tokens,
or other secrets in support requests.

For Snowflake platform-specific behavior, refer to the official Snowflake
documentation at docs.snowflake.com.
