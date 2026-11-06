# Release

## Preconditions

```bash
git status --short             # empty
git branch --show-current      # default branch
git pull --ff-only
git describe --tags --abbrev=0 # last tag, if any
```

If the tree is dirty, commit or stop and say why. Never release unpushed work.

## Is this a release?

List changes since the last tag:

```bash
git log --oneline $(git describe --tags --abbrev=0 2>/dev/null || git rev-list --max-parents=0 HEAD)..HEAD --stat
```

- Only docs, README, AGENTS.md, images, comments: **no release**. Say so and stop.
- Anything under the distribution paths in `AGENTS.md`: release.

## Pick the bump

| Change | Bump |
|---|---|
| bug fix, wording or rule tweak, dependency patch | patch `x.y.Z` |
| new feature, new mode, new reference or command | minor `x.Y.0` |
| removed or renamed command, changed defaults that break users | major `X.0.0` |
| first release | `1.0.0` (or `0.1.0` if the user calls it experimental) |

## Update files

1. Every version file listed in `AGENTS.md` (see `project-types.md`). Grep for the old version to catch stragglers:
   `grep -rn '"<old>"\|version = "<old>"' . --exclude-dir=.git --exclude-dir=node_modules`
2. `CHANGELOG.md`: rename `## Unreleased` to `## vX.Y.Z (YYYY-MM-DD)`, add a fresh empty `## Unreleased` above it. If there was no Unreleased section, write the entry from the commit log, grouped into Added, Changed, Fixed.
3. Run the validation command from `AGENTS.md`.

## Commit, tag, release

```bash
git add -A
git commit -m "Release vX.Y.Z"
git push
git tag -a vX.Y.Z -m "vX.Y.Z"
git push origin vX.Y.Z
# notes = the CHANGELOG section for this version, plus install lines, plus the session's attribution line if configured
gh release create vX.Y.Z --title "vX.Y.Z" --notes-file <notes>
```

Use annotated tags (`-a`), always prefixed with `v`.

## Verify

```bash
gh release view vX.Y.Z --json tagName,url -q '"\(.tagName) \(.url)"'
git ls-remote --tags origin | grep vX.Y.Z
```

For a Claude Code plugin, also reinstall from GitHub and check the version:
`claude plugin marketplace update <marketplace>` then `claude plugin list | grep -A2 <name>`.

## Mistakes

- Wrong version tagged and not yet announced: delete the release and tag (`gh release delete vX.Y.Z --cleanup-tag`), fix, release again. This is destructive; confirm with the user first.
- Already used by others: never move a tag. Release the next patch instead.
