# astroquery source map: API and Scripting

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `get_query_payload`
- `async`
- `launch_job`
- `launch_job_async`
- `upload_resource`
- `upload_table_name`
- `tap_upload`
- `job`
- `output_file`
- `dump_to_file`

## Fast source navigation
- `rg -n "launch_job|launch_job_async|upload_resource|upload_table_name|login|logout" astroquery/utils/tap/core.py`
- `rg -n "tap_upload|get_jobid_from_location|set_top_in_query" astroquery/utils/tap/taputils.py`
- `rg -n "class BaseQuery|def _request|get_query_payload|cache_conf" astroquery/query.py`

## Suggested source entry points
- `astroquery/query.py` | common request/payload/cache behavior across services
- `astroquery/utils/tap/core.py` | TAP orchestration: `Tap.launch_job`, `Tap.launch_job_async`, `TapPlus.login`, `upload_table`
- `astroquery/utils/tap/conn/tapconn.py` | low-level TAP HTTP request/response handling and connection behavior
- `astroquery/utils/tap/taputils.py` | ADQL/output helpers and TAP response parsing utilities
- `astroquery/utils/tap/model/job.py` | async job state/result lifecycle object
- `astroquery/utils/tap/model/modelutils.py` | TAP model parsing utilities used by table/job abstractions
- `astroquery/gaia/core.py` | Gaia-specific TAP+ wrappers and row-limit/persistence behavior
- `astroquery/esa/euclid/core.py` | ESA Euclid TAP patterns and authenticated workflows
- `astroquery/esa/jwst/core.py` | ESA JWST TAP/query integration behaviors
- `astroquery/mast/observations.py` | service-level async/query-size patterns that often mirror TAP staging concerns
- `astroquery/ipac/irsa/ibe/core.py` | IBE query/payload behavior for script-based cutout/archive automation
