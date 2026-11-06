---
name: ossify
description: Set up and maintain the open-source conventions of a repo. Creates AGENTS.md with a CLAUDE.md symlink holding commit, push, and release rules; writes English and Korean READMEs with a language switch and an optional generated banner; and cuts releases with version bumps, CHANGELOG, git tags, and GitHub releases. Use when the user says "오픈소스로 만들 준비", "AGENTS.md 만들어줘", "CLAUDE.md 심볼릭 링크", "릴리스 해줘", "태그 달아줘", "README 써줘", "make this repo open-source ready", "cut a release", or starts a new public repo.
argument-hint: "[init | adopt | readme | release] [path]"
---

# ossify

Three conventions, applied the same way in every repo:

1. **Agent rules**: `AGENTS.md` is the single source of truth; `CLAUDE.md` is a symlink to it. It tells every agent to commit and push after every change and to release only when what users install changes.
2. **Releases**: semantic version bump in every version file, a CHANGELOG entry, an annotated tag, and a GitHub release.
3. **READMEs**: `README.md` in English and `README.ko.md` in Korean, linked to each other, kept in sync, written to a fixed structure.

## Modes

Infer the mode. Do not ask.

| Situation | Mode |
|---|---|
| empty or new folder, "새 레포", "init" | **init** |
| existing repo without these conventions | **adopt** |
| "README 써줘 / 고쳐줘" | **readme** |
| "릴리스", "태그", "배포", or a finished distribution change in a repo that already has `AGENTS.md` | **release** |

## Step 1. Inspect the repo

Find, without changing anything:

- **project type and version files**: see `references/project-types.md` (Claude Code plugin, npm, Python, Rust, Go, Swift, generic)
- **distribution paths**: the files whose change alters what users install
- existing `AGENTS.md`, `CLAUDE.md`, `README*`, `CHANGELOG.md`, `LICENSE`
- git state: `git status --short --branch`, remote, latest tag (`git describe --tags --abbrev=0`)
- GitHub login: `gh api user --jq .login`

## Step 2. Agent rules (init, adopt)

Write `AGENTS.md` from `references/agents-template.md`, filled with the real layout, version files, distribution paths, and validation command for this repo. Then link:

```bash
ln -s AGENTS.md CLAUDE.md
```

Existing files are never overwritten blindly:
- `CLAUDE.md` exists as a regular file: move its project-specific content into `AGENTS.md` (keep every rule, drop duplicates), then replace it with the symlink.
- `AGENTS.md` exists: merge the ossify sections into it; keep the owner's rules.
- Both exist and differ: merge both into `AGENTS.md`, then link.

Report every merge in one line each.

## Step 3. READMEs (init, adopt, readme)

Follow `references/readme-rules.md`. In short: English `README.md` and Korean `README.ko.md`, same structure, language switch under the banner, real examples only, install commands copied from what actually works.

Banner: if `assets/banner.png` does not exist, generate one in the background at the start of this step (`references/readme-rules.md#banner`). Look at it before committing. Skip silently only if no image tool is available, and say so in the report.

## Step 4. Base files (init, adopt)

Add only what is missing: `LICENSE` (MIT unless told otherwise, holder = GitHub login, current year), `CHANGELOG.md` with an `Unreleased` section, `.gitignore` for the project type.

## Step 5. Privacy scan (before any first push)

Grep for employer and internal project names, emails, home paths, tokens, and anything from the user's private instructions. Zero hits before the first public commit.

## Step 6. Commit and push

One logical change per commit, imperative subject, the session's attribution line if one is configured. Push right away. If there is no remote and the user asked to publish, create it: `gh repo create <owner>/<name> --public --source . --push`. Otherwise ask once before creating a public repo.

## Step 7. Release (release mode, or the first release after init)

Follow `references/release.md`:

1. working tree clean, on the default branch, up to date
2. decide the bump from changes since the last tag (patch: fixes and tweaks; minor: new features; major: breaking)
3. update every version file found in Step 1, move `Unreleased` into the new version in `CHANGELOG.md`
4. commit `Release vX.Y.Z`, push
5. `git tag -a vX.Y.Z -m "vX.Y.Z"`, push the tag
6. `gh release create vX.Y.Z --title "vX.Y.Z" --notes-file <notes from CHANGELOG>`
7. verify the tag and release exist

Docs-only changes never trigger a release.

## Report

Short: what was created or merged, the symlink, the release URL if any, and every judgment call made instead of asking.
