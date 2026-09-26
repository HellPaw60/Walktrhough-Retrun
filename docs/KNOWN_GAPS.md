# Known Gaps

## Unresolved

| Area | Evidence | Reason |
|---|---|---|
| Quest chain | `UNRESOLVED` | No flag transition consumer found |
| Quest → Monster | `UNRESOLVED` | No exact reference in binary |
| 1,635 NPC names | `UNRESOLVED` | Not found in `monsters.edt` or client string table |
| Exact 10→11 skill-tier gameplay meaning | `PROBABLE` | Reset-and-spike pattern established; no runtime confirmation |
| field[30]/field[31] (buff_3/buff_4) | `UNRESOLVED` | Not yet sampled against buff.edt |
| field[37] | `UNRESOLVED` | 360/362 zero; exceptions 350, 150 unexplained |
| Direct v7 skill loader | `UNRESOLVED` | SO3DPlus.exe packed; static analysis impossible |
| Actual drop source | `UNRESOLVED` | `drop.py` schema exists, `drop*.scr` file not found in client |
| DropRate | `DEFERRED` | Not investigated per project scope |

## Resolved (Not Gaps)

| Area | Evidence | Detail |
|---|---|---|
| Quest → NPC | `PROBABLE` | 3,674 candidates evaluated; 1,235 valid rows after removing false positives (NPC 2/3 = monsters); 94 NPC IDs with textual evidence (names in quest dialog); 10 NPC IDs with multi-layer corroboration (text + dialog + location); no runtime consumer found |
| NPC identities | `CLIENT_FACT` | 1,928 IDs from `npc_locations`, `npc_dialog`, `quest_dialog_nodes` |
| NPC placements | `CLIENT_FACT` | 1,300 placements from `map/npc*.edt` |
| NPC names | `PROBABLE` | 293 resolved from `monsters.edt` |
| Quest descriptions | `CLIENT_FACT` | 717 records from `flag.edt` |
| Skill binary structure | `BINARY_CONFIRMED` | 362 records/file × 20 files; fixed 38-field layout; chain parse exact EOF 20/20 |
| Skill semantic mapping | `BINARY_CONFIRMED` / `PROBABLE` | field[23]→buff.edt 103/103, field[27]→buff.edt 20/20 (`BINARY_CONFIRMED`); prereq chains, element, projectile, variant axis via cross-build v8 schema (`PROBABLE`) — see `SKILL_V7_SEMANTIC_RESEARCH.md` |
| Skill variant axis | `PROBABLE` | skill01–20 = level/rank variants; files 11–20 = second tier (reset+spike); uskill01–20 = extended family |

## Known Limitations

- **NPC names:** 293 of 1,928 resolved. Names from `monsters.edt` (correlation, no consumer runtime).
- **Skill descriptions:** RESOLVED — SkillFile v7 fully parsed (7,240 records; 362/file × 20 files; 344 named + 18 unnamed). Semantic mapping substantially decoded via cross-build loader schema, prerequisite chains, buff.edt joins (103/103 + 20/20), element clustering, and projectile behavior. Direct v7 consumer remains unavailable (SO3DPlus.exe packed). See `SKILL_V7_SEMANTIC_RESEARCH.md`.
- **Skill tree integration:** Skill data available but not yet integrated into walkthrough/player recommendations.
- **Quest chain:** `quest_node_mapping` uses talk_id equality (PROBABLE, not consumer-tested).
- **Drop tables:** Wiki used as `EXTERNAL_REFERENCE` only, not client-confirmed.
- **Map connection:** No teleporter/warp data in binary (`UNRESOLVED`).

## Evidence Legend

| Label | Meaning |
|---|---|
| `BINARY_CONFIRMED` | Langsung dari client binary |
| `CLIENT_FACT` | Dari client, parsed facts |
| `DERIVED` | Dihitung dari data lain |
| `PROBABLE` | Correlation, no consumer runtime |
| `EXTERNAL_REFERENCE` | Wiki/community/game knowledge |
| `UNRESOLVED` | Tidak ada evidence |
| `DEFERRED` | Tidak dikerjakan |
