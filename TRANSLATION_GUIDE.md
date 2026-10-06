# Translation guide — Batch 3 (`batch3_v272`)

## Game context

Personal fan-translation patch for the Chinese mobile RPG **One Piece Burning Will** (航海王：燃烧意志), private server "Longwang". Most of the game is already in English; this batch covers what the player meets next:

1. **`captains_guide`**: the in-game **Captain's Guide** (船长指南), a help menu.
   - Six categories: Team 战队, Characters 角色, Squad 队伍, Warship 战船, Equipment 装备, Gems 宝石.
   - Each category has topics ("Team Level Up", "Get Characters"…).
   - For each topic, a list of game modes where you progress, with a one-line explanation.
2. **`voyage_guide_old`**: the older **Voyage Guide** page (航海指引), the same kind of text.
3. **`island_conquest`**: **Island Conquest** (海岛争霸), the game's auto-battler mode, similar to Auto Chess or Teamfight Tactics.
   - 8 players; each round you buy heroes with gold from a shop.
   - Three identical heroes merge into a higher star.
   - You place heroes on a board and level up your warship (more heroes, better shop odds).
   - Synergy bonds ("groups", 羁绊) give bonuses.
   - Fights run automatically against monsters or other players.
   - The tables hold tiers and ranks (初心9段…), bonds and their buffs, weekly tasks, rewards and system messages (`TableText` rows whose key contains AutoChess/Chess…).
4. **`item_descriptions`**: `TableItem` rows whose `name` or `intro` is still Chinese, about 1,850 items (bag tooltips).
5. **`ui_texts`**: single Text components of page prefabs.
   - The Captain's Guide title 船长指南 = "Captain's Guide".
   - All the Island Conquest pages (main page, matching, shop, results, codex, tutorials…).

## Source data

`source/batch3_v272_source.json`:
- `header.tables_by_category` lists the tables per category.
- `tables.<category>.<Table>`:
  - `key_column`: identifies the row. Copy it verbatim, never translate it.
  - `text_columns`: fields to translate. Some cells in them may already be English: keep those as they are.
  - `context_columns`: shown for context only, never changed. Example: `TableItem.name`, when already English, tells you what the item is.
  - `rows`: the text of `tabledata.ab`. Only rows that still contain Chinese were exported.
- `ui_texts`: `{bundle, gameobject, path_id, zh}`. Translate `zh` into a new field `en`.

## Critical: the two-bundle rule (same as earlier batches)

The game ships two data files, `tabledata.ab` and `tabledata_v1.ab`, which are not identical for several tables here. When a table has `content_diverges_between_bundles: true`, it also has `other_bundle_rows`, the `tabledata_v1.ab` text with its own row set and wording. Examples:
- TableNewStrongGuideType has 6 categories in one bundle and 7 in the other.
- TableItem has 1,863 rows in one and 1,584 in the other.

Translate `rows` and `other_bundle_rows` independently and completely. `divergence_detail` lists the keys found in only one bundle and the keys whose text differs. When a row is identical in both bundles, give it the identical translation.

## Strict rules

1. **Markup tokens stay exactly as in the source:**
   - `<size=..>…</size>`, `<color=#RRGGBB>…</color>`, `<b>…</b>`;
   - placeholders `{0}`, `{1}`, `${0}$`;
   - the two characters `\n` (literal backslash + n in the cells);
   - `*` bullet markers;
   - every number and percentage.

   Only the words change. Example: `获取<size=28>蓝钻</size>` → `Get <size=28>Blue Diamonds</size>`. Do not add `\n` where the source has none (line wrapping is handled locally).
2. **Spaces:** write normal spaces. The local build converts them to the spacing the game needs.
3. **Labels** (names, titles, category and topic names, tier names, button texts): 1–3 words, Title Case, as short as possible.
4. **Descriptions and tooltips:** short sentences in sentence case, terse mobile-game style.
5. **Terminology:**
   - First use `glossary/game_terms_v271.json`: names exactly as the player already sees them in the game, including hero and item names.
   - Then use `glossary/established_glossary.json`.
   - The same Chinese term always gets the same English, across categories and bundles.
   - Fixed by the player: 海岛争霸 = "Island Conquest", 航线模拟战 = "Voyage Tower", 航线通行证 = "Voyage Tower Pass".
   - Suggested: 战队 = "Team" (account level), 队伍 = "Squad", 战船 = "Warship", 角色 = "Character(s)", 装备 = "Equipment", 宝石 = "Gems", 羁绊 = "Bond", 段位 = "Tier", 段 = (tier) "Rank" (e.g. 初心9段 → "Novice 9"), 争霸币 = "Conquest Coin" (already in game), 金币 (inside Island Conquest) = "Gold".
6. **One Piece names:** official English spellings (Luffy, Zoro, Kaido, Big Mom, Garp, Whitebeard…). Match `game_terms_v271.json` when the name is there.
7. **Empty strings stay empty.** Developer placeholders (e.g. 提示文字提示文字, …需要策划Text填写) are still translated literally.
8. **No Chinese characters** may remain in any translated value.
9. **Uncertainty:** if a term is genuinely ambiguous, pick the best reading and record it in `uncertain_notes`. Never silently drop part of a string.

## Output format

`translated/batch3_v272_translated.json` mirrors the source:
- `tables.<category>.<Table>` with the same `key_column`, `text_columns`, `context_columns`, and `rows`. It also has `other_bundle_rows` when the source has it.
- Key and context columns are copied verbatim; every text column is translated.
- `ui_texts`: the same list, each item with an added `en`.
- `glossary_additions`: new term choices (Chinese → English).
- `uncertain_notes`: anything ambiguous.

## Suggested order (commit after each step; partial progress is welcome)

1. `captains_guide` + `voyage_guide_old` + the 船长指南 ui text: small, do first.
2. `island_conquest` tables + all Island Conquest `ui_texts`.
3. `item_descriptions` (`TableItem`): the big one, about 1,850 + 1,580 rows. Commit every few hundred rows.

## What this repo is NOT for

Text only. No game binaries or bundles. Nothing to build, patch, install or verify here: that happens locally on the original machine.
