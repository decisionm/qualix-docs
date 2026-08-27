# Required Privileges

Qualix follows a least-privilege approach and only requests the account-level
privileges and object references necessary to perform data-quality, governance, and
cost analysis.

## Account-level privileges

| Privilege | Purpose |
| --- | --- |
| `EXECUTE TASK` | Lets Qualix run its data-quality checks on a schedule. Without it, checks never run and pages stay empty. The on-demand assessment and scans do not need it. |
| `IMPORTED PRIVILEGES ON SNOWFLAKE DB` | Read-only Snowflake account usage: cost and usage insights, governance analysis (classification tags, policies, role grants), and the column metadata behind rule and PII suggestions. |

```sql
GRANT EXECUTE TASK
ON ACCOUNT
TO APPLICATION QUALIX;

GRANT IMPORTED PRIVILEGES
ON DATABASE SNOWFLAKE
TO APPLICATION QUALIX;
```

## Object references (granted per install)

| Reference | Purpose | Access |
| --- | --- | --- |
| Tables to monitor | The tables you want Qualix to discover, profile, and monitor. | Read-only (`SELECT`) |
| Views to monitor | The views you want Qualix to profile and monitor. | Read-only (`SELECT`) |
| Warehouse for Qualix | Runs scheduled checks and powers the app. | `USAGE` |

These are granted from **Admin → Connections** inside the app, and can be revoked at
any time — revoking a grant immediately stops monitoring for that object.

## Optional: Snowflake Cortex

AI features (rule suggestions, PII detection, incident explanations, AI-drafted table
descriptions) run on Snowflake Cortex. This requires a database role that cannot be
requested through the manifest at install time, so it must be granted by hand:

```sql
GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER
TO APPLICATION QUALIX;
```

If this is not granted, the affected pages show the exact command above instead of
failing — every AI feature also has a non-AI fallback (rule suggestions fall back to
column statistics, PII detection falls back to value patterns).

## Qualix does not require

Qualix is designed so that its core functionality does **not** require:

- Write access to any table it monitors
- External network access
- External access integrations
- Third-party API credentials
- Export of Snowflake metadata or data values to an external service
- Continued use of `ACCOUNTADMIN` after administrative setup

!!! note
    Snowflake Native App privileges are governed by the application's manifest and by
    privileges explicitly approved by the consumer. Your Snowflake administrator
    should review the privileges presented during installation.
