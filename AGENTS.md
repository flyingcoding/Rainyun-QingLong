# Repository Guidelines

## Project Structure & Module Organization

- `main.py`: entry point; runs sign-in flow, aggregates results, and sends QingLong notifications.
- `config.py`: reads `RAINYUN_CONFIG` (JSON) and provides defaults.
- `account_parser.py`: parses `RAINYUN_ACCOUNT` (multi-account JSON list).
- `api_client.py`: Rainyun API wrapper (used for points/renewal actions).
- `server_manager.py`: server expiry checks and optional auto-renew logic.
- `captcha.py`: login captcha handling (OCR + slider/image workflow).
- `stealth.min.js`: Selenium anti-detection script injected at runtime.
- `ql_cron.json`: QingLong cron template (e.g., `0 9 * * *` running `python3 main.py`).

## Build, Test, and Development Commands

This is a Python script repo (no build step). Common commands:

```bash
# QingLong container deps
apt update && apt install -y chromium-driver
pip3 install selenium opencv-python-headless ddddocr requests

# Smoke run (requires env vars)
RAINYUN_ACCOUNT='[["user@example.com","password","false"]]' python3 main.py

# Quick syntax check
python3 -m py_compile *.py
```

## Coding Style & Naming Conventions

- Python: 4-space indentation, PEP 8, `snake_case` for functions/vars, `PascalCase` for classes.
- Keep logs actionable for QingLong users (clear errors, no stack traces unless necessary).
- Avoid hardcoding secrets/URLs/paths; prefer `RAINYUN_CONFIG` and `config.py` defaults.

## Testing Guidelines

- No dedicated test suite yet. For changes, provide at least:
  - `python3 -m py_compile *.py` (syntax) and a local/QingLong smoke run.
- If adding new parsing/business logic, consider adding `pytest` tests under `tests/` using `test_*.py`.

## Commit & Pull Request Guidelines

- Follow the existing lightweight history: short, imperative commits like `Update README.md` / `Create ql_cron.json`.
- PRs should include: what changed, how it was validated (log snippet or steps), and any config/env var changes.
- If you add/rename config keys, update both `config.py` defaults and the docs in `README.md`.
- Never commit credentials, `.env`, `cookies.json`, logs, or downloaded images (see `.gitignore`).
