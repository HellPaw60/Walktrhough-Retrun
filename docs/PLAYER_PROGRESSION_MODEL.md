# Player Progression Model

## Canonical ID Verification

| ID | Name | Level | Source |
|---|---|---|---|
| 1 | Piya | 1 | monsters.edt |
| 22 | Rascal Rabbit | 8 | monsters.edt |
| 23 | Rascal Rabbit | 8 | monsters.edt |

**Verification:** `SELECT id, name, level FROM monsters WHERE id IN (1, 22, 23)`
**Result:** Confirmed via 31×int64 binary schema



## 3-Layer Architecture

| Layer | Description | Confidence |
|---|---|---|
| A - CLIENT FACTS | Data extracted from client binary | CONFIRMED |
| B - DERIVED PROGRESSION | Calculated from Layer A | DERIVED |
| C - EXTERNAL REFERENCE | Wiki/Legacy data | EXTERNAL |

## Level Bands

| Band | Level | Primary Map | Secondary |
|---|---|---|---|
| Beginner | 1-9 | 1 (Land's End) | 32 (Elim) |
| Early | 10-19 | - | - |
| Developing | 20-29 | - | - |
| Mid | 30-39 | - | - |
| Progression | 40-49 | - | - |
| Late | 50-59 | - | - |
| Endgame | 60-74 | - | - |
| Legend | 75-100 | - | - |

## Confidence Rules
- CLIENT_FACT: From client binary
- DERIVED: Calculated from client data
- PROBABLE: Equality observation
- EXTERNAL_REFERENCE: Wiki/legacy
