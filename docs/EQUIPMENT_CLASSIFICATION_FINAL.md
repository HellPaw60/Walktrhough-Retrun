# Equipment Classification Final

## Method

Items classified by `type` field from client binary (`iteminfo.edt`):

| Type | Category | Count (approx) | Notes |
|---|---|---|---|
| 1 | **MIXED** | ~113 | NOT automatically Weapon — subclassified by name heuristic and stats |
| 4 | ARMOR | — | Binary type=4 |
| 6 | SHIELD | — | Binary type=6 |
| 7 | HELMET | — | Binary type=7 |
| 9 | ACCESSORY | — | Binary type=9 |
| 11 | BOOTS | — | Binary type=11 |
| 14 | CAPE | — | Binary type=14 |
| 0,2 | Consumable | — | — |
| 3 | Material | — | — |
| 5 | Quest | — | — |
| 8 | Scroll | — | — |
| 10 | Ammo | — | — |
| 15 | Skillbook | — | — |
| 16 | Potion | — | — |
| 17 | Food | — | — |

### Type=1 Classification Detail

Type=1 is **mixed** — NOT all weapons. From 1,113 type=1 items:
- 66 classified as FOOD (by name)
- 23 classified as PLACEHOLDER
- 7 classified as POTION
- 10 classified as EQUIPMENT
- 7 classified as EVENT

**Sub-classification uses:**
1. Binary stats (ATK/MATK/DEF) from `iteminfo.edt`
2. Name heuristic (e.g., "Mudfish" → FOOD)
3. Item level from binary

**Evidence:** `BINARY_CONFIRMED` — type field from `iteminfo.edt`; sub-classification uses `CLIENT_FACT` (stats) + `PROBABLE` (name heuristic)

## Results

- Total items: 16,318
- Equipment progression: 125 entries (by slot and level band)
- Type=1 → Weapon only if stats confirm ATK>0 and name doesn't indicate consumable

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
