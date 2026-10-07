# One Piece Longwang — cloud translation batches

Personal fan-translation project. This small repo exists for exactly one purpose: translate batches of Chinese game text to English using a Claude cloud session, so the heavy text work doesn't compete with the main local session that has access to the actual game files.

## Current task: batch 5 (4 source files, about 338k Chinese characters)

1. Read `TRANSLATION_GUIDE.md` in full (order of the files, content, rules).
2. Translate, in order:
   - `source/batch5a_sea_mode_ui_v274_source.json`;
   - `source/batch5b_screens_ui_v274_source.json`;
   - `source/batch5d_npc_tutorial_v274_source.json`;
   - `source/batch5c_skills_v274_source.json`.
3. For each, write `translated/<same name>_translated.json`. Commit and push after each file, and more often inside big ones.

## Folder layout

- `TRANSLATION_GUIDE.md` — guide for the current batch. Read it first, in full.
- `source/` — Chinese text. The 4 `batch5*` files are the current task. Earlier sources (batch 3/4, ships) are finished.
- `glossary/game_terms_v271.json`, `glossary/established_glossary.json` — terminology already used in the game.
- `reference/` — a completed early batch, kept as a style/format example.
- `translated/` — outputs. Earlier outputs are already applied in the game: do not modify them.

## What this repo does NOT contain, on purpose

No APK, no Unity asset bundles (`.ab` files), no emulator/ADB tooling. Text only: Chinese in, English out. Nothing here should be run, built, or installed.

## When you're done (or making a checkpoint)

Commit and push the `translated/batch5*_translated.json` files (partial progress is fine). The results get pulled down separately and applied to the real game files on the original machine.
