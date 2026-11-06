# AGENTS.md template

Fill every `<...>` with real values from the repo. Delete lines that do not apply. Keep it short; agents read it on every task.

````markdown
# AGENTS.md

Instructions for any coding agent (Claude Code, Codex, others) working in this repository. `CLAUDE.md` is a symlink to this file; edit `AGENTS.md` only.

## What this repo is

<one paragraph: what it does and how it is distributed>

```text
<real layout, one line per important path, with a short comment>
```

## Rules for every change

1. **Commit and push after every change.** One logical change per commit. Push to `<default branch>` right after committing. Do not leave work uncommitted at the end of a task.
2. **Release only for distribution changes.** A change is a distribution change when it alters what users install or how the project behaves: <distribution paths, e.g. `src/`, `skills/`, `.claude-plugin/`, `package.json` dependencies>, or a changed install command. For these:
   - bump the version one step in <every version file>: patch for fixes and small tweaks, minor for new features, major for breaking changes
   - move the `Unreleased` notes in `CHANGELOG.md` under the new version
   - commit `Release vX.Y.Z`, push, then tag and release:
     ```bash
     git tag -a vX.Y.Z -m "vX.Y.Z"
     git push origin vX.Y.Z
     gh release create vX.Y.Z --title "vX.Y.Z" --notes-file <notes>
     ```
3. **Docs-only changes do not release.** README, AGENTS.md, images, comments, and typo fixes are committed and pushed with no version bump, tag, or release.
4. **Keep READMEs in sync.** Any change to `README.md` gets the same change in `README.ko.md`, and the reverse.
5. **Validate before committing** a distribution change: `<validation command: tests, build, lint, or plugin validate>` must pass.
6. **Never commit secrets or private information**: no tokens, employer or internal names, personal emails, or home paths.

## Project rules

<rules specific to this repo, merged from any earlier CLAUDE.md or AGENTS.md>

## Commit messages

Imperative, short subject. Example: `<a realistic example for this repo>`.
````

## Symlink notes

- Create from the repo root: `ln -s AGENTS.md CLAUDE.md` (relative target, so it works after cloning).
- Check: `ls -l CLAUDE.md` shows `CLAUDE.md -> AGENTS.md`; on GitHub the raw `CLAUDE.md` content is the text `AGENTS.md`.
- Windows clones need `git config core.symlinks true`; mention it in the README only for projects with Windows users.
