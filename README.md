# One Piece Longwang — cloud translation batches

Personal fan-translation project. This small repo exists for exactly one purpose: translate batches of Chinese game text to English using a Claude cloud session, so the heavy text work doesn't compete with the main local session that has access to the actual game files.

## Current task: batches 3 and 4 (4 source files, about 390k Chinese characters)

1. Read `TRANSLATION_GUIDE.md` in full (order of the files, content, the two-bundle rule, translation rules).
2. Translate, in order:
   - `source/batch3_v272_source.json`;
   - `source/batch4a_system_texts_v273_source.json`;
   - `source/batch4b_world_codex_v273_source.json`;
   - `source/batch4c_events_guild_misc_v273_source.json`.
3. For each, write `translated/<same name>_translated.json`. Commit and push after each file, and more often inside big ones.

## Folder layout

- `TRANSLATION_GUIDE.md` — guide for the current batches. Read it first, in full.
- `source/` — Chinese text to translate. The 4 files above are the current task; `ships_building_missions_v27x_source.json` is an earlier, finished batch.
- `glossary/established_glossary.json` — terminology fixed by earlier batches.
- `glossary/game_terms_v271.json` — about 4,300 terms exactly as they already appear translated in the installed game, limited to terms used in batches 3–4. Prefer these.
- `reference/` — a completed earlier batch (Home/housing tables), source and translation, kept as a style/format example.
- `translated/` — outputs. Earlier outputs (`ships_building_missions_v27x_translated.json`, `batch2_buffs_status_effects_translated.json`) are already applied in the game: do not modify them.
- `README_ships_batch.md`, `TRANSLATION_GUIDE_ships_batch.md` — the instructions of the earlier ships batch, kept for history.

## What this repo does NOT contain, on purpose

No APK, no Unity asset bundles (`.ab` files), no emulator/ADB tooling. This is a personal project and those files are large, proprietary game assets that stay on the original local machine. This repo is text-only: Chinese in, English out. Nothing here should be run, built, or installed — there is nothing to run.

## When you're done (or making a checkpoint)

Commit and push the `translated/*_translated.json` files (partial progress is fine). The results get pulled down separately and applied to the real game files on the original machine.
