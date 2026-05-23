# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is **not a runnable application** — it is a Claude Code **Skill** (and `/plugin` package) that another instance of Claude reads at runtime to generate HTML presentations. The "code" is mostly Markdown that briefs the model, plus three helper scripts. There is no build system, no test suite, no package manager, no `package.json`.

**This repo is a Simplified-Chinese-native fork of upstream `zarazhangrui/frontend-slides`, renamed `slides-cn` at the package level.** The GitHub repo path is `Afee2019/frontend-slides` (kept for fork discoverability), but the marketplace name, plugin name, skill name, and slash command are all `slides-cn` so the two plugins can coexist if a user installs both. It is intentionally diverged — it will not rebase onto upstream. Key differences:

- All Skill instructions, AskUserQuestion options, and code comments are in Simplified Chinese.
- Every preset has a curated Chinese font pairing alongside the original English `font-family`.
- 3 new Chinese-native presets exist with no English equivalent: 宣纸水墨 (Ink & Paper), 港式霓虹 (Kowloon Neon), 杂志感 CN (Editorial CN).
- `viewport-base.css` ships a `[lang^="zh"]` rule block that enables `line-break: strict`, `hanging-punctuation`, `text-spacing` + `palt`, `line-height: 1.7`, and raises the body font-size floor to 14px.
- `animation-patterns.md` has a `# 中文排版规范` section covering full-width punctuation, 避头尾, first-line indent, 悬挂标点, etc.
- Content density limits in `SKILL.md` are recalibrated for CJK character density (titles ≤ 12 chars, bullets ≤ 15 chars, paragraphs ≤ 60 chars).

English content is still a first-class target — the `font-family` strings put the English face first so Latin/digit glyphs always render correctly. Mixed Chinese/English (中英混排) is the most-tested case.

When asked to "run" or "test" this project, the only meaningful operations are:

- Invoke the skill itself in another Claude Code session (`/slides-cn`) and inspect the generated HTML.
- Execute the three scripts directly against a sample deck (see below).

## Repository Layout

Root-level files are the canonical skill content. The `plugins/slides-cn/skills/slides-cn/` tree is the **plugin distribution form** — every file in there is a **symlink** back to the root copy. Edit at the root; do not duplicate.

```
SKILL.md                 # entrypoint — always loaded when the skill is invoked (name: slides-cn)
STYLE_PRESETS.md         # 15 visual presets (12 bilingual + 3 Chinese-native), loaded in Phase 2
viewport-base.css        # MANDATORY CSS — must be inlined verbatim into every generated deck
html-template.md         # HTML/JS architecture reference, loaded in Phase 3
animation-patterns.md    # CSS/JS animation snippets + 中文排版规范, loaded in Phase 3
scripts/extract-pptx.py  # PPTX -> JSON + assets/ extractor (Phase 4)
scripts/deploy.sh        # Vercel deploy (Phase 6A)
scripts/export-pdf.sh    # Playwright-based PDF export (Phase 6B)
.claude-plugin/marketplace.json                  # marketplace metadata (name: slides-cn)
plugins/slides-cn/.claude-plugin/plugin.json     # plugin metadata (name: slides-cn)
plugins/slides-cn/skills/slides-cn/*             # symlinks back to root (do not edit)
```

## Architectural Big Picture

**Progressive disclosure.** `SKILL.md` is intentionally short (~180 lines) and acts as a phase map (Phase 0–6). Each phase points to a supporting file that should only be read when that phase runs. Don't merge supporting files into `SKILL.md`; that defeats the design. When adding new capability, decide which phase it belongs to and extend the file for that phase.

**`viewport-base.css` is contract, not example.** Every presentation Claude generates must contain the full contents of this file inlined into its `<style>` block. Changes to this file change the runtime behavior of every future generated deck — treat edits as breaking changes.

**One self-contained HTML file is the output target.** Zero dependencies, no build, inline CSS/JS, fonts from Fontshare/Google Fonts. This constraint is load-bearing: it's the reason the skill is durable and what `deploy.sh` and `export-pdf.sh` can both assume.

**Two distribution surfaces, one source.** The skill is consumed two ways: (1) copied into `~/.claude/skills/slides-cn/` manually, or (2) installed via `/plugin marketplace add Afee2019/frontend-slides` + `/plugin install slides-cn@slides-cn`. The plugin form just symlinks the root files into a `plugins/.../skills/.../` directory the plugin loader expects. If you add a new top-level file that the skill needs at runtime, add a matching symlink under `plugins/slides-cn/skills/slides-cn/` (or `plugins/.../scripts/` for scripts).

