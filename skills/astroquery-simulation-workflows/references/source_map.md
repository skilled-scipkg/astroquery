# astroquery source map: Simulation Workflows

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `remote_data`
- `query_region`
- `exec_sync`
- `create_async`
- `launch_job_async`
- `cache_conf`
- `deterministic`
- `reproducible`

## Fast source navigation
- `rg -n "query_region_async|exec_sync|create_async|get_images|get_data_urls" astroquery/cadc/core.py`
- `rg -n "launch_job|launch_job_async|upload_resource|output_file" astroquery/utils/tap/core.py`
- `rg -n "cache_conf|clear_cache|_request" astroquery/query.py`

## Suggested source entry points
- `astroquery/cadc/core.py` | CADC query/download primitives used for staged workflows
- `astroquery/utils/tap/core.py` | TAP sync/async orchestration and upload handling
- `astroquery/utils/tap/conn/tapconn.py` | low-level request transport and retry/error details
- `astroquery/query.py` | cache and transport behavior shared across services
- `astroquery/cadc/tests/test_cadctap.py` | local behavior checks for CADC table/query utilities
- `astroquery/cadc/tests/test_cadctap_remote.py` | remote-data CADC regression checks
- `astroquery/utils/tap/tests/test_tap.py` | TAP lifecycle behavior checks for job workflows
- `conftest.py` | cross-test fixtures and remote-data test settings
