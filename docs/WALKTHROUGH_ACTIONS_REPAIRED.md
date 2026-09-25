# Walkthrough Actions Repaired

## Quest Action Graph Status

**Status:** Canonical quest action graph is EMPTY (0 rows).

This does NOT mean WALKTHROUGH.md is invalid. The walkthrough uses different evidence:

- Monster levels (`monster.edt` field2) → `BINARY_CONFIRMED`
- Map assignments (`map_spawns`) → `CLIENT_FACT`
- Equipment slots (`iteminfo.edt` type field) → `BINARY_CONFIRMED`
- Progression bands (calculated) → `DERIVED`

## Old Model (WRONG — Historical Context)

```
walkthrough_actions: 18,466 rows (DEPRECATED)
- Built using NodeIndex-as-QuestID
- TIDAK BOLEH dianggap valid
```

## Why Old Model Was Wrong

- Used NodeIndex as QuestID
- Inferred action_type (TALK_NPC, GO_MAP, KILL_MONSTER) without evidence
- No consumer runtime found

## Current Model

```
walkthrough_actions: 0 rows (not populated)
- Quest chain: UNRESOLVED
- Quest → Monster: UNRESOLVED
- Quest → NPC semantic: UNRESOLVED
```

## WALKTHROUGH.md vs Quest Action Graph

| Area | Status | Source |
|---|---|---|
| Monster per level band | ✅ Exists | `monster_progression` (DERIVED from binary) |
| Map per level band | ✅ Exists | `map_progression_candidates` (DERIVED) |
| Equipment per level | ✅ Exists | `equipment_progression` (BINARY_CONFIRMED) |
| Quest objectives | ❌ `UNRESOLVED` | No monster reference in quest.edt |
| Quest chain | ❌ `UNRESOLVED` | No flag transition consumer |
| Quest → NPC semantic | ❌ `UNRESOLVED` | ID equality only |

**WALKTHROUGH.md uses only evidence-backed data.** Quest action graph is a separate research goal.

## Evidence Legend

| Label | Meaning |
|---|---|
| `BINARY_CONFIRMED` | Langsung dari client binary |
| `CLIENT_FACT` | Dari client, parsed facts |
| `DERIVED` | Dihitung dari data lain |
| `PROBABLE` | Correlation, no consumer runtime |
| `UNRESOLVED` | Tidak ada evidence |
| `DEFERRED` | Tidak dikerjakan |
