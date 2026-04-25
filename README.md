# Hermes Agent Kanban

Hermes Agent Kanban is a focused dashboard extension that adds a native Kanban tab to the Hermes Agent Web UI. It gives agent work a lightweight board with persistent cards, start/stop controls, status columns, logs, and git diffs.

This repository is intentionally standalone. It is not a mirror of `nousresearch/hermes-agent`; it contains only the Kanban-specific source excerpts, tests, documentation, and a patch bundle that can be applied to Hermes Agent.

## What Is Included

- `backend/hermes_cli/kanban.py`: FastAPI routes and persistent board storage for Kanban cards.
- `frontend/web/src/pages/KanbanPage.tsx`: Hermes-styled React page for the dashboard tab.
- `frontend/web/src/lib/api.ts`: dashboard API client types and Kanban request helpers.
- `tests/test_kanban.py`: focused backend tests for card storage, validation, process handling, and git diff/status behavior.
- `docs/kanban.md`: user-facing Kanban documentation for the Hermes docs site.
- `patches/hermes-agent-kanban.patch`: complete patch against current Hermes Agent `main`.

## Upstream PR

The integration PR is open here:

https://github.com/NousResearch/hermes-agent/pull/15719

That PR is the authoritative integration target. This repository is the lightweight companion project for reviewing and reusing the Kanban work without carrying the entire Hermes Agent repository.

## Apply The Patch

From a clean checkout of Hermes Agent:

```bash
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
git apply ../hermes-agent-kanban/patches/hermes-agent-kanban.patch
```

Then install Hermes Agent dependencies and run the focused checks:

```bash
scripts/run_tests.sh tests/test_kanban.py tests/test_hermes_state.py tests/hermes_cli/test_web_server.py tests/hermes_cli/test_pty_bridge.py tests/cron/test_jobs.py tests/cron/test_scheduler.py
npm --prefix web run build
```

## Feature Summary

- Create cards with title, prompt, model override, and workspace path.
- Move cards through Backlog, Running, Review, Done, and Trash.
- Start a card as a Hermes CLI chat task using the card prompt and selected model.
- Stop running card processes from the dashboard.
- View per-card logs and workspace git diffs.
- Persist board state under the active Hermes home directory.
- Match the visual language and navigation conventions of the Hermes Web UI.

## License

MIT
