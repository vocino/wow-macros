# wow-macros — agent notes

## What this is
- Core repo (`main`, `origin` = `https://github.com/vocino/wow-macros.git`): the WoW macros collection.
- `wiki/` is **not** part of the core repo. It is a separate clone of the GitHub wiki repo (`https://github.com/vocino/wow-macros.wiki.git`, branch `master`), kept here as a local LLM reference wiki (156 command pages, 38 conditional pages, 6 concepts, 13 patterns, 9 anti-patterns; see `wiki/index.md`).

## Boundaries
- `wiki/` is listed in `.gitignore` and must stay out of core commits. Never `git add -f` it.
- Work in `wiki/` belongs to the wiki repo: run git from inside it (e.g. `git -C wiki status`), and commit/push there to publish wiki pages (wiki serves from its `master` branch).
- `%SystemDrive%/` is a stray Windows cache artifact (font-cache `.db` files), also ignored. Leave it alone — don't delete it, don't commit it.
- `Thumbs.db` / `Desktop.ini` are ignored OS noise.

## Committing (both repos)
- A "commit" here covers **two repos**: core (root) and `wiki/`. Always check status in both (`git status` and `git -C wiki status`) and commit each one that has changes.
- Keep them in sync: when a change spans both (e.g. macros plus wiki docs), commit both with the same message, wiki first then core, so docs and code stay in step.
- "Commit" means local commits only. Push (either repo) only when explicitly asked.
- Never commit wiki content into core or vice versa.

## Pages site (main repo)

- Static GitHub Pages site at the repo root: `index.html` (class grid),
  `classes/<class>.html` (one page per Forever class, 9 total),
  `assets/style.css` (only stylesheet). No JavaScript, no build step.
- Class pages group future macros under Leveling / Dungeons & raids / PvP.
  New macros must come from the wiki and obey the macro-writer rules.
- `.nojekyll` present; Pages deploys from branch `main`, root.
- Preview by opening `index.html` in a browser.

## Gates
- None yet: no code, no tests, no lint. If macro files or checks are added later, document the gate here.

## Macro-writer rules (binding on every wiki page)

1. One GCD spell per keypress. A failed would-be-GCD `/cast` still blocks
   later lines (since patch 2.0). Instant non-GCD casts chain freely.
2. 255 characters per macro, newlines included. Overflow workaround is a
   `/click`-chain across action-bar buttons (mind the 11.0.2 anti-chain
   rule on Retail: a real macro may not `/click` a button running another
   macro).
3. No wait, no pause, no unattended casting. A macro runs start-to-finish in
   one frame on a player hardware press; a long `/run` freezes the client.
4. Branch only on the allowlisted conditionals. Macros CANNOT test cooldown,
   aura presence, health, mana/resource, or rotation priority. `dead` is
   binary dead/alive only; `help`/`harm` is targetability only.
5. Combat effects go through the secure path only. `/cast` `/use`
   `/castsequence` `/click` etc. run Blizzard-executed on keypress and are
   combat-safe; `/script` `/run` `/console` execute tainted and can never
   cast, use, target, or change action bars (9.1.5 protects more APIs).
6. Version-stamp every claim: `retail` / `classic-era` / `cata-classic` /
   `forever`. Anything unverified is `unknown` — never guessed. Known traps:
   Era has no `/focus`, no `@cursor`/`@player`; Cata uses WotLK-era
   `talent:` spelling, not retail `spec:`; Forever `[spec:]`/`[talent:]` and
   rank-casting syntax are UNRESOLVED (sources contradict).
7. Command names are case-insensitive; `[conditions]` are case-SENSITIVE
   (`[help]`, never `[Help]`). Only secure commands accept conditions —
   chat/emotes do not. Cite a source URL + `patch_verified` per claim and
   flag contradictions instead of smoothing them over.

## Workflows

### Ingest (one source at a time, supervised)

1. Read the source; distill claims into `raw/<slug>.md` (immutable clip:
   summary + source URL + fetch date). Never edit a raw clip — re-fetch.
2. Touch every affected page: new page or section per claim, `index.md`
   summary line, `log.md` entry (`## [YYYY-MM-DD] ingest | Title`).
3. One source can touch 10-15 pages (summary + index + entity/concept
   updates + log). Verify `versions:` + `patch_verified` on each touched
   page before finishing.

### Query (answer from the wiki, then compound it)

1. Read `wiki/index.md` first, then drill into the linked pages only.
2. Synthesize with citations (page + source URL), marking unknowns.
3. File good answers back as new pages (patterns, comparisons, synthesis)
   with index + log entries, so explorations compound.

### Lint (periodic health check)

Check for: contradictions between pages, stale/superseded claims (e.g. old
slot counts, removed `/castrandom`), orphans (unlinked pages), missing
pages/refs, gaps (unresolved list), new questions/sources. Record findings
as `log.md` entries and fix or flag.

## Page templates

### Command page (`wiki/commands/<name>.md`)

```markdown
---
command: /cast
aliases: [/spell]
syntax: "/cast [options] <spell|item|bag slot|invSlot>"
args: "[options] + spell/item/bag-slot/invSlot; name-conflict resolves to SPELL"
secure: true
works_in_combat: true
versions: [retail, classic-era, cata-classic, forever]
patch_verified: "2.3.2"
source_urls:
  - https://warcraft.wiki.gg/wiki/MACRO_cast
last_checked: 2026-11-25
---

# /cast

## What it does

## Syntax

## Examples

## Edge cases

## Common mistakes

## Related commands
```

Frontmatter fields are all required. `secure` = accepts secure command
options; `works_in_combat` follows the secure/taint classification in the
research (secure path = safe; `/script`-class = tainted; chat/UI = not
protected, combat-irrelevant; disabled = parsed no-op). `versions` uses the
four-version vocabulary; `unknown` where unverified.

### Concept page (`wiki/concepts/<slug>.md`)

```markdown
---
topic: <slug>
kind: concept
versions: [...]
patch_verified: "<patch or null>"
source_urls:
  - https://...
last_checked: 2026-11-25
---

# <Title>

## Definition

## Rules

## Examples

## What breaks it

## Related pages
```

### Conditional page (`wiki/conditionals/<name>.md`)

Frontmatter: `conditional`, `syntax`, `allowed_values`, `tests`,
`cannot_test` (required on every page), `versions`, `patch_verified`,
`source_urls`. Body: What it tests (and vs whom: target / player / UI),
Syntax, Examples, What it cannot test, Related conditionals.

### Pattern / anti-pattern page (`wiki/patterns/<slug>.md`)

Frontmatter: `pattern`, `class`, `spec`, `role`, `versions`,
`patch_verified`, `char_count` (counted with a shell command, never
estimated), `tested_in_game`, `source_urls`. Anti-patterns use `topic` +
`status: anti-pattern` with the same version/source fields. Body: full macro block, What it
does, Why it works, What it deliberately does not do, Variations
(anti-patterns: What not to generate, Why it fails, Correct alternative).

