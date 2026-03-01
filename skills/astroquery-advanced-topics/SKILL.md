---
name: astroquery-advanced-topics
description: This skill consolidates long-tail one-doc Astroquery topics and routes users to narrow references without polluting primary routing skills.
---

# astroquery: Advanced Topics

## Scope
- Consolidated long-tail topics with narrow document coverage.
- Use this skill when requests are about project metadata, release notes, config/license docs, validator internals, or SIMBAD migration notes.

## Route the request
- Configuration defaults and config-file mechanics -> `docs/configuration.rst`.
- Changelog and historical release context -> `docs/changelog.rst`, `docs/release_notice_v0.2.rst`.
- License/legal text -> `docs/license.rst`.
- Template module scaffolding -> `docs/template.rst`, `astroquery/template_module`.
- Cone-search validator implementation details -> `docs/vo_conesearch/validator.rst`.
- SIMBAD migration/breaking-evolution details -> `docs/simbad/simbad_evolution.rst`.

## Workflow
- Start from the exact doc in `references/doc_map.md`.
- If behavior details are missing, jump to targeted source entry points in `references/source_map.md`.
- Keep answers narrow; do not expand into broad module usage unless asked.

## Primary documentation references
- `docs/configuration.rst`
- `docs/changelog.rst`
- `docs/license.rst`
- `docs/release_notice_v0.2.rst`
- `docs/template.rst`
- `docs/vo_conesearch/validator.rst`
- `docs/simbad/simbad_evolution.rst`

## Source entry points for unresolved issues
- `astroquery/query.py`
- `astroquery/exceptions.py`
- `astroquery/template_module/core.py`
- `astroquery/simbad/core.py`
- `astroquery/vo_conesearch/validator/validate.py`
- `astroquery/vo_conesearch/validator/tstquery.py`
- `astroquery/vo_conesearch/validator/inspect.py`
- Prefer targeted search: `rg -n "<symbol_or_keyword>" astroquery`.
