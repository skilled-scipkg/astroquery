# astroquery source map: Templates

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `cache`
- `clear_cache`
- `login`
- `logout`
- `keyring`
- `remote_data`
- `retry`
- `timeout`

## Fast source navigation
- `rg -n "clear_cache|cache_conf|set_temp|login|_login" astroquery/query.py`
- `rg -n "class MastAuth|login|logout" astroquery/mast/auth.py`
- `rg -n "RemoteServiceError|InvalidQueryError|InputWarning" astroquery/exceptions.py`

## Suggested source entry points
- `astroquery/query.py` | cache toggling, request retry path, and login wrapper behavior
- `astroquery/exceptions.py` | canonical exception/warning classes for triage templates
- `astroquery/utils/system_tools.py` | environment/system helper behavior used during troubleshooting
- `astroquery/mast/auth.py` | MAST authentication/session handling paths
- `astroquery/mast/core.py` | authenticated request dispatch and response parsing behavior
- `astroquery/jplhorizons/core.py` | representative remote-service edge-case handling in a high-traffic module
- `astroquery/mast/tests/test_mast_remote.py` | remote-data regression patterns for service/auth checks
- `conftest.py` | project-level pytest/remote-data fixtures and defaults
