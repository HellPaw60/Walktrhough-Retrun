# Player Progression Model

## Canonical ID Verification

| ID | Name | Level | Evidence |
|---|---|---|---|
| 1 | Piya | 1 | `BINARY_CONFIRMED` |
| 22 | Rascal Rabbit | 8 | `BINARY_CONFIRMED` |
| 23 | Rascal Rabbit | 8 | `BINARY_CONFIRMED` |

**Verification:** `SELECT id, name, level FROM monsters WHERE id IN (1, 22, 23)` — Confirmed via 31×int64 binary schema

## 3-Layer Architecture

| Layer | Description | Evidence |
|---|---|---|
| A - CLIENT FACTS | Data extracted dari client binary | `CLIENT_FACT` |
| B - DERIVED PROGRESSION | Dihitung dari Layer A | `DERIVED` |
| C - EXTERNAL REFERENCE | Wiki/Gameplay knowledge | `EXTERNAL_REFERENCE` |

## Level Bands

| Band | Level | Map Evidence | Equipment Evidence |
|---|---|---|---|
| Beginner | 1-9 | `DERIVED` | `BINARY_CONFIRMED` |
| Early | 10-19 | `DERIVED` | `BINARY_CONFIRMED` |
| Developing | 20-29 | `DERIVED` | `BINARY_CONFIRMED` |
| Mid | 30-39 | `DERIVED` | `BINARY_CONFIRMED` |
| Progression | 40-49 | `DERIVED` | `BINARY_CONFIRMED` |
| Late | 50-59 | `DERIVED` | `BINARY_CONFIRMED` |
| Endgame | 60-74 | `DERIVED` | `BINARY_CONFIRMED` |
| Legend | 75-100 | `DERIVED` | `BINARY_CONFIRMED` |

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

## Source Priority

1. Binary/schema terbukti
2. Parsed client facts
3. Derived data
4. Structural correlation
5. External reference
