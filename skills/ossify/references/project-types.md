# Project types

Detect by the files present. A repo can match more than one type; use all of their version files.

| Type | Detect | Version files | Distribution paths | Validate |
|---|---|---|---|---|
| Claude Code plugin / skill | `.claude-plugin/plugin.json` or `skills/*/SKILL.md` | `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` (`metadata.version`) | `skills/`, `agents/`, `commands/`, `hooks/`, `.claude-plugin/` | `claude plugin validate .` |
| npm | `package.json` | `package.json` (`npm version --no-git-tag-version <bump>`) | `src/`, `bin/`, `lib/`, `package.json` deps | `npm test`, `npm run build` if defined |
| Python | `pyproject.toml` | `pyproject.toml` `[project] version`, `__version__` if present | `src/`, the package dir, dependencies | `pytest` if tests exist |
| Rust | `Cargo.toml` | `Cargo.toml` `[package] version` | `src/`, `Cargo.toml` deps | `cargo test` |
| Go | `go.mod` | git tag only | `*.go`, `go.mod` | `go test ./...` |
| Swift package | `Package.swift` | git tag only | `Sources/` | `swift build` |
| Codex plugin | `.codex-plugin/plugin.json` | that file | same as Claude plugin | none |
| Generic | none of the above | `VERSION` file if present, otherwise git tag only | everything except docs | none |

Docs-only paths for every type: `README*`, `AGENTS.md`, `CLAUDE.md`, `docs/`, `assets/` images, `CHANGELOG.md` edits outside a release, `LICENSE`, comments.

## .gitignore starters

- every type: `.DS_Store`, `.env`, `.env.*` (but not `.env.example`), editor folders
- npm: `node_modules/`, `dist/`
- Python: `__pycache__/`, `.venv/`, `*.egg-info/`, `dist/`
- Rust: `target/`
