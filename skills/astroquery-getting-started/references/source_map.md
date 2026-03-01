# astroquery source map: Getting Started

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `install`
- `import`
- `query_object`
- `query_region`
- `cache`
- `clear_cache`
- `cache_conf`
- `configuration`

## Fast source navigation
- `rg -n "query_object|query_region|clear_cache|get_query_payload" astroquery/simbad/core.py astroquery/vizier/core.py`
- `rg -n "class BaseQuery|def clear_cache|cache_conf" astroquery/query.py`
- `rg -n "InputWarning|RemoteServiceError|InvalidQueryError" astroquery/exceptions.py`

## Suggested source entry points
- `astroquery/query.py` | base transport/caching behavior: `BaseQuery._request`, `BaseQuery.clear_cache`, `QueryWithLogin.login`
- `astroquery/simbad/core.py` | first-query behavior: `SimbadClass.query_object`, `query_region`, `clear_cache`
- `astroquery/vizier/core.py` | region/object query adapters and payload inspection options
- `astroquery/utils/class_or_instance.py` | class-or-instance dispatch used by service query methods
- `astroquery/utils/commons.py` | shared parsing/validation helpers used by multiple services
- `astroquery/exceptions.py` | warning/error classes surfaced during startup troubleshooting
