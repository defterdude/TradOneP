# Translation guide — Batch 5

Personal fan-translation patch for the Chinese mobile RPG **One Piece Burning Will** (航海王：燃烧意志), private server "Longwang". Batches 2–4 are done and applied in the game. Batch 5 covers what is still Chinese. Not included, on purpose: main story quests, side quests and main cutscene texts.

Work through the source files **in this order**, and commit and push after each file (inside the big ones, every few hundred rows):

| # | Source file | Output file | Content | ≈ Chinese chars |
|---|---|---|---|---|
| 1 | `source/batch5a_sea_mode_ui_v274_source.json` | `translated/batch5a_sea_mode_ui_v274_translated.json` | sea mode screens: sea areas, exploration, seaport, sailing | 2k |
| 2 | `source/batch5b_screens_ui_v274_source.json` | `translated/batch5b_screens_ui_v274_translated.json` | every other screen still in Chinese, 6,508 texts | 43k |
| 3 | `source/batch5d_npc_tutorial_v274_source.json` | `translated/batch5d_npc_tutorial_v274_translated.json` | NPC ambient bubble lines in towns, tutorial bubbles | 24k |
| 4 | `source/batch5c_skills_v274_source.json` | `translated/batch5c_skills_v274_translated.json` | skill names and descriptions, boss skills, buffs, talents, awakening | 269k |

## Screen texts (`ui_texts`, files 5a and 5b)

Each item is `{bundle, gameobject, path_id, zh}`. Dropdown entries also have `option_index`. Add `en` to every item and keep the other fields verbatim.

- **Context:** `bundle` names the screen (e.g. `restobundle/pages/seaareapage.ab` = the sea map HUD) and `gameobject` names the widget (`Title`, `btnText`, `Desc`…).
- **Length:** buttons, tabs and titles are small widgets, so keep them 1–3 words, Title Case, as short as the Chinese. Example: 炮击 = "Fire" (the cannon button of the sea HUD).
- **Line breaks:** keep exactly the same number of real line breaks as `zh`. A text with no line break gets none; a text with one gets exactly one.
- **Placeholders:** some texts are developer placeholders filled at runtime (e.g. 文本, 名字七个字, 1天23小时). Still translate them literally and keep their numbers.
- **Consistency:** the same `zh` in several screens should usually get the same `en`.

## NPC lines and tutorials (file 5d)

- `TableSceneNpc.defaultDialog`: the speech bubble a town NPC says when you walk by. It is short flavor text, not quest dialogue. Keep the character's voice.
- `TableGuideStepExtra.textModuleId`: tutorial bubble texts ("Tap here to…"). Despite the column name, the Chinese cells are display text.

## Skills (file 5c)

- `TableSkill.skillName` (skill names) and `skillDesc` (descriptions); boss skills; buffs; talents; hero awakening; camps…
- Many rows have one column already in English (often `skillDesc`, written with ideographic spaces `　`). **Copy those cells unchanged**: only Chinese cells are translated.
- Descriptions are full of markup: `<color=#d14e0d>{atkper}%</color>`. Keep every `{name}` placeholder, every tag pair and every number exactly.
- Follow the style of the existing English descriptions in the same file, e.g. "Deals `{atkper}%` of ATK as skill damage to four enemies in a straight line, plus `{atkval}` fixed damage."
- Skill names: short, punchy, in One Piece style. Keep official attack names when they exist (e.g. 橡胶火箭炮 = "Gum-Gum Bazooka").

## Source format (data files 5c and 5d)

Same as the earlier batches:
- `tables.<Table>` with `key_column` (copy verbatim), `text_columns` and `rows`.
- When the two bundles differ, also `other_bundle_rows`. These are only the `tabledata_v1.ab` rows that are missing from `rows` or whose text differs: translate them too. Identical v1 rows are not repeated and reuse your translation.

## Strict rules (all files)

1. **Markup tokens stay exactly as in the source:**
   - `<size=..>`, `<color=#RRGGBB>…</color>`, `<b>`;
   - placeholders `{0}` `{name}` `${0}$`;
   - the two characters `\n` (literal backslash + n) in table cells;
   - bullets `*`;
   - every number and percentage.
2. **Spaces:** write normal spaces in new translations. The local build converts them when needed.
3. **Terminology:**
   - Use `glossary/game_terms_v271.json` first (names exactly as the player already sees them in the game), then `glossary/established_glossary.json`.
   - The same Chinese term always gets the same English.
   - Fixed by the player: 海岛争霸 = "Island Conquest", 航线模拟战 = "Voyage Tower", 航线通行证 = "Voyage Tower Pass".
4. **One Piece names:** official English spellings.
5. **Empty strings stay empty.**
6. **No Chinese characters** may remain in any `en` value or translated cell.
7. **Uncertainty:** if something is genuinely ambiguous, choose the best reading and record it in `uncertain_notes`. Never drop part of a string.

## Output format

The output mirrors the source:
- UI files keep the same `ui_texts` list, with `en` added to each item.
- Data files keep the same `tables` structure, with `rows` and `other_bundle_rows` translated.

Add `glossary_additions` and `uncertain_notes`.

## What this repo is NOT for

Text only. Nothing to build, patch, install or verify here: that happens locally on the original machine.
