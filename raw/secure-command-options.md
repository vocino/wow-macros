# RAW CLIP — Secure command options syntax (immutable, do not edit)

- Fetch date: 2026-11-25 (HTTP 200).
- Source: https://wowpedia.fandom.com/wiki/Secure_command_options (currency caveat: carries banner "The API is no longer being updated here until further notice")
- Options string = one or more CLAUSES delimited by semicolons.
- Each clause = zero or more CONDITIONS + a VALUE string (value MUST NOT contain `[`, `]`, `;`).
- Each condition in square brackets holds zero or more macro conditionals separated by COMMAS = logical AND within one bracket.
- Multiple bracket groups = fallbacks: conditions evaluated IN ORDER, LEFT TO RIGHT; FIRST satisfied condition wins, returning its clause value + conditional target; unsatisfied rest skipped.
- `[]` (or clause with no brackets, e.g. `; Rejuvenation`) is always satisfied = default/fallback.
- If NO condition satisfied, no value returned (command does nothing).
- Meaning of returned value/target is CONTEXT-DEPENDENT per macro command (e.g. `/stopcasting` ignores value/target; `/target` ignores value, uses target).
- Canonical example from page: `/cast [@mouseover,harm][harm] Moonfire; [@mouseover,help][@focus,help,nodead][@player,mod:alt] Regrowth; Rejuvenation` — 3 clauses, 6 conditions (one implicit `[]`), first match wins.
- Also from page: `/stopcasting [mod:alt]`; `/focus [@mouseover,mod:alt][]`; `/target [@focus,help][] player`.
- No contradictions with Macro_conditionals page on shared syntax (brackets/commas/semicolons/order/fallbacks consistent).
- Unresolved: post-10.0.7 currency (API-stale banner; whether secure options changed after Dragonflight unverified).
