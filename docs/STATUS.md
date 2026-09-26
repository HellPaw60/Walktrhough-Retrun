# Project Status

**Last Updated:** 2026-09-26

## Validation Status

| Phase | Result |
|---|---|
| A — Audit | COMPLETE |
| B1 — Repair | COMPLETE |
| C1 — Validation | COMPLETE |
| B2 — Repair | COMPLETE |
| C2 — Validation | COMPLETE |
| D — GitHub Sync | COMPLETE |
| Skill v7 binary parsing | COMPLETE |
| Skill v7 semantic research | COMPLETE |

## Walkthrough Coverage

| Level Band | Monster/Map Evidence | Equipment Evidence |
|---|---|---|
| L1–9 (BEGINNER) | `DERIVED` | `BINARY_CONFIRMED` |
| L10–19 (EARLY) | `DERIVED` | `BINARY_CONFIRMED` |
| L20–29 (DEVELOPING) | `DERIVED` | `BINARY_CONFIRMED` |
| L30–39 (MID) | `DERIVED` | `BINARY_CONFIRMED` |
| L40–49 (PROGRESSION) | `DERIVED` | `BINARY_CONFIRMED` |
| L50–59 (LATE) | `DERIVED` | `BINARY_CONFIRMED` |
| L60–74 (ENDGAME) | `DERIVED` | `BINARY_CONFIRMED` |
| L75–100 (LEGEND) | `DERIVED` | `BINARY_CONFIRMED` |

## Canonical Numbers

| Table | Count | Evidence |
|---|---|---|
| monsters | 9,999 | `BINARY_CONFIRMED` |
| items | 16,318 | `BINARY_CONFIRMED` |
| quest_identity | 717 | `CLIENT_FACT` |
| quest_dialog_nodes | 39,950 | `CLIENT_FACT` |
| npc_identity | 1,928 | `CLIENT_FACT` |
| resolved NPC | 293 | `PROBABLE` |
| unresolved NPC | 1,635 | `UNRESOLVED` |
| npc_locations | 1,300 | `CLIENT_FACT` |
| npc_dialog | 793 | `CLIENT_FACT` |
| monster_progression | 5,161 | `DERIVED` |
| map_progression_candidates | 40 | `DERIVED` |
| map_progression_graph | 1,247 | `DERIVED` |
| equipment_progression | 125 | `BINARY_CONFIRMED` |
| skill_records | 7,240 | `BINARY_CONFIRMED` |
| skill_named_records | 344 | `BINARY_CONFIRMED` |
| skill_unnamed_records | 18 | `BINARY_CONFIRMED` |
| skill_ids | 1–362 | `BINARY_CONFIRMED` |
| skill_header_max_id | 363 | `BINARY_CONFIRMED` |

## Key Facts

| Fact | Value | Evidence |
|---|---|---|
| MonsterID 1 | Piya | `BINARY_CONFIRMED` |
| MonsterID 22 | Rascal Rabbit | `BINARY_CONFIRMED` |
| NPC Joan | ID 4429 | `PROBABLE` |
| NPC Arus | ID 4441 | `PROBABLE` |
| NPC Duran | ID 5288 | `PROBABLE` |
| NPC Hanaiel | ID 5690 | `PROBABLE` |

## Unresolved / Deferred

| Area | Status |
|---|---|
| Quest chain | `UNRESOLVED` |
| Quest → Monster | `UNRESOLVED` |
| Drop table | `UNRESOLVED` |
| DropRate | `DEFERRED` |
| Skill binary structure | COMPLETE |
| Skill semantic mapping | SUBSTANTIALLY DECODED |
| Direct v7 loader | `UNRESOLVED` (NOT FOUND — SO3DPlus.exe packed) |
| Skill tree integration | NOT YET DONE |
| Exact 10→11 gameplay meaning | `PROBABLE` / UNCONFIRMED |
| field[30]/field[31] (buff_3/buff_4) | `UNRESOLVED` |
| field[37] | `UNRESOLVED` |
| Map connection | `UNRESOLVED` |
| Quest objective | `UNRESOLVED` |
| 1,635 NPC names | `UNRESOLVED` |

## Not Unresolved

| Area | Status | Reason |
|---|---|---|
| Quest → NPC | `PROBABLE` | 3,674 candidates evaluated; 1,235 valid rows; 94 NPC IDs with textual evidence (names in quest dialog); 10 NPC IDs with multi-layer corroboration (text + dialog + location); no runtime consumer found |
| NPC locations | `CLIENT_FACT` | 1,300 placements extracted from `npc*.edt` |
| NPC identities | `CLIENT_FACT` | 1,928 IDs from client (dialog/location/quest refs) |
| Quest descriptions | `CLIENT_FACT` | 717 records from `flag.edt` |

## Repository

- **Walkthrough:** https://github.com/HellPaw60/Walktrhough-Retrun
- **Research docs:** `docs/` in this repository (SKILLFILE_V7_RESEARCH.md, SKILL_V7_PARSER_VALIDATION.md, SKILL_V7_VARIANT_ANALYSIS.md, SKILL_V7_SEMANTIC_RESEARCH.md, dll.)
- **Source/data workspace:** `D:\SealR_Database` (local only — raw extraction, parsers, SQLite, CSV artifacts)
