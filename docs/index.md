# Qualix

## Data quality, governance & observability, made visible

**Qualix** is a 100% native Snowflake application that helps teams trust their
data — without moving it outside Snowflake.

Qualix runs entirely inside your Snowflake account and helps you:

- Define data-quality rules, track pass/fail results, and quarantine bad rows
- Watch tables for unexpected changes in volume, freshness, and schema
- Browse a catalog of your data, see lineage, and surface stale or unused tables
- Review classification, policies, access grants, and tag coverage
- Find personal data nobody has classified yet, and generate masking SQL for it
- See warehouse spend, burn rate, idle warehouses, and your most expensive queries

This turns time-consuming manual investigation into a fast, guided data-trust workflow.

## Why Qualix?

Qualix helps data engineers, platform teams, governance owners, and FinOps teams
quickly answer questions such as:

- Which tables have failing quality checks right now?
- Did today's load land on time, and does the row count look right?
- Which columns hold personal data that isn't masked yet?
- Who has access to sensitive tables, and is that access still needed?
- Which warehouses and queries are driving cost?
- Is data quality trending better or worse over time?

## Native by design

Qualix runs entirely within your Snowflake account.

!!! success "Zero data egress"
    No business data, query metadata, credentials, or telemetry is sent to external
    services by Qualix. The app declares no external access integration and has
    no network endpoint of its own.

Qualix does not require external APIs or external network access for its core
data-quality, governance, or cost-analysis workflows. Its AI features run entirely on
Snowflake Cortex, inside your account.

## Get started

Install Qualix from the Snowflake Marketplace, grant the required privileges, and
launch the app from Snowsight.

[Installation guide](installation.md){ .md-button .md-button--primary }
[Required privileges](privileges.md){ .md-button }
