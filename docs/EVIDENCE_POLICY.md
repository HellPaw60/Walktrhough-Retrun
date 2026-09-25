# Evidence Policy

## Hierarchy

1. `BINARY_CONFIRMED` — Binary field with proven schema (monster level field2, item type field from `iteminfo.edt`, etc.)
2. `CLIENT_FACT` — Parsed client facts with validated semantic (NPC identity from client references, map name from `map_info.edt`, quest descriptions from `flag.edt`)
3. `DERIVED` — Calculated/inferred from confirmed data (progression band from monster level distribution, monster_per_map assignment, equipment progression order)
4. `PROBABLE` — Structural correlation/equality without runtime consumer (NPC name from `monsters.edt`, quest ↔ NPC ID overlap)
5. `EXTERNAL_REFERENCE` — Wiki/community/game knowledge (job names, level requirements)
6. `UNRESOLVED` — No evidence available (quest chain, drop table, 1,635 NPC names)
7. `DEFERRED` — Not investigated (drop rate, skill effect)

## Canonical Evidence Labels

| Label | Meaning | Example |
|---|---|---|
| `BINARY_CONFIRMED` | Langsung dari client binary | item type → slot, monster level (field2), monster name |
| `CLIENT_FACT` | Dari client, parsed facts | map name, NPC locations (1,300 placements), quest descriptions (717 records) |
| `DERIVED` | Dihitung dari data lain | progression band (kluster level monster), monster_per_map assignment |
| `PROBABLE` | Correlation, tidak ada consumer runtime | NPC name dari `monsters.edt` (293 resolved) |
| `EXTERNAL_REFERENCE` | Wiki/community/game knowledge | job names (L10 first change), level requirements |
| `UNRESOLVED` | Tidak ada evidence | quest chain, drop table, 1,635 NPC names |
| `DEFERRED` | Tidak dikerjakan | drop rate, skill effect |

## Rules

- **"Best" terminology avoided.** Use "candidate", "available", "relevant" — not "RECOMMENDED" as evidence label.
- **Historical values preserved** with clear marking (`INVALIDATED`/`SUPERSEDED`).
- **Empty results documented** as `NO EVIDENCE`, not hidden.
- **Stale values in historical docs** must be in historical context only.

## Canonical IDs

| ID | Name | Evidence |
|---|---|---|
| 1 | Piya | `BINARY_CONFIRMED` |
| 22 | Rascal Rabbit | `BINARY_CONFIRMED` |
| 23 | Rascal Rabbit | `BINARY_CONFIRMED` |
