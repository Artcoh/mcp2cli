# mcp2cli — Agent Instructions

## Project Overview

`mcp2cli` turns any MCP server, OpenAPI spec, or GraphQL endpoint into a CLI at runtime — no codegen, no wrappers. It saves 96–99% of tokens wasted on tool schemas.

- **Source**: `src/mcp2cli/` (single `__init__.py` entry point)
- **Skills**: `skills/mcp2cli/SKILL.md` (installable AI agent skill)
- **Python**: 3.14 (see `.python-version`)
- **Package manager**: `uv`

## Development Setup

```bash
# Install all dependencies including test extras
uv sync --extra test

# Verify the CLI works
uv run mcp2cli --help
```

## Running Tests

```bash
# Run full test suite
uv run pytest tests/ -v

# Run a specific test file
uv run pytest tests/test_openapi.py -v

# Run with stdout for debugging
uv run pytest tests/test_token_savings.py -v -s
```

## Project Structure

```
src/mcp2cli/__init__.py   # All CLI logic lives here (single-file implementation)
tests/                    # pytest test suite
  conftest.py             # Shared fixtures (petstore spec, MCP servers, HTTP mocks)
  test_openapi.py         # OpenAPI mode tests
  test_mcp.py             # MCP HTTP/SSE tests
  test_cache.py           # Caching tests
  test_helpers.py         # Utility/helper tests
  test_bake.py            # Bake mode tests
  test_graphql.py         # GraphQL mode tests
  test_token_savings.py   # Token efficiency tests
  test_oauth.py           # OAuth tests
skills/mcp2cli/SKILL.md   # Installable skill for Claude Code / Cursor / Codex
```

## Key CLI Commands

```bash
# List tools from an MCP server
uv run mcp2cli --mcp https://mcp.example.com/sse --list

# Call an OpenAPI endpoint
uv run mcp2cli --spec ./openapi.json --base-url https://api.example.com list-pets

# GraphQL query
uv run mcp2cli --graphql https://api.example.com/graphql users --limit 10

# MCP stdio
uv run mcp2cli --mcp-stdio "npx @modelcontextprotocol/server-filesystem /tmp" --list
```

## Coding Conventions

- All CLI logic is in `src/mcp2cli/__init__.py` — keep changes surgical
- Tests use `pytest` and `pytest-asyncio`; fixtures are in `tests/conftest.py`
- No external linter config; follow the style of surrounding code
- Secrets in CLI args support `env:VAR` and `file:/path` prefixes — preserve this pattern
