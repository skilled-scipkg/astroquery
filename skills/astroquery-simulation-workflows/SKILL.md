---
name: astroquery-simulation-workflows
description: This skill handles reproducible Astroquery simulation-style workflows: staged remote queries, deterministic payload checks, and remote-data validation loops.
---

# astroquery: Simulation Workflows

## High-Signal Playbook
### Route conditions
- Use this skill when users need repeatable, staged computational workflows (query -> artifact -> validation), not just one-off lookups.
- Use it for remote-data test orchestration, deterministic replay checks, and simulation-like parameter sweeps.
- Route service-specific API semantics to the corresponding module skill once workflow scaffolding is stable.

### Triage questions
- Which backend is the workflow centered on (CADC TAP, generic TAP, or mixed services)?
- Is the run public-only or authenticated?
- What is the minimal deterministic input set (query text, coordinates, date range, row limit)?
- Which output artifact is required (table, file, download manifest)?
- What is the acceptance check (schema, row-count bounds, stable columns, file existence)?

### Canonical workflow
1. Start from a tiny reproducible input set and explicit limits (`TOP`, short date windows, small radius).
2. Capture payload before scale-up (`get_query_payload=True` where supported).
3. Run one synchronous pass, then switch to async/staged mode for larger runs.
4. For repeatability checks, compare cache-on and cache-off runs.
5. Promote the scenario into a remote-data test command once stable.
6. Validate schema/units and artifact presence before scientific interpretation.

### Quick-start commands
```bash
python -m pip install -U --pre astroquery[test]
python -m pytest -P cadc -m remote_data --remote-data=any --maxfail=1
python -m pytest -P utils/tap -m remote_data --remote-data=any --maxfail=1
```

### Minimal workflow seed
```python
from astropy.coordinates import SkyCoord
import astropy.units as u
from astroquery.cadc import Cadc
from astroquery import cache_conf

cadc = Cadc()
coords = SkyCoord("10h00m00s +02d00m00s", frame="icrs")

with cache_conf.set_temp("cache_active", False):
    rows = cadc.query_region(coords, radius=0.01 * u.deg)

print(len(rows), rows.colnames[:5])
```
- Backing docs: `docs/cadc/cadc.rst`, `docs/testing.rst`.

### Validation checkpoints
- Input set is explicit and versionable (query string, coordinates, radius/time range).
- Returned table schema is checked before row-value assertions.
- Cache-disabled rerun reproduces expected shape/units.
- Remote-data test command passes with bounded runtime and deterministic checks.

## Scope
- Reproducible computational workflows over remote scientific archives.
- Emphasize deterministic setup, staged execution, and test-backed validation.

## Primary documentation references
- `docs/cadc/cadc.rst`
- `docs/utils/tap.rst`
- `docs/query.rst`
- `docs/testing.rst`

## Workflow
- Start with docs-first workflow definition and minimal inputs.
- Use `references/doc_map.md` to expand coverage.
- Use `references/source_map.md` for implementation-level behavior checks.

## Source entry points for unresolved issues
- `astroquery/cadc/core.py`
- `astroquery/utils/tap/core.py`
- `astroquery/utils/tap/conn/tapconn.py`
- `astroquery/query.py`
- `astroquery/cadc/tests/test_cadctap_remote.py`
- `astroquery/utils/tap/tests/test_tap.py`
- `conftest.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" astroquery`).
