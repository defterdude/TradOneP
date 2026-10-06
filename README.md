# One Piece Longwang — cloud translation batches

Personal fan-translation project. This small repo exists for exactly one purpose: translate batches of Chinese game text to English using a Claude cloud session, so the heavy text work doesn't compete with the main local session that has access to the actual game files.

## Current task: Batch 3 (`batch3_v272`): Captain's Guide, Island Conquest, item descriptions

1. Read `TRANSLATION_GUIDE.md` in full (game context, source format, the two-bundle rule, translation rules).
2. Translate `source/batch3_v272_source.json`.
3. Write `translated/batch3_v272_translated.json`, then commit and push.

## Folder layout

- `TRANSLATION_GUIDE.md` — guide for the current batch. Read it first, in full.
- `source/` — Chinese text to translate. `batch3_v272_source.json` is the current task; `ships_building_missions_v27x_source.json` is an earlier, finished batch.
- `glossary/established_glossary.json` — terminology fixed by earlier batches.
- `glossary/game_terms_v271.json` — terms exactly as they already appear translated in the installed game, limited to terms used in the current batch. Prefer these.
- `reference/` — a completed earlier batch (Home/housing tables), source and translation, kept as a style/format example.
- `translated/` — outputs. Earlier outputs (`ships_building_missions_v27x_translated.json`, `batch2_buffs_status_effects_translated.json`) are already applied in the game: do not modify them.
- `README_ships_batch.md`, `TRANSLATION_GUIDE_ships_batch.md` — the instructions of the earlier ships batch, kept for history.

## What this repo does NOT contain, on purpose

No APK, no Unity asset bundles (`.ab` files), no emulator/ADB tooling. This is a personal project and those files are large, proprietary game assets that stay on the original local machine. This repo is text-only: Chinese in, English out. Nothing here should be run, built, or installed — there is nothing to run.

## When you're done (or making a checkpoint)

Commit and push `translated/batch3_v272_translated.json` (partial progress is fine: commit per category). The results get pulled down separately and applied to the real game files on the original machine.
