# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spendly is a Flask-based personal expense tracker, built incrementally as a step-by-step learning project. Many routes and the database layer are intentionally unimplemented placeholders (see "Incomplete / placeholder areas" below) — do not assume functionality exists just because a route or file is present.

## Commands

```bash
# Activate the virtual environment (already created at ./venv)
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run the app (serves on http://127.0.0.1:5001, debug mode on)
python app.py

# Run tests
pytest
pytest path/to/test_file.py::test_name   # run a single test
```

There is no build step or frontend bundler — templates and static assets are served directly by Flask.

## Architecture

- **`app.py`** — single-file Flask app containing all routes. No blueprints, no app factory pattern. New routes are added directly here.
- **`database/db.py`** — intended to hold `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (creates tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample dev data). Not yet implemented as of the current state — only the docstring/spec comment exists.
- **`templates/`** — Jinja2 templates. `base.html` is the shared layout (nav, footer, font links, block structure: `title`, `head`, `content`, `scripts`). All pages extend `base.html` via `{% extends "base.html" %}` and fill the `content` block. Route URLs in templates use `url_for('endpoint_name')`, never hardcoded paths.
- **`static/css/style.css`** — all styling lives in this single stylesheet (no CSS framework).
- **`static/js/main.js`** — currently empty/placeholder; JS is added here as features require it.
- **SQLite** is the intended database (per `database/db.py`'s spec comment and `.gitignore` entry for `expense_tracker.db`), accessed via the stdlib `sqlite3` module — no ORM is in use.

## Incomplete / placeholder areas

The codebase is being built in numbered steps; several pieces are stubs and should be treated as such rather than assumed-working:

- `database/db.py` has no implementation yet (Step 1).
- `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete` in `app.py` return plain placehold er strings ("coming in Step N"), not real views (Steps 3, 4, 7, 8, 9).
- `login.html` and `register.html` POST to `/login` and `/register`, but `app.py` only defines `GET` handlers for those routes — form submission (auth logic) has not been wired up yet.
- No `tests/` directory exists yet, despite `pytest` and `pytest-flask` being in `requirements.txt`.

When implementing one of these, follow the existing conventions already established elsewhere in the file/templates rather than introducing new patterns (e.g. new routes should match the plain-function, `render_template`-based style already used for `landing`/`login`/`register`).
