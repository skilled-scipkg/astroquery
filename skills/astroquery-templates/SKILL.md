---
name: astroquery-templates
description: This skill is the Astroquery troubleshooting template for cache, authentication/keyring, and remote-data reproducibility issues.
---

# astroquery: Templates

## High-Signal Playbook
### Route conditions
- Use this skill for failure triage, reproducibility checks, cache invalidation, and authentication/keyring issues.
- Route service-specific usage questions back to the corresponding module skill after triage.

### Triage questions
- Is the failure stale-result/cache related, auth related, or remote-service/network related?
- Which service class/method is failing, and does it require login?
- Is the issue reproducible with cache disabled?
- Are warnings/exceptions from `astroquery.exceptions` being captured?
- Is this a local test failure, remote-data-only failure, or mixed?
- Is keyring available/healthy in the current environment?

### Canonical workflow
1. Reproduce with the smallest query that still fails.
2. Inspect and reset cache state:
   - service cache path (`Service.cache_location` / `Service.clear_cache()`)
   - global cache settings via `astroquery.cache_conf`
3. Retry with cache disabled (`cache_conf.set_temp("cache_active", False)`) to separate stale cache vs live service failure.
4. For auth paths, retry login explicitly and confirm authenticated state/session info where supported.
5. If keyring fails, handle fallback prompt flow and/or explicit credentials inputs.
6. For tests, separate local monkeypatched tests from `remote_data` tests and run with proper flags.
7. Escalate to source entry points for transport/parsing/login internals.

### Minimal working example
```python
from astroquery import cache_conf
from astroquery.simbad import Simbad

# Force live call to compare against cached behavior
with cache_conf.set_temp("cache_active", False):
    live = Simbad.query_object("m1")

# Service-level cache cleanup
Simbad.clear_cache()
print(len(live))
```
- Backing docs: `docs/index.rst` (cache), `docs/testing.rst` (remote-data testing).

### Pitfalls and fixes
- Stale cached payload interpreted as current data: clear service cache and retry (`docs/index.rst`, `astroquery/query.py`).
- Login credentials accidentally cached: login wrapper disables cache by design (`astroquery/query.py`).
- Keyring backend failures: `QueryWithLogin` falls back to prompt and logs warnings (`astroquery/query.py`).
- Misclassified remote tests: ensure `pytest.mark.remote_data` and run with `--remote-data=any` (`docs/testing.rst`).
- Hidden remote variability: prefer deterministic, narrow test queries and assert on schema/shape, not exact row values (`docs/testing.rst`).

### Convergence and validation checks
- Same request with cache off/on behaves as expected (difference only when stale cache existed).
- Authentication state is explicitly verified after login where API supports it.
- Failure class is identified (`TimeoutError`, `RemoteServiceError`, `InvalidQueryError`, warnings).
- Remote-data tests and local tests are isolated and reproducible independently.

## Scope
- Troubleshooting templates for cache/auth/test reproducibility across Astroquery services.

## Primary documentation references
- `docs/index.rst`
- `docs/testing.rst`
- `docs/configuration.rst`

## Workflow
- Start with docs first, then inspect source internals only for unresolved failures.
- Keep all recommendations reproducible and minimal.

## Source entry points for unresolved issues
- `astroquery/query.py`
- `astroquery/exceptions.py`
- `astroquery/mast/auth.py`
- `astroquery/mast/core.py`
- `astroquery/jplhorizons/core.py`
- `astroquery/utils/system_tools.py`
- `conftest.py`
- Prefer targeted search: `rg -n "<symbol_or_keyword>" astroquery`.
