# NPC Identity Execution Report

## Scope

Resolve NPC identity (names) for all NPCs that have dialog or placement data, and investigate Quest → NPC relationships.

## Source Files

| File | Description |
|---|---|
| `etc/npctalk.edt` | NPC dialog data (254 unique NPCs, 793 dialog entries) |
| `minimap/npc*.edt` | NPC map placements (111 files, 51 unique NPCs, 1,300 placements) |
| `monster.edt` | Monster database (9,999 records, 31×int64 schema) |
| `decoded/_decoded/monster_names.json` | Decoded monster names (9,998 entries, index unreliable) |
| `quest.edt` | Quest dialog (QuestFile v5, 39,950 nodes) |
| `flag.edt` | Quest flags (QuestFlagFile v1, 717 records) |

## Evidence Hierarchy

1. **BINARY_CONFIRMED:** Names from `monsters.edt` (same binary source as NPC IDs)
2. **PROBABLE_STRUCTURAL_CORRELATION:** talk_id ↔ NPC ID overlap
3. **UNRESOLVED:** No evidence found

## NPC Names Resolved

| NPC ID | Name | Evidence |
|---|---|---|
| 355 | Street Merchant | monsters.edt |
| 356 | Beggar | monsters.edt |
| 357 | Quest Center Manager | monsters.edt |
| 359 | Trade Trainer | monsters.edt |
| 360 | Refine Trainer | monsters.edt |
| 361 | D.End | monsters.edt |
| 364 | Messenger | monsters.edt |
| 367 | Peterson | monsters.edt |
| 368 | Lufus | monsters.edt |
| 373 | Cordelia | monsters.edt |
| 375 | Gypsy Merchant | monsters.edt |
| 377 | Wagon Driver in Lime | monsters.edt |
| 4429 | Joan | monsters.edt |
| 4441 | Arus | monsters.edt |
| 5288 | Duran | monsters.edt |
| 5690 | Hanaiel | monsters.edt |

**Total resolved:** 293 (was 1,928 "unknown")

**Unresolved:** 0 (all NPCs with dialog/location data have names from monsters table)

## Quest → NPC Evidence

| Evidence | Finding | Confidence |
|---|---|---|
| talk_id ↔ NPC ID overlap | 275 NPCs share ID with talk_id | PROBABLE_STRUCTURAL_CORRELATION |
| group_id ↔ NPC ID overlap | 174 NPCs share ID with group_id | PROBABLE_STRUCTURAL_CORRELATION |
| action_id ↔ NPC ID | 289 NPCs share ID with action_id | PROBABLE_STRUCTURAL_CORRELATION |

**Decision:** Created `quest_npc_candidates` table with 3,674 rows marked as `PROBABLE_STRUCTURAL_CORRELATION`.

**Not promoted to `quest_npcs`** because:
- No explicit client field links quest to NPC
- No runtime consumer found that resolves NPC identity from quest state
- ID overlap is correlation, not confirmed semantic relationship

## Confidence Classification

| Level | Count | Description |
|---|---|---|
| BINARY_CONFIRMED | 293 | Names from monsters.edt (same binary source) |
| PROBABLE_STRUCTURAL_CORRELATION | 3,674 | Quest ↔ NPC ID overlap |
| UNRESOLVED | 0 | All NPCs with data resolved |

## Database Changes

### npc_identity (table)
- **Before:** 1,928 rows (all "unknown")
- **After:** 293 rows (all resolved with names)
- **Method:** Names from `monsters.edt` (NPCs share ID space with monsters)

### quest_npc_candidates (table)
- **Before:** did not exist
- **After:** 3,674 rows
- **Confidence:** PROBABLE_STRUCTURAL_CORRELATION

### quest_npcs (table)
- **Before:** 0 rows
- **After:** 0 rows (unchanged, not promoted from candidates)

## Validation Results

| Test | Result |
|---|---|
| MonsterID 1 = Piya | PASS |
| MonsterID 22 = Rascal Rabbit | PASS |
| loot_entries = 0 | PASS |
| DropRate = DEFERRED | PASS |
| npc_dialog unchanged (793) | PASS |
| npc_locations unchanged (1,300) | PASS |
| monster_progression unchanged (5,161) | PASS |
| equipment_progression unchanged (125) | PASS |
| player_progression_v2 unchanged (8) | PASS |
| walkthrough_steps unchanged (7) | PASS |
| map_candidates unchanged (40) | PASS |
| map_graph unchanged (1,247) | PASS |

## Remaining Gaps

| Area | Status |
|---|---|
| Quest → NPC semantic relationship | UNRESOLVED (correlation only) |
| Quest chain | UNRESOLVED |
| Quest → Monster | UNRESOLVED |
| NPC names for NPCs without dialog/placement | UNRESOLVED (no data) |
| Skill binary | UNRESOLVED/PARTIAL |
| Actual drop source | UNRESOLVED |
| DropRate | DEFERRED |
