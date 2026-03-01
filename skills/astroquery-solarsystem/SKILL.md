---
name: astroquery-solarsystem
description: This skill routes and executes Solar System workflows across MPC, JPL Horizons/SBDB, and IMCCE services.
---

# astroquery: Solarsystem

## High-Signal Playbook
### Route conditions
- Use this skill when users ask Solar System object lookup/ephemerides/identification questions.
- Route by provider intent.
- MPC orbit/ephemeris/observatory services -> `astroquery.mpc`.
- JPL Horizons ephemerides/elements/vectors and SBDB object records -> `astroquery.jplhorizons` / `astroquery.jplsbdb`.
- IMCCE sky-field object identification and ephemerides -> `astroquery.imcce` (`Skybot`, `Miriade`).

### Triage questions
- Which service family is required (MPC, JPL, IMCCE)?
- Required output: orbit parameters, ephemerides table, vectors/elements, or field object identification?
- Is object identity ambiguous and does `id_type` need to be explicit?
- Which observer location convention is expected (MPC code, Horizons center, topocentric dict, IAU code)?
- Are epochs discrete list values or range (`start/stop/step`) values?
- Is query size large enough to require range-based requests instead of long explicit lists?

### Canonical workflow
1. Select provider from `docs/solarsystem/solarsystem.rst` and linked module docs.
2. Start with a minimal single-target query and explicit location/epoch.
3. If ambiguity appears, set service-specific identifier mode (`id_type`).
4. Expand to ranged ephemerides/elements using provider-native step syntax.
5. Validate units/columns in returned `Table`/`QTable` before downstream use.
6. Clear cache and retry if stale or inconsistent remote state is suspected.

### Minimal working example
```python
from astroquery.jplhorizons import Horizons
from astroquery.mpc import MPC

# JPL Horizons ephemerides over a range
obj = Horizons(
    id="Ceres",
    location="568",
    epochs={"start": "2010-01-01", "stop": "2010-01-03", "step": "1d"},
)
print(obj.ephemerides())

# MPC ephemeris at default geocenter
eph = MPC.get_ephemeris("24", step="1d", number=3)
print(eph)
```
- Backing docs: `docs/jplhorizons/jplhorizons.rst`, `docs/mpc/mpc.rst`.

### Pitfalls and fixes
- Horizons default id behavior changed: set `id_type` when target resolution is ambiguous (`docs/jplhorizons/jplhorizons.rst`).
- Horizons topocentric longitude sign gotcha on many prograde bodies (`docs/jplhorizons/jplhorizons.rst`).
- Oversized explicit epoch lists create very long URIs; switch to range dicts (`astroquery/jplhorizons/core.py`).
- MPC `step` must be day/hour/min/sec quantity (`astroquery/mpc/core.py`).
- Skybot cone radius >10 deg or positional error >120 arcsec gets clipped/warned (`astroquery/imcce/core.py`).
- Location array format errors: provide exactly `(lon, lat, alt)` where required (`astroquery/mpc/core.py`).

### Convergence and validation checks
- Returned tables have expected columns/units and non-empty rows.
- Epoch cadence in results matches requested `step`/range semantics.
- Location interpretation matches provider conventions (MPC code vs Horizons center syntax).
- Cross-check one target across providers when precision/consistency is critical.

## Scope
- Solar System service routing and query setup across MPC/JPL/IMCCE backends.

## Primary documentation references
- `docs/solarsystem/solarsystem.rst`
- `docs/solarsystem/mpc/mpc.rst`
- `docs/solarsystem/jpl/jpl.rst`
- `docs/solarsystem/imcce/imcce.rst`
- `docs/mpc/mpc.rst`
- `docs/jplhorizons/jplhorizons.rst`
- `docs/jplsbdb/jplsbdb.rst`
- `docs/imcce/imcce.rst`

## Workflow
- Start with provider docs first.
- Use `references/doc_map.md` for full inventory.
- Escalate to source via `references/source_map.md` only when behavior is unclear.

## Source entry points for unresolved issues
- `astroquery/jplhorizons/core.py`
- `astroquery/jplsbdb/core.py`
- `astroquery/mpc/core.py`
- `astroquery/imcce/core.py`
- `astroquery/jplhorizons/tests/test_jplhorizons_remote.py`
- `astroquery/jplsbdb/tests/test_jplsbdb_remote.py`
- `astroquery/mpc/tests/test_mpc_remote.py`
- `astroquery/imcce/tests/test_skybot_remote.py`
- `astroquery/imcce/tests/test_miriade_remote.py`
- Prefer targeted search: `rg -n "<symbol_or_keyword>" astroquery/jplhorizons astroquery/jplsbdb astroquery/mpc astroquery/imcce`.
