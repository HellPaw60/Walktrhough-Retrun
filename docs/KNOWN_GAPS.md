# Known Gaps

## Unresolved

| Area | Evidence | Reason |
|---|---|---|
| Quest chain | `UNRESOLVED` | No flag transition consumer found |
| Quest → NPC | `UNRESOLVED` | Equality only, no runtime evidence |
| Quest → Monster | `UNRESOLVED` | No exact reference in binary |
| 1,635 NPC names | `UNRESOLVED` | Not found in `monsters.edt` or client string table |
| Skill semantics | `UNRESOLVED` | Binary SkillFile v7 format not parsed; effect mapping unknown |
| Actual drop source | `UNRESOLVED` | `drop.py` schema exists, `drop*.scr` file not found in client |
| DropRate | `DEFERRED` | Not investigated per project scope |

## Resolved (Not Gaps)

| Area | Evidence | Detail |
|---|---|---|
| NPC identities | `CLIENT_FACT` | 1,928 IDs from `npc_locations`, `npc_dialog`, `quest_dialog_nodes` |
| NPC placements | `CLIENT_FACT` | 1,300 placements from `map/npc*.edt` |
| NPC names | `PROBABLE` | 293 resolved from `monsters.edt` |
| Quest descriptions | `CLIENT_FACT` | 717 records from `flag.edt` |

## Known Limitations

- **NPC names:** 293 of 1,928 resolved. Names from `monsters.edt` (correlation, no consumer runtime).
- **Skill descriptions:** Binary SkillFile v7 format not parsed; skills table = 0 rows in SQLite.
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
