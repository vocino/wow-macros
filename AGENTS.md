# wow-macros — agent notes

## What this is
- Core repo (`main`, `origin` = `https://github.com/vocino/wow-macros.git`): the WoW macros collection.
- `wiki/` is **not** part of the core repo. It is a separate clone of the GitHub wiki repo (`https://github.com/vocino/wow-macros.wiki.git`, branch `master`), kept here as a local LLM reference wiki. Currently just a placeholder `Home.md`.

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

## Gates
- None yet: no code, no tests, no lint. If macro files or checks are added later, document the gate here.
