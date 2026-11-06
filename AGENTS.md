# AGENTS.md

Instructions for any coding agent (Claude Code, Codex, others) working in this repository. `CLAUDE.md` is a symlink to this file; edit `AGENTS.md` only.

## What this repo is

`ossify` is an agent skill that sets up open-source conventions in a repo: `AGENTS.md` with a `CLAUDE.md` symlink, a release routine with tags and GitHub releases, and English and Korean READMEs. It is distributed as a Claude Code plugin and as a plain skill folder. This repo follows its own conventions.

```text
skills/ossify/SKILL.md            the skill (entry point)
skills/ossify/references/*.md     loaded on demand by SKILL.md
.claude-plugin/plugin.json        plugin manifest (version)
.claude-plugin/marketplace.json   marketplace manifest (metadata.version)
README.md / README.ko.md          English and Korean docs
assets/banner.png                 README banner
CHANGELOG.md                      release notes
```

## Rules for every change

1. **Commit and push after every change.** One logical change per commit. Push to `main` right after committing. Do not leave work uncommitted at the end of a task.
2. **Release only for distribution changes.** A change is a distribution change when it alters what users install or how the skill behaves: anything under `skills/` or `.claude-plugin/`, or a changed install command. For these:
   - bump the version one step in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`: patch for fixes and rule tweaks, minor for new modes or references, major for breaking changes
   - move the `Unreleased` notes in `CHANGELOG.md` under the new version
   - commit `Release vX.Y.Z`, push, then tag and release:
     ```bash
     git tag -a vX.Y.Z -m "vX.Y.Z"
     git push origin vX.Y.Z
     gh release create vX.Y.Z --title "vX.Y.Z" --notes-file <notes>
     ```
3. **Docs-only changes do not release.** README, AGENTS.md, images, and typo fixes are committed and pushed with no version bump, tag, or release.
4. **Keep READMEs in sync.** Any change to `README.md` gets the same change in `README.ko.md`, and the reverse.
5. **Validate before committing** a distribution change: `claude plugin validate .` must pass.
6. **Never commit secrets or private information**: no tokens, employer or internal names, personal emails, or home paths.

## Project rules

- The templates in `skills/ossify/references/` must stay consistent with this file. When a rule here changes, change `agents-template.md` too, and the reverse.
- Keep the skill general: no assumptions about a specific project, company, or topic.
- `SKILL.md` stays under 200 lines; detail goes into `references/`.

## Commit messages

Imperative, short subject. Example: `Add Swift package detection`.
