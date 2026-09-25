# Evidence Policy

## Hierarchy

1. **Binary field with proven schema** → `BINARY_CONFIRMED`
2. **Parser/schema with proven semantic** → `CLIENT_FACT`
3. **Client consumer reference** → `DERIVED`
4. **Structural correlation** → `PROBABLE`
5. **External reference** → `EXTERNAL_REFERENCE`

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
