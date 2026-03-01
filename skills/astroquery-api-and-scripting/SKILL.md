---
name: astroquery-api-and-scripting
description: This skill handles cross-service Astroquery API patterns, TAP usage, and automation-oriented query workflows.
---

# astroquery: API and Scripting

## High-Signal Playbook
### Route conditions
- Use this skill for common API semantics shared across services and TAP-heavy automation.
- Route service-specific behavior details to the target module skill (for example `astroquery-mast`, `astroquery-solarsystem`).
- Route end-user runnable snippets to `astroquery-examples-and-tutorials` if the ask is example-first.

### Triage questions
- Is the call small/interactive (`sync`) or large/staged (`async`)?
- Is this generic service API (`query_object/query_region`) or TAP/ADQL (`launch_job*`)?
- Must results stay server-side (job persistence) or be serialized immediately?
- Is authenticated user space required (uploads, persistent jobs)?
- Is the issue in payload construction, transport, parse step, or caching?
- Are upload tables temporary (`tap_upload`) or persistent user-space tables?

### Canonical workflow
1. Confirm common method contract in `docs/api.rst` and `docs/query.rst`.
2. Use synchronous methods for small result sets; switch to async for large/staged jobs (`docs/utils/tap.rst`).
3. Use `get_query_payload`/request parameters for deterministic debug (`docs/api.rst`).
4. For TAP, instantiate `TapPlus` and run `launch_job` or `launch_job_async` (`docs/utils/tap.rst`, `docs/gaia/gaia.rst`).
5. For upload-join workflows, pass `upload_resource` + `upload_table_name` and query `tap_upload.<name>` (`docs/utils/tap.rst`).
6. Parse via `job.get_results()` and validate row counts/columns.
7. Serialize using module-supported output flags (`dump_to_file`, `output_file`) where available.

### Minimal working example
```python
from astroquery.utils.tap.core import TapPlus

# Generic TAP endpoint example (Gaia TAP endpoint shown)
tap = TapPlus(url="https://gea.esac.esa.int/tap-server/tap")

# Async query for larger/staged workloads
job = tap.launch_job_async("select top 10 source_id, ra, dec from gaiadr3.gaia_source")
results = job.get_results()
print(len(results), results.colnames)

# On-the-fly upload + join
upload_resource = "my_table.xml"
join_job = tap.launch_job(
    query="select * from tap_upload.table_test",
    upload_resource=upload_resource,
    upload_table_name="table_test",
)
print(len(join_job.get_results()))
```
- Backing docs: `docs/api.rst`, `docs/utils/tap.rst`, `docs/gaia/gaia.rst`.

### Pitfalls and fixes
- TAP sync over large output: sync mode is not for big results; use async (`docs/utils/tap.rst`).
- Assuming identical retention semantics: anonymous async retention differs from authenticated persistence (`docs/utils/tap.rst`, `docs/gaia/gaia.rst`).
- Upload query failures: ensure ADQL references `tap_upload.<table_name>` exactly (`docs/utils/tap.rst`).
- Hidden row truncation: inspect per-service row limits before trusting counts (`docs/gaia/gaia.rst`).
- Parse/transport ambiguity: use payload echo and job metadata before source deep dive.

### Convergence and validation checks
- Job phase is `COMPLETED` and `job.get_results()` returns expected schema.
- Returned row count matches limits/filters and does not silently truncate.
- Output artifacts exist when `dump_to_file`/`output_file` options are enabled.
- Re-running with cache disabled reproduces transport/parsing behavior as expected.

## Scope
- Common API interfaces, scripting patterns, TAP jobs, and upload/query workflows.
- Keep responses implementation-aware but service-agnostic unless asked otherwise.

## Primary documentation references
- `docs/api.rst`
- `docs/query.rst`
- `docs/utils/tap.rst`
- `docs/gaia/gaia.rst`

## Workflow
- Start with docs above.
- Expand through `references/doc_map.md` when needed.
- Escalate to ranked files in `references/source_map.md` only if docs are insufficient.

## Source entry points for unresolved issues
- `astroquery/query.py`
- `astroquery/utils/tap/core.py`
- `astroquery/utils/tap/taputils.py`
- `astroquery/utils/tap/conn/tapconn.py`
- `astroquery/gaia/core.py`
- `astroquery/esa/jwst/core.py`
- `astroquery/esa/euclid/core.py`
- `astroquery/mast/observations.py`
- Prefer targeted search: `rg -n "<symbol_or_keyword>" astroquery`.
