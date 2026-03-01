---
name: astroquery-examples-and-tutorials
description: This skill curates minimal runnable Astroquery examples, mapped to module docs and annotated for remote-data/auth requirements.
---

# astroquery: Examples and Tutorials

## High-Signal Playbook
### Route conditions
- Use this skill when users ask for runnable snippets or "quickest working command" flows.
- Route deep API mechanics to `astroquery-api-and-scripting`.
- Route troubleshooting-heavy cases to `astroquery-templates`.
- Route MAST- or SolarSystem-heavy asks to `astroquery-mast` / `astroquery-solarsystem`.

### Triage questions
- Which service should the first working example target (Simbad, MAST, Gaia, ESA archive)?
- Is remote-data access available in the execution environment?
- Is authentication available/required for the requested operation?
- Is the user asking query-only or query+download workflows?
- Do they need in-memory result handling or on-disk artifacts?

### Canonical workflow
1. Start from a minimal query that returns a small table (`docs/index.rst`, `docs/simbad/simbad.rst`).
2. Add one practical workflow step (product list, async job, or cutout) from module docs.
3. Keep coordinates/radius/result size intentionally small.
4. Tag each example with runtime requirements (`remote-data`, `auth-required`, `large-download`).
5. Map every snippet to the exact docs page for expansion.

### Minimal working examples
```python
# Example A: SIMBAD name + region (remote-data)
from astropy.coordinates import SkyCoord
import astropy.units as u
from astroquery.simbad import Simbad

obj = Simbad.query_object("m1")
nearby = Simbad.query_region(SkyCoord("05h35m17.3s -05d23m28s"), radius=5 * u.arcmin)
print(len(obj), len(nearby))
```
- Docs map: `docs/index.rst`, `docs/simbad/simbad.rst`.

```python
# Example B: MAST observations -> products (remote-data)
from astroquery.mast import Observations

obs = Observations.query_object("M8", radius="0.02 deg")
products = Observations.get_product_list(obs[:1])
print(len(obs), len(products))
```
- Docs map: `docs/mast/mast_obsquery.rst`.

```python
# Example C: TAP async query (remote-data)
from astroquery.utils.tap.core import TapPlus

tap = TapPlus(url="https://gea.esac.esa.int/tap-server/tap")
job = tap.launch_job_async("select top 5 source_id, ra, dec from gaiadr3.gaia_source")
print(job.get_results())
```
- Docs map: `docs/utils/tap.rst`, `docs/gaia/gaia.rst`.

### Runtime requirement flags
- `remote-data`: nearly all examples in this skill.
- `auth-required`: TAP user-space persistence, proprietary archive downloads, and credentialed ESA archive operations (`docs/gaia/gaia.rst`, `docs/esa/xmm_newton/xmm_newton.rst`, `docs/esa/jwst/jwst.rst`).
- `large-download`: Euclid/JWST/Hubble/XMM data-product examples can be large (`docs/esa/euclid/euclid.rst`, `docs/esa/hubble/hubble.rst`, `docs/esa/xmm_newton/xmm_newton.rst`).

### Pitfalls and fixes
- Example copied without remote connectivity: mark and test with remote-data expectations.
- Overly broad radius defaults: force explicit small radii for first pass.
- Hidden row limits in TAP-backed services: set/inspect row-limit settings before validating counts.
- Download examples stalling: split query-only and download steps, validate metadata first.
- Auth-only examples failing anonymously: downgrade to public-mode equivalent query first.

### Convergence and validation checks
- Example returns expected object type (`Table`, `TableList`, `HDUList`, or manifest).
- Output row count is plausible and stable on immediate rerun.
- Auth-tagged examples are clearly separated from public examples.
- Each snippet includes at least one direct module-doc path for expansion.

## Scope
- Curated runnable examples across high-traffic Astroquery services.
- Prioritize short, composable snippets with explicit runtime requirements.

## Primary documentation references
- `docs/index.rst`
- `docs/simbad/simbad.rst`
- `docs/vizier/vizier.rst`
- `docs/mast/mast_obsquery.rst`
- `docs/gaia/gaia.rst`
- `docs/utils/tap.rst`
- `docs/esa/jwst/jwst.rst`
- `docs/esa/hubble/hubble.rst`
- `docs/esa/euclid/euclid.rst`
- `docs/esa/xmm_newton/xmm_newton.rst`

## Workflow
- Start with docs examples and prefer smallest runnable variants.
- Use `references/doc_map.md` to locate additional service examples.
- Inspect source only when behavior differs from docs examples.

## Source entry points for unresolved issues
- `astroquery/simbad/core.py`
- `astroquery/vizier/core.py`
- `astroquery/mast/observations.py`
- `astroquery/mast/cutouts.py`
- `astroquery/gaia/core.py`
- `astroquery/utils/tap/core.py`
- `astroquery/esa/jwst/core.py`
- `astroquery/esa/hubble/core.py`
- `astroquery/esa/euclid/core.py`
- `astroquery/esa/xmm_newton/core.py`
- Prefer targeted search: `rg -n "<symbol_or_keyword>" astroquery`.
