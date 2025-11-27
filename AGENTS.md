# Repository Guidelines

Kosong is an LLM abstraction layer for modern agent applications. Use this guide to keep contributions consistent and easy to review.

## Project Structure & Module Organization
- `src/kosong/_generate.py` holds the core generate/step orchestration and is re-exported via `src/kosong/__init__.py`.
- `src/kosong/message.py` defines message structures; `src/kosong/chat_provider/` contains provider adapters (Kimi, chaos testing, mock), and `src/kosong/tooling/` provides tool abstractions plus the `SimpleToolset`.
- `src/kosong/utils/` contains async helpers and JSON Schema utilities; `src/kosong/contrib/` is for optional integrations; `src/kosong/__main__.py` backs the demo CLI.
- Tests live in `tests/` and mirror the modules above; doctests run on modules that include examples.

## Build, Test, and Development Commands
- `make format`: `uv run ruff check --fix` then `uv run ruff format`.
- `make check`: lint plus type-check (`uv run ruff check`, `uv run ruff format --check`, `uv run pyright`).
- `make test`: `uv run pytest --doctest-modules -vv`; target a node with `uv run pytest tests/test_generate.py::test_generate_stream`.
- Demo CLI: `uv run python -m kosong kimi --with-bash` (requires `KIMI_BASE_URL` and `KIMI_API_KEY`).

## Coding Style & Naming Conventions
- Ruff formatter with line length 100; use 4-space indentation and avoid hand-formatting.
- Functions/variables use `snake_case`, classes use `PascalCase`, modules stay lower_snake; keep tool names short verbs (`add`, `search_files`).
- Prefer full type hints; pyright is enforced. Use Pydantic models for structured payloads and `TypedDict` when lighter weight is sufficient.
- Keep module-level side effects minimal and colocate provider-specific constants with their adapter.

## Testing Guidelines
- Add or extend tests under `tests/test_*.py` matching the module under change; mark async cases with `@pytest.mark.asyncio`.
- Use representative chat histories and tool payloads; keep external calls mocked (see `chat_provider.mock` or `SimpleToolset`).
- Run `make test` before opening a PR; small fixups can rely on targeted pytest node runs.

## Commit & Pull Request Guidelines
- Use concise, present-tense subjects; conventional prefixes (`feat:`, `chore:`, `fix:`) align with existing history.
- Commit logical units that include code, tests, and relevant docs. Avoid mixing formatting-only churn with behavior changes.
- PRs should summarize motivation, behavior changes, tests run, and any new env vars or provider setup; link issues when applicable and include screenshots for demo-facing tweaks.
- Call out breaking API changes clearly and update README snippets or docstrings alongside the code.

## Configuration & Security
- Keep API keys and endpoints out of source control; load via env vars such as `KIMI_API_KEY` and `KIMI_BASE_URL` or a local `.env`.
- When adding providers, isolate credentials to configuration arguments, avoid hardcoded endpoints, and document required env vars in the PR description.
