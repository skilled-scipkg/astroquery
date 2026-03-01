---
name: astroquery-getting-started
description: This skill handles first-week Astroquery setup and first successful queries, with docs-first routing and source fallback links.
---

# astroquery: Getting Started

## High-Signal Playbook
### Route conditions
- Use this skill for installation, first import/query, and baseline cache/config setup.
- Route MAST-specific workflows to `astroquery-mast`.
- Route TAP automation and async job pipelines to `astroquery-api-and-scripting`.
- Route troubleshooting (auth/cache/test reproducibility) to `astroquery-templates`.

### Triage questions
- Which package channel is required (`pip --pre` dev release vs conda tagged release)?
- Is the user starting from a clean environment, source checkout, or editable install?
- Which first service should be used for sanity checks (`Simbad`, `Vizier`, etc.)?
- Is the request name-based (`query_object`) or coordinate-based (`query_region`)?
- Are stale results suspected (cache) or are calls failing live (network/service)?
- Does the user need optional dependencies (`[all]`, `[test]`, `[docs]`)?

### Canonical workflow
1. Install/upgrade (`docs/index.rst`, `README.rst`):
   - `python -m pip install -U --pre astroquery`
   - `python -m pip install -U --pre astroquery[all]`
   - `conda install -c conda-forge astroquery`
2. Verify service-specific import (not just `import astroquery`).
3. Run a name-based smoke test with `query_object` (`docs/index.rst`).
4. Run a coordinate-based smoke test with `query_region` (`docs/index.rst`).
5. Check per-service cache path and clear if stale (`docs/index.rst`).
6. Adjust global cache knobs through `astroquery.cache_conf` (`docs/index.rst`).

### Minimal working example
```python
from astropy import coordinates as coord
import astropy.units as u
from astroquery.simbad import Simbad
from astroquery import cache_conf

# 1) Name-based query
obj = Simbad.query_object("m1")
print(len(obj), obj.colnames[:5])

# 2) Coordinate-based query
c = coord.SkyCoord("05h35m17.3s -05d23m28s", frame="icrs")
reg = Simbad.query_region(c, radius=5 * u.arcmin)
print(len(reg))

# 3) Cache controls
print(cache_conf.cache_active, cache_conf.cache_timeout)
Simbad.clear_cache()
```
- Backing docs: `docs/index.rst`, `README.rst`, `docs/configuration.rst`.

### Pitfalls and fixes
- Missing latest continuous release: use `--pre` with `-U` (`docs/index.rst`, `README.rst`).
- Importing top-level package only: import concrete service modules (for example `astroquery.simbad`).
- Stale responses after upstream data updates: clear service cache (`docs/index.rst`).
- Confusing service-specific and global cache settings: check both `Service.cache_location` and `cache_conf` (`docs/index.rst`).
- First query fails on networked docs examples: many examples are remote-data dependent.

### Convergence and validation checks
- First `query_object` and `query_region` both return non-empty `astropy.table.Table` outputs.
- Reported cache path exists and `clear_cache()` empties cached files.
- `cache_conf.cache_active` and `cache_conf.cache_timeout` reflect intended runtime defaults.
- Installed version/channel matches expected behavior (`--pre` vs stable tag).

## Scope
- Initial setup, quickstarts, and first successful query workflows.
- Keep guidance compact and procedural; escalate module details to topic skills.

## Primary documentation references
- `docs/index.rst`
- `README.rst`
- `docs/configuration.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md`.
- If ambiguity remains after docs, inspect `references/source_map.md`.
- Cite exact file paths in responses.

## Source entry points for unresolved issues
- `astroquery/query.py`
- `astroquery/simbad/core.py`
- `astroquery/vizier/core.py`
- `astroquery/utils/class_or_instance.py`
- `astroquery/utils/commons.py`
- `astroquery/exceptions.py`
- Prefer targeted search: `rg -n "<symbol_or_keyword>" astroquery`.
