---
name: astroquery-inputs-and-modeling
description: This skill handles input-shaping and criteria/modeling workflows in Astroquery, especially SIMBAD ADQL/TAP and IRSA MOST mode-specific parameters.
---

# astroquery: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this skill when the user’s main problem is input semantics (criteria structure, input mode fields, or parameter validation).
- Route generic TAP job lifecycle questions to `astroquery-api-and-scripting`.
- Route service-independent troubleshooting to `astroquery-templates`.

### Triage questions
- Is input ADQL/TAP-oriented (SIMBAD) or mode/field-oriented (IRSA MOST)?
- Does the user need payload introspection before executing remote calls?
- Which `input_mode` is required for MOST (`name_input`, `naifid_input`, `mpc_input`, `manual_input`)?
- Are required fields present for the selected mode?
- Are constraints expected in server syntax (`criteria` string/ADQL) or Python-side filtering?

### Canonical workflow
1. Confirm accepted input structure from docs (`docs/simbad/query_tap.rst`, `docs/ipac/irsa/most.rst`).
2. Build the smallest valid query/payload first.
3. For SIMBAD, test query shape with `get_query_payload=True` or a `TOP`-limited ADQL query.
4. For MOST, set `input_mode` first, then provide only fields required by that mode.
5. Run one short time window (`obs_begin`/`obs_end`) before large retrievals.
6. Validate returned columns and row count before downstream analysis.

### Minimal working example
```python
from astroquery.simbad import Simbad
from astroquery.ipac.irsa import Most

# SIMBAD TAP input-shape check
adql = "SELECT TOP 5 basic.main_id, basic.ra, basic.dec FROM basic"
rows = Simbad.query_tap(adql)
print(len(rows), rows.colnames)

# MOST mode-specific input check
most_rows = Most.query_object(
    input_mode="name_input",
    obj_name="Ceres",
    output_mode="Brief",
    obs_begin="2010-01-01",
    obs_end="2010-01-02",
)
print(len(most_rows), most_rows.colnames)
```
- Backing docs: `docs/simbad/query_tap.rst`, `docs/ipac/irsa/most.rst`.

### Pitfalls and fixes
- Mixing MOST input modes and parameters: set one `input_mode` and satisfy only that mode’s required fields (`astroquery/ipac/irsa/most.py`).
- Overly broad SIMBAD ADQL from the start: use `TOP` and add constraints iteratively.
- Treating input modeling as post-filtering only: push criteria to service query syntax when possible.
- Assuming all services share identical criteria grammar: keep service-specific docs open while shaping inputs.

### Convergence and validation checks
- Query returns expected schema and non-empty rows for a known target.
- Payload/ADQL string is deterministic and reusable.
- MOST run succeeds with no missing-required-field warnings for chosen `input_mode`.
- Expanding time range or criteria changes counts predictably.

## Scope
- Input contracts and modeling patterns for SIMBAD and IRSA MOST.
- Focus on building valid, minimal, testable requests before scale-up.

## Primary documentation references
- `docs/simbad/query_tap.rst`
- `docs/simbad/simbad.rst`
- `docs/ipac/irsa/most.rst`
- `docs/ipac/irsa/irsa.rst`
- `docs/utils/tap.rst`

## Workflow
- Start with docs and derive minimal valid inputs.
- Use `references/doc_map.md` for nearby modeling docs.
- Escalate to `references/source_map.md` when validation behavior differs from docs.

## Source entry points for unresolved issues
- `astroquery/simbad/core.py`
- `astroquery/simbad/utils.py`
- `astroquery/ipac/irsa/most.py`
- `astroquery/ipac/irsa/core.py`
- `astroquery/utils/tap/core.py`
- `astroquery/utils/tap/taputils.py`
- `astroquery/query.py`
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" astroquery`.
