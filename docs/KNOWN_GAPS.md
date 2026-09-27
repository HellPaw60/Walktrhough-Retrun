# Known Gaps

## Unresolved

| Area | Evidence | Reason |
|---|---|---|
| Quest chain | `UNRESOLVED` | No flag transition consumer found; script.dat/scripts.dat (4.7MB) not yet decodable — format unknown |
| Quest → Monster | `UNRESOLVED` | No exact reference in binary; action_id (10,845 unique) has no definition table |
| 1,635 NPC names | `UNRESOLVED` (client) | Client sources exhausted (npctalk = dialog only, no name table); wiki names available as `EXTERNAL_REFERENCE` for all 1,635 |
| Exact 10→11 skill-tier gameplay meaning | `PROBABLE` | Reset-and-spike + tab structure (skilltree.men) strengthen; no runtime confirmation |
| Direct v7 skill loader | `UNRESOLVED` | SO3DPlus.exe packed (`.text` virtual-only, entropy 8.00, no RTTI); sealres.dll only references skill.edt (v4) |
| DropRate roll semantics | `PROBABLE` | Rate values `BINARY_CONFIRMED` in drop_1/2/3.edt; per-million cumulative interpretation not runtime-confirmed |

## Resolved (Not Gaps)

| Area | Evidence | Detail |
|---|---|---|
| Skill tree integration | COMPLETE | Prerequisite graph (362 nodes, 284 edges, 0 cycles) built and integrated into WALKTHROUGH.md as player-facing skill paths |
| Drop data integration | COMPLETE | Client drop tables (drop_1/2/3.edt) integrated into WALKTHROUGH.md as Notable Drops per level band (72 monsters, ~194 named items) |
| Quest → NPC | `PROBABLE` | 3,674 candidates evaluated; 1,235 valid rows after removing false positives (NPC 2/3 = monsters); 94 NPC IDs with textual evidence (names in quest dialog); 10 NPC IDs with multi-layer corroboration (text + dialog + location); no runtime consumer found |
| NPC identities | `CLIENT_FACT` | 1,928 IDs from `npc_locations`, `npc_dialog`, `quest_dialog_nodes` |
| NPC placements | `CLIENT_FACT` | 1,300 placements from `map/npc*.edt` |
| NPC names | `PROBABLE` | 293 resolved from `monsters.edt` |
| Quest descriptions | `CLIENT_FACT` | 717 records from `flag.edt` |
| Skill binary structure | `BINARY_CONFIRMED` | 362 records/file × 20 files; fixed 38-field layout; chain parse exact EOF 20/20 |
Skill semantic mapping | `BINARY_CONFIRMED` / `PROBABLE` | field[23]/field[27]/field[30]/field[31]/field[37] → buff.edt (all `BINARY_CONFIRMED` via cross-table joins: 103/103, 20/20, 3/3, 5/5, 2/2); prereq chains, element, projectile, variant axis remain `PROBABLE` |
| Skill variant axis | `PROBABLE` | skill01–20 = level/rank variants; files 11–20 = second tier (reset+spike); uskill01–20 = extended family |
| Actual drop source | `BINARY_CONFIRMED` | `minimap/drop_1.edt` (4,006 rows) + drop_2/3 — pipe-delimited (item_id, cumulative_rate); monster field13 → row N; Piya→Piya's Egg verified 7/7 vs wiki |
| field[30] buff_3_id | `BINARY_CONFIRMED` | 3/3 nonzero join buff.edt (Shatter Armor/Time Bomb→급습, Blade Waltz→제압1) |
| field[31] buff_4_id | `BINARY_CONFIRMED` | 5/5 float-decoded join buff.edt (296, 951, 1038, 1258) |
| field[37] buff_5_id | `BINARY_CONFIRMED` | 2/2 join buff.edt (Throw Shield→쾌검, Disintegrate→토네이도1) |
| field[0] job tree structure | `CLIENT_FACT` | skilltree.men UI memuat job_001–job_231 persis nilai field[0]; tab structure N/N+10/N+20; 2ndjobLock element |

## Not Yet Built

| Area | Reason |
|---|---|
| Player skill recommendations | Chain structure available; build/rating recommendations require gameplay evidence beyond dependency structure |

## Known Limitations

- **NPC names:** 293 of 1,928 resolved. Names from `monsters.edt` (correlation, no consumer runtime).
- **Skill descriptions:** RESOLVED — SkillFile v7 fully parsed (7,240 records; 362/file × 20 files; 344 named + 18 unnamed). Semantic mapping substantially decoded via cross-build loader schema, prerequisite chains, buff.edt joins (103/103 + 20/20), element clustering, and projectile behavior. Direct v7 consumer remains unavailable (SO3DPlus.exe packed). See `SKILL_V7_SEMANTIC_RESEARCH.md`.
- **Quest chain:** `quest_node_mapping` uses talk_id equality (PROBABLE, not consumer-tested).
- **Drop tables:** `BINARY_CONFIRMED` — client drop tables tersedia di `minimap/drop_1.edt`, `drop_2.edt`, `drop_3.edt`. DropRate values `BINARY_CONFIRMED`; exact runtime roll semantics remain `PROBABLE`. Wiki hanya digunakan sebagai cross-reference semantic verification.
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
