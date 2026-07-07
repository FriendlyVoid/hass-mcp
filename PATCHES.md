# PATCHES.md — delta vs upstream

This file records what this fork changes relative to upstream, and which
upstream base the changes sit on.

## Upstream

- Upstream project: `voska/hass-mcp` (https://github.com/voska/hass-mcp);
  `upstream` remote is configured in this clone.
- Upstream base: `7879b22` ("fix(docker): multi-stage build so hatch-vcs can
  read .git") — merge-base of `main` with `upstream/master`.

## Fork-local changes

From `git log upstream/master..HEAD` and the README fork notice:

### Added MCP tools

1. **Automation-config CRUD** (`34d6d47` helpers in `app/hass.py`, `1fab76b`
   tools in `app/server.py`, merged via PR #1), using HA's
   `/api/config/automation/config/{id}` REST endpoints:
   - `get_automation_config(automation_id)` — fetch the full automation config
   - `upsert_automation_config(automation_id, config)` — create or replace an
     automation (auto-reloads)
   - `delete_automation_config(automation_id)` — delete an automation
     (auto-reloads)
   - `reload_automations()` — reload automations from disk (wrapper around the
     existing service call)
2. **`call_api(method, path, body, params)`** (`b959d8c` helper, `53af5c6`
   tool) — generic REST passthrough; escape hatch for any HA endpoint not
   covered by a dedicated tool.

### Fixes

3. `e6c59ab` — relax `reload_automations` return type from `Dict` to `Any`.

### Publishing / CI

4. Removed upstream's Docker Hub publish (`eda8336`), PyPI publish
   (`e9c71bf`), and release (`1c1e61c`) workflows — this fork is installed
   from git via uvx and doesn't publish to those registries. `test.yml`
   remains.

### Docs

5. `862945b`, `4656526` — README fork notice documenting the added tools.

*(Plus fork housekeeping, not upstream-relevant: `CLAUDE.md`, `PATCHES.md`,
`.gitleaks.toml`, gitleaks CI workflow, `.gitignore` env-file hardening.)*

## Updating

`git fetch upstream && git merge upstream/master`, then update the upstream
base SHA above. Watch `app/server.py` / `app/hass.py` for conflicts with the
added tools.
