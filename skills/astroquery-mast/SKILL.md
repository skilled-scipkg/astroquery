---
name: astroquery-mast
description: This skill handles MAST observation, catalog, mission, and cutout workflows, with product retrieval and pagination/download pitfalls.
---

# astroquery: Mast

## High-Signal Playbook
### Route conditions
- Use this skill for MAST observation discovery, catalog lookups, mission-specific metadata, and cutout/download workflows.
- Route low-level generic TAP mechanics to `astroquery-api-and-scripting` only when needed.
- Route general troubleshooting (cache/auth reproducibility) to `astroquery-templates`.

### Triage questions
- Is the user querying observations, catalogs, mission metadata, or cutouts?
- Target input type: object name, coordinates, criteria filters, or direct IDs?
- Does the user need counts first or full tables immediately?
- Are downloads required, and if so single-file vs multi-product manifests?
- Are they using `obsid` (MAST product group id) vs mission `obs_id` correctly?
- Is cloud download expected (`enable_cloud_dataset`) or on-prem only?

### Canonical workflow
1. Choose interface (`Observations`, `Catalogs`, `MastMissions`, `Tesscut`/`Zcut`/`Hapcut`, or low-level `Mast`) from `docs/mast/mast.rst`.
2. Discover metadata/valid fields first (`list_missions`, `get_metadata`, service docs).
3. Run narrow query (`query_region`, `query_object`, or `query_criteria`) with explicit radius/criteria.
4. Use count endpoints before large pulls when sizing matters.
5. For data retrieval, call `get_product_list` (or unique variant) using `obsid` and tuned `batch_size`.
6. Filter products, then download and inspect returned manifest table.
7. For cutouts, use mission-specific class and explicit `sector`/`survey` when possible.
8. Escalate to `Mast.mast_query` / `service_request_async` for unwrapped services.

### Minimal working example
```python
from astroquery.mast import Observations

# Query observation metadata
obs = Observations.query_object("M8", radius="0.02 deg")

# Product discovery must use obsid-backed rows
products = Observations.get_product_list(obs[:1], batch_size=200)

# Filter then download a tiny subset
filtered = Observations.filter_products(products, productType="SCIENCE")
manifest = Observations.download_products(filtered[:2])
print(manifest)
```
- Backing docs: `docs/mast/mast.rst`, `docs/mast/mast_obsquery.rst`.

### Pitfalls and fixes
- `obs_id` vs `obsid` mix-up: product list methods require `obsid` (`docs/mast/mast_obsquery.rst`).
- Missing non-positional criteria: `query_criteria` requires at least one (`docs/mast/mast_obsquery.rst`, `astroquery/mast/observations.py`).
- Pagination confusion: use `pagesize`/`page` deliberately for large result sets (`astroquery/mast/observations.py`).
- TESSCut request throttling: >10 simultaneous requests can trigger `503` (`docs/mast/mast_cut.rst`).
- Wrong low-level service mode: TESS `.Rows` services are for tabular outputs, count semantics differ (`docs/mast/mast_mastquery.rst`).
- `cloud_only` without cloud access: enable cloud dataset first or expect fallback warnings (`astroquery/mast/observations.py`).

### Convergence and validation checks
- Query results have expected columns and plausible row counts.
- Count endpoints and full-query lengths are consistent with filters.
- Download manifest status/messages are reviewed for per-file failures.
- Cutout requests return expected number of sectors/surveys and local paths.

## Scope
- MAST data discovery and retrieval across observations, catalogs, missions, and cutouts.
- Keep responses operational and method-specific.

## Primary documentation references
- `docs/mast/mast.rst`
- `docs/mast/mast_obsquery.rst`
- `docs/mast/mast_catalog.rst`
- `docs/mast/mast_missions.rst`
- `docs/mast/mast_cut.rst`
- `docs/mast/mast_mastquery.rst`

## Workflow
- Start with docs above.
- Expand to `references/doc_map.md` for full MAST doc inventory.
- Inspect source via `references/source_map.md` only when docs leave ambiguity.

## Source entry points for unresolved issues
- `astroquery/mast/observations.py`
- `astroquery/mast/missions.py`
- `astroquery/mast/cutouts.py`
- `astroquery/mast/core.py`
- `astroquery/mast/services.py`
- `astroquery/mast/discovery_portal.py`
- `astroquery/mast/cloud.py`
- `astroquery/mast/auth.py`
- `astroquery/mast/utils.py`
- Prefer targeted search: `rg -n "<symbol_or_keyword>" astroquery/mast`.
