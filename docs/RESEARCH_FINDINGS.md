# Research Findings

## Confirmed Facts

### Monster Database
- **Source:** `monster.edt` (2.48MB, 9,999 records × 31 × int64 × 248 bytes)
- **Field mapping:** field0=ID, field2=Level, field3=HP, field7=Critical, field8=Accuracy, field9=Evasion, field10=ATK, field11=DEF, field12=EXP, field13=field13_raw
- **field13_raw:** Probable drop-linked identifier (100% match Wiki dropid)

### Item Database
- **Source:** `iteminfo.edt` (85-column decoded structure)
- **Type field:** Binary-confirmed categories (0=Consumable, 1=Mixed, 4=Armor, 6=Shield, 7=Helmet, 9=Accessory, 11=Boots, 14=Cape, 16=Potion, 17=Food)
- **Type=1 sub-classification:** 10 equipment (Blessed Bells with stats), 103 consumables

### Quest Identity
- **Source:** `flag.edt` (QuestFlagFile v1, 717 records)
- **Mapping:** flag.quest_id == talk_id (717/717 equality)
- **NodeIndex != QuestID:** Verified (quest_node_mapping separates them)

### NPC Placements
- **Source:** `minimap/npc*.edt` (111 files)
- **Coverage:** 1,300 placements across 98 maps, 51 unique NPC IDs
- **Note:** NPC names not in client binary (likely server-side)

## Classification Results

### Monster Classification
| Category | Count |
|---|---|
| VALID (progression) | 5,161 |
| SPECIAL (tutorial/boss) | 3,073 |
| INVALID (placeholder) | 1,564 |
| SENTINEL (level≥10000) | 188 |
| ENDGAME (level>150) | 613 |

### Item Classification
| Category | Count |
|---|---|
| EQUIPMENT | 10 |
| ARMOR | 602 |
| HELMET | 517 |
| SHIELD | 799 |
| ACCESSORY | 562 |
| BOOTS | 623 |
| CAPE | 370 |
| POTION | 666 |
| FOOD | 655 |
| MATERIAL | 256 |

## Map Progression

- 40 maps with valid monster data
- Level ranges calculated from valid monsters only (excludes sentinels)
- Progression bands: BEGINNER (4 maps), EARLY (5), MID (5), LATE (8), ENDGAME (7), LEGEND (11)

## Equipment Progression

- 125 entries across 10-level bands per slot
- Zero classification conflicts (all entries verified as actual equipment)
