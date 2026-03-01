---
name: astroquery-index
description: This skill routes Astroquery requests to the correct topic skill and enforces docs-first escalation before source inspection.
---

# astroquery Skills Index

## Route the request
- Use this skill as a router only; do not answer deep implementation questions here.
- Route by user intent:
- `astroquery-getting-started`: install/upgrade, first query, base cache/config behavior.
- `astroquery-api-and-scripting`: shared API patterns, sync vs async queries, TAP/upload automation.
- `astroquery-examples-and-tutorials`: "show me a working command" requests and runnable snippets.
- `astroquery-mast`: Observation/Catalog/Missions/Cutout workflows for MAST.
- `astroquery-solarsystem`: MPC/JPL/IMCCE service routing and Solar System query setup.
- `astroquery-templates`: troubleshooting for cache/auth/remote-data reproducibility.
- `astroquery-inputs-and-modeling`: modeling/criteria-heavy input semantics not covered by core skills.
- `astroquery-simulation-workflows`: test/automation workflow orchestration questions.
- `astroquery-advanced-topics`: long-tail docs (configuration/changelog/license/release-note/template/validator/SIMBAD evolution).

## Service-family shortcuts
- MAST family (`Observations`, `Catalogs`, `MastMissions`, `Tesscut`, `Zcut`, `Hapcut`) -> `astroquery-mast`.
- Solar System family (`MPC`, `Horizons`, `SBDB`, `Skybot`, `Miriade`) -> `astroquery-solarsystem`.
- ESA TAP-family examples (Gaia/JWST/Hubble/Euclid/XMM/ISO/Integral) -> `astroquery-examples-and-tutorials` first, then `astroquery-api-and-scripting` for deeper TAP mechanics.

## Generated topic skills
- `astroquery-getting-started`
- `astroquery-api-and-scripting`
- `astroquery-examples-and-tutorials`
- `astroquery-mast`
- `astroquery-solarsystem`
- `astroquery-templates`
- `astroquery-inputs-and-modeling`
- `astroquery-simulation-workflows`
- `astroquery-advanced-topics`

## Documentation-first escalation
1. Start in the selected topic skill `Primary documentation references`.
2. If needed, expand to that skill's `references/doc_map.md`.
3. Inspect source only after docs are insufficient, starting from that skill's `references/source_map.md` entry points.
4. Use targeted lookup while in source: `rg -n "<symbol_or_keyword>" astroquery`.

## Documentation and source roots
- Docs root: `docs`
- Test guidance root: `docs/testing.rst`, `astroquery/tests`
- Source root: `astroquery`

## Long-tail consolidation policy
- Keep one-doc and low-traffic topics consolidated in `astroquery-advanced-topics` to avoid router noise.
- If additional narrow one-doc topics appear in later passes, merge them there and keep index routing stable.
