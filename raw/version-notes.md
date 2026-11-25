# Raw clip — macro version-divergence notes (IMMUTABLE: do not edit, re-fetch instead)

Fetch date: 2026-11-25. Mark verified vs unknown; never guess.
Per-claim version is UNKNOWN unless a patch note states it.

## Common core (verified all versions)

`/cast` `/castsequence` `/use` `/cancelaura` `/stopcasting` `/startattack`
`/stopattack` `/petattack` `/petfollow`, `#showtooltip`,
`[help]` `[harm]` `[exists]` `[dead]`/`[nodead]` `[stance:n]`/`[form:n]`
`[stealth]` `[combat]`/`[nocombat]` `[mod:shift/ctrl/alt]` `[button]`/`[btn:n]`
`[equipped:]` `[channeling:]` `[group:]` `[mounted]` `[swimming]`
`[indoors]`/`[outdoors]` `[actionbar:]` `[party]`/`[raid]`; `;` fallbacks.

Sources: wiki.gg conditionals + Classic/Cata guides, fetched 2026-11-25.

## Retail (The War Within / Midnight era)

Verified via wiki.gg patch 11.2.7 note + retail guides:

- Adds `[spec:n]` `[talent:]` `[known:]` `[advflyable]` `[flying]`
  `[flyable]` `[pvpcombat]` `[petbattle]` `[house:*]`.
- Adds `@cursor` `@player` `@none` target tokens, `/focus` + `focustarget`.
- Slots: 120 general + 30 character-specific in 12.1. Older 18-slot figures
  are obsolete for Retail.
- 11.0.2 rule: real macros/macrotext may NOT `/click` a button that runs
  another macro (anti-chain rule).
- 12.0.0 Secret Values model: combat/PvP/encounter/dungeon-raid APIs may
  return secrets; tainted code may store/pass/concatenate but not compare,
  do arithmetic, take length, or call. 12.0.1 adds
  `RestrictedForMacroChatMessages` (macro-initiated chat restricted in
  encounters).
- 9.1.5: several APIs protected from direct `/script` calls.

## Classic Era

Verified via Blizzard forum "Macros: Essential Information" + Classic threads:

- NO `/focus` or `focustarget` unit ID.
- NO `@cursor` / `@player` reticle conditionals — ground-target spells need
  a manual reticle click.
- `[spec:]` / `[talent:]` / `[flyable]` not relevant.
- `[@mouseover]` WORKS.
- 18 general + 18 character-specific slots, 255 chars per macro.

## Cataclysm Classic

Verified via Icy Veins Cata/WotLK guides + addon compat 4.4.2:

- HAS `[@cursor]` (e.g. `/cast [@cursor] Healing Rain`, Rain of Fire).
- HAS `[@focus]` / `[target=focus]` (e.g. Polymorph / Fire Blast focus macros).
- WotLK-era vocabulary incl. `talent:tree/column` form (NOT retail `spec:`
  form). Exact talent-conditional spelling Cata-vs-Retail is PARTIALLY
  VERIFIED: era-appropriate `talent:` form attested in WotLK-era docs, retail
  `spec:` form verified modern only.

## WoW Forever (interface 16001)

Verified: interface 16001, modern-code-family game type; beta warrior macro
with `[stance:]`/`[nostance:]` `[combat]`/`[nocombat]` + `#showtooltip` /
`/startattack` / multi-`/cast` + `;` fallbacks CONFIRMED working (Oct beta
post). Standard vocabulary listed as expected-not-tested. Uses the
MODERN/Retail macro system; no Forever interface-diff macro changes (one
gamepad-bar fix only). Restrictions target addon combat-state reads, not
player macros. One action per keypress; no cooldown/aura/resource-based
selection (Classic baseline).

CONTRADICTION FLAGGED: CurseForge "Forever Macro-builder" claims
Classic-only syntax with retail `[spec:]`/`[talent:]` deliberately excluded —
contradicts tier-list modern-system evidence. Treat Forever
`[spec:]`/`[talent:]` support as UNRESOLVED; test in game.

## UNKNOWN — do not publish as fact

- Forever rank-casting syntax (Classic parenthesised-rank form vs modern
  highest-rank-only) — undocumented in every readable source.
- 255-char limit + slot counts in Forever (Classic baseline assumed, no
  Forever statement).
- Cooldown Manager / damage-meter details beyond beta notes.
- Per-expansion applicability (Retail vs Classic Era vs SoD/Hardcore) for
  index-only commands — recorded as unknown.
- `/castrandom` + `/userandom` current-Retail validity: Macro_commands page
  (ed. 2026-02-08) still lists them; believed removed in Cataclysm-era
  cleanup — needs a patch-note or in-game check.
- `talent:` exact spelling on a live Cata client (era docs only).
- wiki.gg Patch 1.60.1/API_changes page body (cited secondhand only).
- In-game FrameXML; `/api` `/dump` `/eventtrace` `/framestack` probes
  (no client available); wowuidev Discord (invite-gated).
