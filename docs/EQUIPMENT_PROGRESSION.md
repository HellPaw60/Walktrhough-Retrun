# Equipment Progression

## Slots

| Slot | Type Code | Evidence |
|---|---|---|
| ACCESSORY | type=9 | `BINARY_CONFIRMED` |
| ARMOR | type=4 | `BINARY_CONFIRMED` |
| BOOTS | type=11 | `BINARY_CONFIRMED` |
| CAPE | type=14 | `BINARY_CONFIRMED` |
| HELMET | type=7 | `BINARY_CONFIRMED` |
| SHIELD | type=6 | `BINARY_CONFIRMED` |

**Note:** type=1 is MIXED — not automatically weapon. Sub-classified by stats+name.

## Level Milestones

- Every 10 levels: new equipment tier
- Total: 125 entries across 6 slots
- Source: `equipment_progression` table (derived from `item_classification`)

## Evidence

| Claim | Evidence |
|---|---|
| Slot assignment | `BINARY_CONFIRMED` — binary type field from `iteminfo.edt` |
| Level range | `BINARY_CONFIRMED` — `items.level` from binary |
| Item name | `BINARY_CONFIRMED` — `items.name` from binary |
| Progression order | `DERIVED` — sorted by level within each slot |
