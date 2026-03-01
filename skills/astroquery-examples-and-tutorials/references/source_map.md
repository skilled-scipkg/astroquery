# astroquery source map: Examples and Tutorials

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `example`
- `snippet`
- `query_object`
- `query_region`
- `launch_job_async`
- `get_product_list`
- `download_products`
- `cutout`

## Fast source navigation
- `rg -n "query_object|query_region|get_query_payload" astroquery/simbad/core.py astroquery/vizier/core.py astroquery/mast/observations.py`
- `rg -n "launch_job|launch_job_async|upload_resource" astroquery/utils/tap/core.py astroquery/gaia/core.py`
- `rg -n "download|retrieve|cutout" astroquery/mast/cutouts.py astroquery/esa/jwst/core.py astroquery/esa/hubble/core.py astroquery/esa/euclid/core.py astroquery/esa/xmm_newton/core.py`

## Suggested source entry points
- `astroquery/simbad/core.py` | object and region example behavior (`query_object`, `query_region`, `query_tap`)
- `astroquery/vizier/core.py` | catalog/object/region query example behavior
- `astroquery/mast/observations.py` | observation query -> product list -> download example path
- `astroquery/mast/cutouts.py` | TESS/HAP/ZTF cutout request and artifact workflow
- `astroquery/gaia/core.py` | Gaia TAP+ example behavior and job execution wrappers
- `astroquery/utils/tap/core.py` | shared TAP execution patterns used in many tutorial snippets
- `astroquery/esa/jwst/core.py` | JWST archive example behavior and retrieval methods
- `astroquery/esa/hubble/core.py` | Hubble archive query/download examples
- `astroquery/esa/euclid/core.py` | Euclid query and authenticated example behavior
- `astroquery/esa/xmm_newton/core.py` | XMM query example behavior and mission-specific options
- `astroquery/query.py` | cross-service cache/login mechanics that affect tutorial reproducibility
