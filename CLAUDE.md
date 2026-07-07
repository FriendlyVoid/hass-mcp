# CLAUDE.md — hass-mcp (fork)

## What this is

A fork of [voska/hass-mcp](https://github.com/voska/hass-mcp) — a Home
Assistant MCP server — extended with **automation-config CRUD tools** (read /
create-or-replace / delete the full automation YAML via HA's
`/api/config/automation/config/{id}` REST endpoints, plus `reload_automations`)
and a **`call_api` generic REST passthrough** (escape hatch for any HA endpoint
without a dedicated tool). See the fork notice at the top of `README.md` and
`PATCHES.md` for the exact delta.

## Key rules (repo-specific)

- **This fork is installed from git via uvx, not PyPI/Docker Hub** — upstream's
  Docker Hub / PyPI / release publish workflows were deliberately removed
  (tests still run via `test.yml`). Don't re-add publish workflows here; the
  wrapper image repo ([FriendlyVoid/hass-mcp-http](https://github.com/FriendlyVoid/hass-mcp-http))
  handles serving this over HTTP.
- **Keep the fork delta small and documented** — new tools go in
  `app/server.py` with helpers in `app/hass.py` (matching the existing CRUD
  pattern); update the README fork notice and `PATCHES.md` when the tool set
  changes.
- **Secrets:** `HA_URL` / `HA_TOKEN` come from the environment at runtime.
  `.env.example` carries placeholders only; never commit real env files
  (`.gitignore` blocks `*.env`; gitleaks CI enforces).

## Conventions

Repo-specific rules live here; the fork otherwise follows the maintainer's
shared cross-repo conventions (gitignore baseline, gitleaks CI, PATCHES.md
delta tracking for forks).
