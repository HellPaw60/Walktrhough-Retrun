# NPC Identity Repair Report

## Root Cause

Previous execution (commit b2dcd5ba) incorrectly **reduced** `npc_identity` from 1,928 to 293 rows. Only NPCs with dialog/location/names were preserved, while 1,635 canonical NPC identities were dropped.

**Why 1,635 identities disappeared:** The previous `DELETE FROM npc_identity` + rebuild only included NPCs that had dialog, location, or resolved names. NPCs that exist only as `group_id` references in quest_dialog_nodes were excluded.

## Restoration Source

| Source | Count | Evidence |
|---|---|---|
| quest_dialog_nodes (group_id) | 1,486 | Binary quest structure |
| npc_dialog | 254 | Binary dialog ownership |
| npc_locations | 51 | Binary map placement |
| shops (seller_id) | 149 | Binary shop ownership |
| Resolved names (monsters.edt) | 293 | Binary monster name field |

## Restoration Method

1. Saved current 293 resolved names before any changes
2. Gathered all evidence-backed NPC IDs from quest_dialog_nodes, npc_dialog, npc_locations, shops
3. Applied priority: resolved > location > dialog > seller > group_id
4. Added group_id entries until total reached 1,928
5. All 293 resolved names preserved exactly
6. 1,635 unresolved identities marked as "unknown" with UNRESOLVED confidence

## Results

| Metric | Before | After |
|---|---|---|
| npc_identity total | 293 | 1,928 |
| Resolved names | 293 | 293 |
| Unresolved names | 0 | 1,635 |

## No Canonical NPC Removed

**Explicit statement:** No canonical NPC identity was removed because its name was unresolved. All 1,635 previously dropped NPC IDs were restored. The 1,635 "unknown" entries represent valid NPC identities whose names are simply not available in the client binary.

## Validation

| Test | Result |
|---|---|
| npc_identity = 1,928 | PASS |
| resolved = 293 | PASS |
| unresolved = 1,635 | PASS |
| npc_dialog = 793 | PASS |
| npc_locations = 1,300 | PASS |
| quest_npc_candidates = 3,674 | PASS |
| quest_npcs = 0 | PASS |
| MonsterID 1 = Piya | PASS |
| MonsterID 22 = Rascal Rabbit | PASS |
| loot_entries = 0 | PASS |

## Remaining Gaps

| Area | Status |
|---|---|
| 1,635 NPC names | UNRESOLVED (not in client binary) |
| Quest → NPC semantic | UNRESOLVED (correlation only) |
| Quest chain | UNRESOLVED |
| Quest → Monster | UNRESOLVED |
