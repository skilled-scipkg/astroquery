# astroquery source map: Inputs and Modeling

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `query_tap`
- `criteria`
- `input_mode`
- `name_input`
- `naifid_input`
- `mpc_input`
- `manual_input`
- `payload`

## Fast source navigation
- `rg -n "query_tap|query_criteria|list_columns|list_tables" astroquery/simbad/core.py`
- `rg -n "CriteriaTranslator|_parse_coordinate|_convert_column" astroquery/simbad/utils.py`
- `rg -n "input_mode|_validate_.*_input_type|query_object|get_images" astroquery/ipac/irsa/most.py`

## Suggested source entry points
- `astroquery/simbad/core.py` | ADQL/TAP execution path and criteria-aware query methods
- `astroquery/simbad/utils.py` | criteria translation and coordinate/string normalization helpers
- `astroquery/ipac/irsa/most.py` | MOST input validation gates and mode-specific parameter handling
- `astroquery/ipac/irsa/core.py` | shared IRSA query transport and parsing behavior
- `astroquery/utils/tap/core.py` | TAP submission behaviors used by ADQL workflows
- `astroquery/utils/tap/taputils.py` | TAP payload/header parsing helpers for edge-case modeling checks
- `astroquery/query.py` | common request/cache mechanics affecting repeated parameterized runs
