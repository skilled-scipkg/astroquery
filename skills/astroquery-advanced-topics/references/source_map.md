# astroquery source map: Advanced Topics

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `configuration`
- `changelog`
- `license`
- `release`
- `template`
- `validator`
- `simbad`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" astroquery`
- `rg -n "class|def|namespace" astroquery`
- Search exact symbol names from docs first, then inspect nearby implementation files.

## Suggested source entry points
- `astroquery/query.py` | config/cache/login plumbing
- `astroquery/exceptions.py` | exception and warning types
- `astroquery/template_module/core.py` | template module behavior
- `astroquery/simbad/core.py` | SIMBAD behavior tied to migration notes
- `astroquery/simbad/utils.py` | SIMBAD helpers
- `astroquery/vo_conesearch/validator/validate.py` | validator execution path
- `astroquery/vo_conesearch/validator/tstquery.py` | test-query helpers
- `astroquery/vo_conesearch/validator/inspect.py` | validator inspection helpers
- `astroquery/vo_conesearch/validator/exceptions.py` | validator exception classes
