# Translation guide — Batches 3 and 4

Personal fan-translation patch for the Chinese mobile RPG **One Piece Burning Will** (航海王：燃烧意志), private server "Longwang". Most of the game is already in English. These batches cover the remaining Chinese text, except story dialogue, which stays untranslated on purpose and is not included.

Work through the source files **in this order**:

| # | Source file | Output file | Content | ≈ Chinese chars |
|---|---|---|---|---|
| 1 | `source/batch3_v272_source.json` | `translated/batch3_v272_translated.json` | Captain's Guide, Island Conquest, item descriptions, page texts | 81k |
| 2 | `source/batch4a_system_texts_v273_source.json` | `translated/batch4a_system_texts_v273_translated.json` | system messages, help/rule pages, mails, codex tasks | 132k |
| 3 | `source/batch4b_world_codex_v273_source.json` | `translated/batch4b_world_codex_v273_translated.json` | maps, dungeons, scenes, NPC names, codex entries, bosses | 116k |
| 4 | `source/batch4c_events_guild_misc_v273_source.json` | `translated/batch4c_events_guild_misc_v273_translated.json` | events, guild, tasks, titles, every other table | 65k |

Commit and push after each file. For big tables, commit every few hundred rows.

## Batch 3 content (`batch3_v272`)

1. **`captains_guide`**: the in-game **Captain's Guide** (船长指南), a help menu.
   - Six categories: Team 战队, Characters 角色, Squad 队伍, Warship 战船, Equipment 装备, Gems 宝石.
   - Each category has topics ("Team Level Up", "Get Characters"…).
   - For each topic, a list of game modes where you progress, with a one-line explanation.
2. **`voyage_guide_old`**: the older **Voyage Guide** page (航海指引), the same kind of text.
3. **`island_conquest`**: **Island Conquest** (海岛争霸), the game's auto-battler mode, similar to Auto Chess or Teamfight Tactics.
   - 8 players; each round you buy heroes with gold from a shop.
   - Three identical heroes merge into a higher star.
   - You place heroes on a board and level up your warship (more heroes, better shop odds).
   - Synergy bonds (羁绊) give bonuses.
   - The tables hold tiers and ranks (初心9段…), bonds and their buffs, weekly tasks, rewards and the mode's system messages.
4. **`item_descriptions`**: `TableItem` rows whose `name` or `intro` is still Chinese (bag tooltips).
5. **`ui_texts`**: single Text components of page prefabs.
   - The Captain's Guide title 船长指南 = "Captain's Guide".
   - All the Island Conquest pages.
   - Translate `zh` into a new field `en`.

## Batch 4 content (`batch4a/b/c_v273`)

- **4a, system texts:**
  - `TableText`: thousands of keyed UI messages, tips and long help or rules pages. The key often names the feature, e.g. `UnionSLG…` = guild war, `WorldpersonBOSS…` = world boss, `…Explain…` = help page.
  - `TableTextModule`; mails (`TableMailConfig`: title, content, sender; 系统 = "System").
  - Codex/handbook tasks and small system tables.
- **4b, world and codex:** dungeon run-map descriptions, raid chapters and episodes, scene and fight names, NPC names, codex (`TablePokedex` hero intros, `TableOPDex`), boss names and descriptions, hero idle lines, island trials, explorations, fortresses…
- **4c, events, guild, tasks and everything else:** activity and event tasks, guild buildings, titles, ranks, rewards, small UI tables. Here the table name and key are your best context.

The format is the same as batch 3, with a flat `tables.<Table>` and no category level. Each file's `header.tables` lists its tables.

## Source data (all files)

- `tables[.<category>].<Table>`:
  - `key_column`: identifies the row. Copy it verbatim, never translate it.
  - `text_columns`: fields to translate. A cell already in English stays as it is.
  - `context_columns` (batch 3 only): for context, never changed.
  - `rows`: the `tabledata.ab` text. Only rows still containing Chinese were exported.

## Critical: the two-bundle rule

The game ships two data files, `tabledata.ab` and `tabledata_v1.ab`. When a table has `content_diverges_between_bundles: true`, it also has `other_bundle_rows`:
- These are **only the `tabledata_v1.ab` rows that are missing from `rows` or whose text differs** (see `other_bundle_rows_note`).
- Translate every one of them independently. Same Chinese text means the same English.
- `v1` rows identical to `rows` are not repeated: they reuse your translation of `rows`.

## Strict rules

1. **Markup tokens stay exactly as in the source:**
   - `<size=..>…</size>`, `<color=#RRGGBB>…</color>`, `<b>…</b>`;
   - placeholders `{0}`, `{1}`, `${0}$`;
   - the two characters `\n` (literal backslash + n in the cells);
   - bullets `*`, separators `|` or `;` when the source has them;
   - every number and percentage.

   Only the words change. Do not add `\n` where the source has none (line wrapping is handled locally). Keep the line structure of long help texts: same `\n` count, same bullets.
2. **Spaces:** write normal spaces. The local build converts them to the spacing the game needs.
3. **Labels** (names, titles, tier names, button texts): 1–3 words, Title Case, as short as possible.
4. **Descriptions, tips and help pages:** clear short sentences, terse mobile-game style.
5. **Terminology:**
   - First use `glossary/game_terms_v271.json`: names exactly as the player already sees them in the game, including heroes, items, modes and currencies.
   - Then use `glossary/established_glossary.json`.
   - The same Chinese term always gets the same English, across files and bundles.
   - Fixed by the player: 海岛争霸 = "Island Conquest", 航线模拟战 = "Voyage Tower", 航线通行证 = "Voyage Tower Pass".
   - Suggested: 战队 = "Team" (account level), 队伍 = "Squad", 战船 = "Warship", 角色 = "Character(s)", 装备 = "Equipment", 宝石 = "Gems", 羁绊 = "Bond", 段位 = "Tier", 公会 = "Guild".
6. **One Piece names:** official English spellings (Luffy, Zoro, Kaido, Big Mom, Garp, Whitebeard…). Match `game_terms_v271.json` when the name is there.
7. **Empty strings stay empty.** Developer placeholders or notes (e.g. 提示文字, 勿删) are still translated literally.
8. **No Chinese characters** may remain in any translated value.
9. **Uncertainty:** if a term is genuinely ambiguous, pick the best reading and record it in `uncertain_notes`. Never silently drop part of a string.

## Output format (each file)

The output mirrors its source file:
- same `tables` structure, `key_column`, `text_columns`, `rows`;
- `other_bundle_rows` when the source has it;
- key columns copied verbatim, text columns translated;
- batch 3 also has `ui_texts` with an added `en`.

Add `glossary_additions` (new term choices, Chinese → English) and `uncertain_notes`.

## What this repo is NOT for

Text only. No game binaries or bundles. Nothing to build, patch, install or verify here: that happens locally on the original machine.
