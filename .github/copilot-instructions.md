# Copilot Coding Agent Instructions

## Mandatory Development Checklist
Run all three before handing off changes:
- [ ] Lint: `uv run ruff check .`
- [ ] Build: `uv sync`
- [ ] Test: `uv run pytest`

## Project Snapshot
Soc Ops is a FastAPI + Jinja2 + HTMX social bingo app. The backend returns rendered HTML fragments, and HTMX swaps them into the page without full reloads.

## Architecture (What talks to what)
```
app/
├── main.py           # FastAPI routes + SessionMiddleware + template responses
├── game_service.py   # GameSession state transitions + in-memory session store
├── game_logic.py     # Pure bingo logic (board generation, toggles, bingo checks)
├── models.py         # Frozen Pydantic models + GameState enum
├── data.py           # Question bank + FREE_SPACE label
├── templates/        # Jinja2 screens and HTMX partials
└── static/           # Utility CSS and bundled htmx.min.js

tests/
├── test_api.py       # Endpoint/HTML behavior with TestClient
└── test_game_logic.py# Pure logic invariants and edge cases
```

## Request/State Flow
1. Browser posts to an endpoint in `app/main.py`.
2. Route calls `_get_game_session(request)` to retrieve a `GameSession` by signed cookie session ID.
3. Route calls a `GameSession` method (`start_game`, `handle_square_click`, `reset_game`, `dismiss_modal`).
4. Route returns `TemplateResponse` with updated `session`.
5. HTMX swaps `#game-container` via `hx-swap="outerHTML"`.

## Code Conventions That Matter Here
- Keep route handlers thin; put state transitions in `GameSession` and pure rules in `game_logic.py`.
- Endpoints use `response_class=HTMLResponse` and `templates.TemplateResponse(request, template, context)`.
- Always include `session` in template context; include `GameState` where templates branch on state.
- `game_logic.py` functions are immutable: return new values, never mutate inputs.
- Update frozen models with `.model_copy(update={...})`.
- Center square is free space (`CENTER_INDEX = 12`): starts marked and is never toggled.

## HTMX Pattern (Do this consistently)
```html
<button
  hx-post="/toggle/{{ square.id }}"
  hx-target="#game-container"
  hx-swap="outerHTML">
```
- Free-space square has no `hx-post` and is rendered disabled.
- Main swap target is `#game-container` in `templates/components/game_screen.html`.

## Where to Change What
- New game action: `app/game_service.py` method → `app/main.py` POST route → template trigger (`hx-post`) → tests.
- New/changed bingo rule: `app/game_logic.py` + `tests/test_game_logic.py`.
- New UI partial: `app/templates/components/*.html` and include from parent template.
- Styling changes: reuse/add utility classes in `app/static/css/app.css`.

## Testing Patterns
- Logic tests assert invariants (board size, free center, immutability, winning lines).
- API tests use `TestClient(app)` and typically call `client.get("/")` first to establish session cookie before POST actions.
- HTML assertions are expected (text snippets and HTMX attributes), not JSON contract assertions.

## Local Development
```bash
uv sync
uv run uvicorn app.main:app --reload --port 8000
uv run pytest
uv run ruff check .
```
Use a full external browser for manual testing; do not use VS Code Simple Browser (HTMX behavior is unreliable there).

## Non-Obvious Constraints
- Session storage is intentionally in-memory (`_sessions` dict): restarting server resets games.
- Session cookie is signed by `SessionMiddleware`; keep the flow intact when touching session logic.
- Ruff target is Python 3.13 (`pyproject.toml`), so maintain 3.13-compatible typing/style.
