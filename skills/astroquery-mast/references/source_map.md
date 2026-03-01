# astroquery source map: Mast

Generated from source roots:
- `astroquery`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `query_object`
- `query_region`
- `query_criteria`
- `get_product_list`
- `download_products`
- `missions`
- `cutout`
- `cloud`

## Fast source navigation
- `rg -n "query_object_async|query_region_async|query_criteria_async|get_product_list|download_products" astroquery/mast/observations.py`
- `rg -n "class MastMissionsClass|query_.*|list_missions" astroquery/mast/missions.py`
- `rg -n "TesscutClass|ZcutClass|HapcutClass|download_cutouts|get_cutouts" astroquery/mast/cutouts.py`

## Suggested source entry points
- `astroquery/mast/observations.py` | observation query/count/product/download workflow implementation
- `astroquery/mast/missions.py` | mission-specific metadata and criteria query behavior
- `astroquery/mast/cutouts.py` | cutout request and retrieval logic for TESS/ZTF/HAP
- `astroquery/mast/services.py` | low-level service API request wrappers and response shaping
- `astroquery/mast/discovery_portal.py` | discovery portal query serialization/parsing path
- `astroquery/mast/core.py` | shared MAST query/login behavior (`MastQueryWithLogin`)
- `astroquery/mast/auth.py` | authentication/session token behavior
- `astroquery/mast/cloud.py` | cloud dataset enablement and cloud URI access paths
- `astroquery/mast/utils.py` | object resolution, batching, filter parsing, and product utilities
- `astroquery/mast/tests/test_mast_remote.py` | remote-data behavior checks for end-to-end workflows
