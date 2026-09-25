# NPC Identity Execution Report

## Scope

Resolve NPC identity (names) for all NPCs that have dialog or placement data, and investigate Quest → NPC relationships.

## Source Files

| File | Description |
|---|---|
| `etc/npctalk.edt` | NPC dialog data (254 unique NPCs, 793 dialog entries) |
| `minimap/npc*.edt` | NPC map placements (111 files, 51 unique NPCs, 1,300 placements) |
| `monster.edt` | Monster database (9,999 records, 31×int64 schema) |
| `quest.edt` | Quest dialog (QuestFile v5, 39,950 nodes) |
| `flag.edt` | Quest flags (QuestFlagFile v1, 717 records) |

## Evidence Hierarchy

1. `BINARY_CONFIRMED` — Direct binary field with proven schema (monster level field2, item type field, etc.)
2. `CLIENT_FACT` — NPC identity existence (ID from client references: locations, dialog, quest nodes)
3. `PROBABLE` — NPC name resolution (293 names from `monsters.edt`, correlation without runtime consumer)
4. `PROBABLE` — Structural correlation (talk_id/group_id/action_id ↔ NPC ID overlap)
5. `UNRESOLVED` — No evidence found

## NPC Names Resolved (Historical State)

| NPC ID | Name | Source |
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

### Historical State

| Metric | Before Repair | After Repair (Current Canonical) |
|---|---|---|
| npc_identity total | 293 rows (INCORRECT) | 1,928 rows |
| Resolved names | 293 | 293 |
| Unresolved names | 0 (INCORRECT — NPCs dropped) | 1,635 |

**Root Cause:** Previous execution deleted all npc_identity rows and only recreated NPCs with dialog/location/names, dropping 1,635 valid NPC identities. Commit b2dcd5ba incorrectly reduced from 1,928 → 293.

**Current Canonical State:** 1,928 total = 293 resolved + 1,635 unresolved.

**Evidence:**
- Identity exists (1,928): `CLIENT_FACT` — from `npc_locations` (1,300), `npc_dialog` (793), `quest_dialog_nodes` (group_id)
- Names resolved (293): `PROBABLE` — from `monsters.edt`, no consumer runtime confirms semantic NPC identity
- Names unresolved (1,635): `UNRESOLVED` — not found in `monsters.edt` or client string tables

## Quest → NPC Evidence

| Evidence | Finding | Evidence |
|---|---|---|
| talk_id ↔ NPC ID overlap | 275 NPCs share ID with talk_id | `PROBABLE` |
| group_id ↔ NPC ID overlap | 174 NPCs share ID with group_id | `PROBABLE` |
| action_id ↔ NPC ID | 289 NPCs share ID with action_id | `PROBABLE` |

**Decision:** Created `quest_npc_candidates` table with 3,674 rows.

**Not promoted to `quest_npcs`** because:
- No explicit client field links quest to NPC
- No runtime consumer found
- ID overlap is correlation, not confirmed semantic relationship

**Semantic Quest → NPC evidence:** `UNRESOLVED`

## Confidence Classification

| Level | Count | Description |
|---|---|---|
| `CLIENT_FACT` | 1,928 | NPC identity exists (ID from client references: locations, dialog, quest nodes) |
| `PROBABLE` | 293 | NPC name from `monsters.edt` (correlation, no runtime consumer) |
| `PROBABLE` | 3,674 | Quest ↔ NPC ID overlap (structural correlation) |
| `UNRESOLVED` | 1,635 | NPC names not found in client |

## Validation Results

| Test | Result |
|---|---|
| MonsterID 1 = Piya | PASS |
| MonsterID 22 = Rascal Rabbit | PASS |
| loot_entries = 0 | PASS |
| DropRate = `DEFERRED` | PASS |
| npc_dialog unchanged (793) | PASS |
| npc_locations unchanged (1,300) | PASS |
| monster_progression unchanged (5,161) | PASS |
| equipment_progression unchanged (125) | PASS |
| player_progression_v2 unchanged (8) | PASS |
| map_candidates unchanged (40) | PASS |
| map_graph unchanged (1,247) | PASS |

## Remaining Gaps

| Area | Evidence |
|---|---|
| Quest → NPC semantic | `UNRESOLVED` (correlation only) |
| Quest chain | `UNRESOLVED` |
| Quest → Monster | `UNRESOLVED` |
| 1,635 NPC names | `UNRESOLVED` (no data in client) |
| Skill binary | `UNRESOLVED` |
| Actual drop source | `UNRESOLVED` |
| DropRate | `DEFERRED` |

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
