# README rules

## Files

- `README.md`: English. GitHub shows this one.
- `README.ko.md`: Korean. Same sections in the same order.
- Any change to one goes into the other in the same commit.

## Header (both files)

```markdown
<p align="center">
  <img src="assets/banner.png" alt="<name> banner" width="100%">
</p>

<p align="center">
  <b>English</b> · <a href="README.ko.md">한국어</a>
</p>
```

In `README.ko.md` the switch is `<a href="README.md">English</a> · <b>한국어</b>`. Drop the image block if there is no banner.

## Sections, in order

1. `# <name>` and a **bold one-line promise** (what the user gets, not what the code is)
2. **What it is**: two or three sentences
3. **Why it exists**: the real problem or failure that led to it
4. **What it does**: a table or a short list
5. **Example**: a real input and its real output
6. **Requirements**: only what is actually needed
7. **Install**: every supported path, commands that were actually run
8. **Usage**
9. **Repository layout**: a short tree with one comment per line
10. **Credits**, if the project builds on others
11. **License**

Drop sections that would be empty. Do not add badges, emoji headers, or a table of contents for READMEs under ~200 lines.

## Writing rules

- Every example is real: copied from an actual run, or clearly marked as illustrative.
- Every command was run at least once while writing, or comes straight from the tool's docs.
- Short sentences. Active voice. No hype words ("blazing", "revolutionary", "seamless", "powerful").
- No em dashes in prose; use periods, commas, or colons.
- Korean is written as Korean, not translated line by line. Keep code blocks, paths, and commands identical across both files.
- No employer names, internal project names, emails, or home paths.

## Banner

Generate with GPT image generation through the Codex CLI. Start it in the background; it takes a few minutes.

```bash
codex features list | grep image_generation    # must be true
mkdir -p assets
codex exec --skip-git-repo-check --sandbox workspace-write -C <repo> \
  "Use your image generation tool to create ONE README hero banner image, wide landscape (about 1600x640). \
Concept: <what the project does, as a simple visual metaphor>. Style: warm hand-drawn editorial illustration, \
cream paper, ink lines, one accent color (<color>). Put the word '<name>' in large clean lowercase lettering \
in the upper left. No other text, no logos, no brand names. Copy the final image to ./assets/banner.png and print its path." \
  > <scratchpad>/banner.log 2>&1
sips -Z 1600 assets/banner.png --out assets/banner.png    # macOS; keeps the README light
```

If Codex is not available but `OPENAI_API_KEY` is set, use the OpenAI Images API. If neither, skip the banner and say so.

Open the image before committing. Reject it if it has misspelled text, brand logos, or details that are unreadable at README width.
