# One Piece Longwang — cloud translation batch

Personal fan-translation project. This small repo exists for exactly one purpose: translate a batch of Chinese game text to English using a Claude cloud session, so the heavy text work doesn't compete with the main local session that has access to the actual game files.

## Read first

Start with `TRANSLATION_GUIDE.md` — it has the game context, the source data format, the critical two-bundle-divergence issue you must handle correctly, and all the translation rules.

## Folder layout

- `TRANSLATION_GUIDE.md` — read this first, in full.
- `source/ships_building_missions_v27x_source.json` — the Chinese text to translate (the actual task).
- `reference/` — a previously-completed translation batch (Home/housing tables), both source and translated, kept here purely as a style/format example. Not part of this task.
- `glossary/established_glossary.json` — terminology already fixed by the housing batch; reuse it for consistency.
- `translated/` — write your output JSON here (`ships_building_missions_v27x_translated.json`), and commit/push as you make progress rather than waiting until everything is done.

## What this repo does NOT contain, on purpose

No APK, no Unity asset bundles (`.ab` files), no emulator/ADB tooling. This is a personal project and those files are large, proprietary game assets that stay on the original local machine. This repo is text-only: Chinese in, English out. Nothing here should be run, built, or installed — there is nothing to run.

## When you're done (or making a checkpoint)

Commit and push `translated/ships_building_missions_v27x_translated.json` (partial progress is fine and encouraged — commit per category rather than one giant commit at the end). The results get pulled down separately and applied to the real game files on the original machine.
