# wow-macros — agent notes

## What this is
- Core repo (`main`, `origin` = `https://github.com/vocino/wow-macros.git`): the WoW macros collection. Currently empty — no commits yet.
- `wiki/` is **not** part of the core repo. It is a separate clone of the GitHub wiki repo (`https://github.com/vocino/wow-macros.wiki.git`, branch `master`), kept here as a local LLM reference wiki. Currently just a placeholder `Home.md`.

## Boundaries
- `wiki/` is listed in `.gitignore` and must stay out of core commits. Never `git add -f` it.
- Work in `wiki/` belongs to the wiki repo: run git from inside it (e.g. `git -C wiki status`), and commit/push there to publish wiki pages (wiki serves from its `master` branch).
- `%SystemDrive%/` is a stray Windows cache artifact (font-cache `.db` files), also ignored. Leave it alone — don't delete it, don't commit it.
- `Thumbs.db` / `Desktop.ini` are ignored OS noise.

## Gates
- None yet: no code, no tests, no lint. If macro files or checks are added later, document the gate here.