**Naming map** (easy to confuse — they're all `slides-cn` but live at different paths):

| Identifier               | Value                                          | Where                                    |
| ------------------------ | ---------------------------------------------- | ---------------------------------------- |
| GitHub repo              | `Afee2019/frontend-slides`                     | git remote (intentionally not renamed)   |
| Marketplace name         | `slides-cn`                                    | `.claude-plugin/marketplace.json`        |
| Plugin name              | `slides-cn`                                    | `plugins/slides-cn/.claude-plugin/plugin.json` |
| Skill name               | `slides-cn`                                    | `SKILL.md` frontmatter                   |
| Slash command            | `/slides-cn`                                   | derived from skill name                  |
| Plugin install spec      | `slides-cn@slides-cn`                          | `<plugin>@<marketplace>` form            |

## Load-Bearing Invariants

These are documented in `SKILL.md` and `html-template.md` but worth surfacing here because they are easy to break with a "small fix":

- **Viewport fitting is non-negotiable.** Every `.slide` is `100vh / 100dvh` with `overflow: hidden`. Font sizes and spacing use `clamp()`. Breakpoints at 700/600/500px exist. Content that doesn't fit must be split into more slides, not allowed to scroll.
- **Inline editing must be opt-in.** Phase 1 asks the user; if they say No, do **not** generate any edit-related HTML/CSS/JS.
- **`exportFile()` must strip edit state before snapshotting `outerHTML`.** Otherwise the saved file opens permanently in edit mode (this is the bug fixed in commit `ee6f18e`). See `html-template.md` "Inline Editing Implementation" for the exact pattern.
- **`setupNavDots()` must clear its container before populating.** Otherwise re-opening an exported file appends a duplicate set of dots (same commit).
- **Never use CSS `~` sibling selector for the edit-hotzone hover-reveal.** `pointer-events: none` breaks the hover chain. Use the JS pattern with a 400ms grace timeout documented in `html-template.md`.
- **`<html lang="zh-CN">` is the trigger for the entire Chinese typography rule set in `viewport-base.css`.** If you omit it, none of the 避头尾 / `palt` / hanging-punctuation / raised font-size floor rules activate. Always set `lang="zh-CN"` for Chinese-primary decks and `lang="en"` for English-primary; mixed decks use the language of the dominant content.
- **Font-family ordering matters:** English face first, Chinese face second, system fallback last. Reversing the order makes digits and Latin characters fall into the Chinese font's Latin glyph table (usually ugly). The one exception is the `Sarasa Mono SC` + `JetBrains Mono` pairing, where width alignment is engineered for both orderings.

## Commonly Used Commands

There is no `pnpm dev` here — these are the only scripts in this repo:

```bash
# Extract a .pptx into JSON + assets/ (requires python-pptx)
python scripts/extract-pptx.py <input.pptx> [output_dir]

# Deploy a deck folder or single HTML file to Vercel (interactive login if needed)
bash scripts/deploy.sh ./my-deck/
bash scripts/deploy.sh ./presentation.html

# Export an HTML deck to PDF via Playwright (installs Chromium on first run)
bash scripts/export-pdf.sh ./my-deck/index.html
bash scripts/export-pdf.sh ./presentation.html ./out.pdf --compact   # 1280x720 instead of 1920x1080
```

When changing any of the three scripts, smoke-test with a sample deck — there is no automated test suite. `deploy.sh` and `export-pdf.sh` both assume slides use `class="slide"`; that contract comes from `viewport-base.css` and `html-template.md`.

## When Editing the Markdown Files

Most "code changes" in this repo are prose changes that alter how a downstream Claude generates HTML. Useful habits:

- If you change `SKILL.md`, keep it terse. Long instructions degrade adherence; that's the whole point of the progressive-disclosure split.
- If you add a new style preset, update `STYLE_PRESETS.md` and the mood→preset table in `SKILL.md` Phase 2.2.
- If you change `viewport-base.css`, also re-read the CSS that `html-template.md` shows around it — they must stay consistent.
- The README's "Architecture" table and `SKILL.md`'s "Supporting Files" table both list the file → phase mapping. Keep them in sync when adding/removing supporting files.
- **All user-facing prose in this repo (skill instructions, AskUserQuestion options, generated HTML comments) should be in Simplified Chinese.** English appears only where the audience is GitHub readers (the short English preamble at the top of README) or where a CSS keyword / library name must stay English. When in doubt, write Chinese.
- **Don't break English support while improving Chinese support.** The English `font-family` faces are kept exactly where they were; the Chinese fonts are added _after_ them in the stack. CSS rules scoped under `[lang^="zh"]` won't fire for `lang="en"` pages. Test both orientations after touching `viewport-base.css`.
