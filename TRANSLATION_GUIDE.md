# Translation guide — Ships / Building / Missions batch

## Game context

This is a personal fan-translation patch for a Chinese mobile RPG, **One Piece Burning Will** (航海王：燃烧意志), private server "Longwang". The game has a naval/ship system (players own warships with props/skills, trade routes, treasure hunting), a guild building system, and a housing/base-building system (already translated separately — see `reference/`). You are translating three categories that have never been translated before: **Building** (guild + seaport buildings), **Ships** (warship stats, skills, combat attribute text), and **Missions** (daily/repeatable task boards — explicitly NOT the main story or side-quest dialogue, which stays untranslated on purpose).

## Source data

`source/ships_building_missions_v27x_source.json` — read its `header` first. It explains the file shape and lists every table under `header.tables_by_category` (`building`, `ships`, `missions`). Each table lives at `tables.<category>.<TableName>` with:
- `key_column` — the column that uniquely identifies a row (copy verbatim, never translate)
- `text_columns` — which fields contain translatable Chinese text
- `rows` — the Chinese text from `text_source_bundle`
- `content_diverges_between_bundles` — **read this per table, it matters** (see next section)

## Critical: the two-bundle divergence problem

The game ships two nearly-identical data files (`tabledata.ab` and `tabledata_v1.ab`) that are supposed to mirror each other, but for most tables in this batch **they don't** — same row ID, different Chinese wording and/or different numbers, presumably from different build revisions.

- If `content_diverges_between_bundles` is `false`: translate `rows` once. That translation is used for both bundle copies.
- If `content_diverges_between_bundles` is `true`: the table also has `other_bundle` (the bundle name) and `other_bundle_rows` (that bundle's own Chinese text). **Translate `rows` and `other_bundle_rows` completely independently.** Do not assume they mean the same thing or reuse one translation for the other, even if they look similar — check `divergence_detail.keys_with_differing_text_in_both_bundles` to see exactly which rows differ. Some rows in a "diverging" table may still be identical between bundles; only the ones the detail lists need separate treatment (though translating everything independently is always fine and never wrong).

`TableShipSkillAttributeConfig` is the extreme case: `tabledata.ab` has 867 rows (ids 1–10050) and `tabledata_v1.ab` has 2272 rows (ids 1–3012), covering mostly different ids with genuinely different text — not a clean superset. Both are large; treat them as two mostly-separate translation jobs sharing the same output table.

## Strict rules (same discipline used for the housing batch)

1. **Placeholders**: text contains `{0}`, `{1}`, etc. in `tabledata.ab`, but `${0}$`, `${1}$` (dollar-wrapped) in `tabledata_v1.ab` for `TableShipSkillAttributeConfig` specifically. Preserve whichever exact placeholder syntax appears in the row you're translating — do not convert one style to the other, do not renumber, do not drop a placeholder.
2. **Color tags**: `<color=#RRGGBB>...</color>` must appear in your translation with the exact same hex code and the same open/close pairing as the source. Never add or remove one.
3. **Numbers**: every percentage, flat number, and quantity in the source must appear unchanged in the translation. Never recalculate or round.
4. **Empty strings** (e.g. some `skillName` fields are `""`) stay empty — never invent content.
5. **Terminology**: read `glossary/established_glossary.json` first and reuse its terms whenever the same Chinese word appears. Add new terms you coin for ship/naval/mission vocabulary to that file (or note them in your output) so future batches stay consistent.
6. **Don't guess wildly**: if a term is genuinely ambiguous, pick your best interpretation and note it — don't silently drop or paraphrase away something you don't understand.
7. **Style**: short UI labels (names, titles) stay short — a few words. Descriptions can be short sentences matching typical mobile-game tooltip terseness. See `reference/home_tables_v262_translated.json` for a worked example of the tone/format already accepted for this project.
8. **Missions scope reminder**: everything in the `missions` category here is daily/repeatable/system content (login rewards, arena scores, codex progress, trial encounters, treasure hunting) — none of it is main-story or side-quest narrative, so there's no ambiguity about whether it's in scope. Translate all of it.

## Output format

For each table, produce a translated version with the exact same keys/shape as the source: `rows` (translated) and, when the source had it, `other_bundle_rows` (translated independently). Keep every key column verbatim. Write your output to `translated/ships_building_missions_v27x_translated.json`, mirroring the source's `tables.<category>.<TableName>` structure. Include a top-level `glossary_additions` object recording any new term choices you made, and an `uncertain_notes` array for anything genuinely ambiguous (same pattern as `reference/home_tables_v262_translated.json`).

## What this repo is NOT for

This repo contains **only text** — no game binaries, no Unity asset bundles, no APK. Do not try to build, patch, install, or verify anything against the actual game here; that happens later, locally, on the original machine, using the real bundle files this repo doesn't have. Your job is purely to produce correct, complete translated JSON and commit/push it.

## Suggested order (commit as you finish each one, don't wait until everything is done)

1. `building` category — small (3 tables, ~38 rows total), good warm-up.
2. `missions` category — small-medium (6 tables, ~100 rows total).
3. `ships` category — do the smaller tables first (`TableShipProp`, `TableShipSkill`, `TableShipTradeEvent`, `TableGvoShip`, `TableSeaAreaShipSkill`, `TableHideTreasureShipPool`, `TableUnionTradeShipReward`, `TableUnionTradeShipItemType`, `TableUSFAIShip`), then tackle `TableShipSkillAttributeConfig` last since it's by far the largest (~3100 rows combined across both bundles).
