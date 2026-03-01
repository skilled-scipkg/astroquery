# astroquery source map: Solarsystem

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `ephemerides`
- `elements`
- `vectors`
- `id_type`
- `topocentric`
- `mpc`
- `sbdb`
- `skybot`
- `miriade`

## Fast source navigation
- `rg -n "ephemerides_async|elements_async|vectors_async|id_type" astroquery/jplhorizons/core.py`
- `rg -n "query_object_async|get_ephemeris|id_type" astroquery/mpc/core.py astroquery/jplsbdb/core.py`
- `rg -n "Skybot|Miriade|query_region|query_.*" astroquery/imcce/core.py`

## Suggested source entry points
- `astroquery/jplhorizons/core.py` | Horizons query assembly, identifier resolution, and ephemerides/elements/vectors parsing
- `astroquery/jplsbdb/core.py` | SBDB object lookup behavior and parsed result structure
- `astroquery/mpc/core.py` | MPC object/ephemeris interfaces and input validation (`step`, `location`, `id_type`)
- `astroquery/imcce/core.py` | Skybot/Miriade query parameter handling and result parsing
- `astroquery/jplhorizons/tests/test_jplhorizons_remote.py` | remote behavior checks for Horizons endpoint interactions
- `astroquery/jplsbdb/tests/test_jplsbdb_remote.py` | remote SBDB regression checks
- `astroquery/mpc/tests/test_mpc_remote.py` | remote MPC behavior checks for ephemerides and target modes
- `astroquery/imcce/tests/test_skybot_remote.py` | remote Skybot behavior checks
- `astroquery/imcce/tests/test_miriade_remote.py` | remote Miriade behavior checks
