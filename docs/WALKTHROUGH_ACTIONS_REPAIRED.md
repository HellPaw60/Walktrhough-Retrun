# Walkthrough Actions Repaired

## Status: EMPTY (no valid walkthrough yet)

## Old Model (WRONG)
```
walkthrough_actions: 18,466 rows
- dibangun dari NodeIndex-as-QuestID
- TIDAK BOLEH dianggap valid
```

## Why It Was Wrong
- walkthrough_actions lama memakai quest_id yang sebenarnya NodeIndex
- action_type (TALK_NPC, GO_MAP, KILL_MONSTER, COLLECT_ITEM) diinferensikan dari raw fields tanpa bukti

## New Model
```
walkthrough_actions:
  quest_id TEXT            -- canonical (FLAG-based, NOT NodeIndex)
  seq INTEGER              -- sequence in quest
  action_type TEXT
  target_type TEXT
  target_id INTEGER
  npc_id INTEGER
  map_id INTEGER
  monster_id INTEGER
  item_id INTEGER
  quantity INTEGER
  source TEXT
  evidence TEXT
  confidence TEXT
```

## Current State
- walkthrough_actions: 0 rows (no valid graph yet)

## Requirement for Walkthrough
1. Canonical Quest ID ✅
2. Quest → Node Mapping ✅
3. Quest → NPC ⚠️ (PROBABLE equality)
4. Quest → Item ✅ (PROBABLE)
5. Quest → Monster ❌ (UNRESOLVED)
6. Quest Chain ❌ (UNRESOLVED)

## Recommendation
Walkthrough final TIDAK BOLEH dibuat sebelum:
- Quest chain terbukti (flag transition)
- Monster reference terbukti
- NPC mapping terbukti
