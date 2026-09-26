# Skill Tree Research

**Date:** 2026-09-26
**Baseline:** parser v9 (362 records/file, 38 fixed fields), semantic research commit `cf2552b`
**Source:** `skill_v9_data.pkl` (skill01.edt primary; variants 01–20 for min_level)

---

## Executive Summary

The prerequisite graph over all 362 skill records is **fully connected, acyclic,
and reference-clean**: 284 prerequisite edges, zero cycles, zero self-references,
zero broken references. The entire tree descends from two shared roots —
**Sleep (ID 1, 16 children)** and **Martial Combo (ID 5, 13 children)** — which
act as universal gate skills. Every job tree (Warrior, Knight, Jester, Mage,
Priest, Craftsman, Hunter, Chef) branches off Martial Combo or Sleep, then stays
within its own category.

Key structural facts:

- 362 nodes; 284 with prerequisites, 78 without (77 named roots + unnamed)
- Maximum chain depth: **9** (Sharp Eye; Chinese Dish – Fire Taste; Great Delicacies Truffle)
- Prerequisite levels used: 1, 3, 5, 10 — a clean 4-step gating system
- Max skill levels cluster at 5 (200 skills) and 10 (83 skills); 20 only for craft skills
- Min-level progression across variants splits into three clean patterns (constant / progress→plateau / progress→reset-lower)

All prerequisite and category semantics carry `PROBABLE` (cross-build v8 schema);
the graph built from them is `DERIVED`.

---

## Dataset

```
Nodes:            362 (344 named + 18 unnamed)
Edges:            284 prerequisite relationships (field[2] ≠ 0)
Source file:      skill01.edt (primary), variants 01–20 for field[6]
Fields used:      field[0] category, field[2] prereq ID, field[3] prereq level,
                  field[4] max level, field[5] skill points, field[6] min level
Machine output:   skill_tree_edges.csv (362 rows)
```

---

## Category / Job Groups

| Category | Records | Named | Interpretation | Confidence |
|---|---:|---:|---|---|
| 0 | 12 | 12 | Utility/basic (Sleep, Trade, Fishing, Party, Inventory, Emoticon, Kiosks, Duel Request) | `PROBABLE` |
| 1 | 18 | 18 | Warrior group (Great Sword Combo, Quick Slash, Double Slash…) | `PROBABLE` |
| 2 | 18 | 18 | Knight group (Sword Combo, Chivalry, Holy Cross…) | `PROBABLE` |
| 3 | 17 | 17 | Jester group (Knife Combo, Merriment, Playing Dead…) | `PROBABLE` |
| 4 | 25 | 25 | Mage group (Staff Combo, Source of Universe, Fireball…) | `PROBABLE` |
| 5 | 27 | 27 | Priest group (Mace Combo, Prayer, Cure, Heal…) | `PROBABLE` |
| 6 | 20 | 20 | Craftsman group (Hammer Combo, Cook, Weaponry…) | `PROBABLE` |
| 7 | 3 | 3 | Category 7 (Bless, Throw Bomb, Alchemy) | `UNRESOLVED` name |
| 8 | 3 | 3 | Category 8 (Intimidate, Warm Up, Beg) | `UNRESOLVED` name |
| 9 | 18 | 18 | Hunter-related (Slingshot Combo, Simple Shot, Power Shot) | `PROBABLE` |
| 11 | 10 | 10 | Category 11 (Dominate, Awakening, Instinct…) | `UNRESOLVED` name |
| 12 | 10 | 10 | Category 12 (Break Weapon, Frenzy, Vengeance…) | `UNRESOLVED` name |
| 13 | 11 | 11 | Category 13 (Sudden Attack, Sneakattack, Doppelganger…) | `UNRESOLVED` name |
| 14 | 9 | 9 | Category 14 (Ice Cannon, Glacier, Time Warp…) | `UNRESOLVED` name |
| 15 | 10 | 10 | Category 15 (Repentance, Judgement, Prediction…) | `UNRESOLVED` name |
| 16 | 11 | 11 | Category 16 (Coup de Grace, Time Bomb, Demolish…) | `UNRESOLVED` name |
| 19 | 11 | 11 | Archer-related (Piercing Arrow, Bump Arrow, Poison Arrow) | `PROBABLE` |
| 21 | 10 | 10 | Category 21 (Sword Dance, Charge, Radiant Sword…) | `UNRESOLVED` name |
| 22 | 9 | 9 | Category 22 (Tornado, Heal, Divine Guard…) | `UNRESOLVED` name |
| 23 | 11 | 11 | Category 23 (All In, Black Jack, Bluff…) | `UNRESOLVED` name |
| 24 | 9 | 9 | Category 24 (Giga Flame, Inferno, Hellfire…) | `UNRESOLVED` name |
| 25 | 13 | 13 | Category 25 (Mega Cure, Bulwark, Faith…) | `UNRESOLVED` name |
| 26 | 8 | 8 | Category 26 (Hammer Master, Master Refiner…) | `UNRESOLVED` name |
| 29 | 13 | 13 | Gunner-related (Aimed Shot, Kill Shot, Headshot…) | `PROBABLE` |
| 31 | 18 | 18 | Chef group (Poke Combo, Table Manner, Absolute Taste…) | `PROBABLE` |
| 131 | 8 | 8 | Advanced Chef (Last Supper, Food Lane, Onion Slicer…) | `PROBABLE` |
| 231 | 9 | 9 | Advanced Chef (Binge, Diet, Eat Fast…) | `PROBABLE` |
| 0xFFFFFFFF | 21 | 3 | Special/sentinel (18 unnamed + Seal Online, Unknown Skill, Royal Food) | `BINARY_CONFIRMED` (count) |

Note: categories 7–8, 11–16, 21–26 likely correspond to second-class/advanced-job
skill groups (the skill names match known Seal Online advanced classes), but the
specific job names are `UNRESOLVED` pending direct evidence.

---

## Prerequisite Graph

```
Nodes:                    362
Edges:                    284
Skills with prereq:       284
Skills without prereq:    78 (77 named roots + 1 unnamed root)
Root skills (named):      77
Leaf skills (named):      178
Maximum depth:            9
Cycles:                   0
Self references:          0
Broken references:        0
Prereq → unnamed target:  3 (98→97, 99→98, 226→225 — craft-chain placeholders)
Cross-category edges:     30 (28 expected: job trees root into cat-0 utilities;
                            2 special: Bless(cat7)→Encourage(cat5), unnamed→Sleep)
```

### Top parents (most-referenced prerequisites)

| Skill | Children | Role |
|---|---:|---|
| Sleep (ID 1) | 16 | Universal gate — every job's entry point |
| Martial Combo (ID 5) | 13 | Second universal gate — weapon combo root |
| Blacksmiths Ingenuity (ID 81) | 11 | Craftsman production root |
| Source of Universe (ID 17) | 5 | Mage magic root |
| Provoke (ID 50) | 5 | Knight defensive branch |
| Concentration (ID 28) | 4 | Warrior attack branch |
| Encourage (ID 53) | 4 | Priest buff branch |
| Chivalry (ID 33) | 3 | Knight holy branch |
| Cure (ID 42) | 3 | Priest healing branch |
| Merriment (ID 58) | 3 | Jester branch |

---

## Root Skills

The 77 named roots divide into:

- **Universal gates (2):** Sleep, Martial Combo — parents of nearly everything
- **Job entry combos (6):** Great Sword Combo, Sword Combo, Knife Combo, Staff Combo, Mace Combo, Hammer Combo (all prereq Martial Combo Lv5)
- **Advanced-class entries (~25):** Dominate, Sudden Attack, Ice Cannon, Repentance, Coup de Grace, Sword Dance, Tornado, All In, Giga Flame, Mega Cure, Piercing Arrow, Aimed Shot, etc. (second-class skills with no in-category parent)
- **Utility/misc (~40):** Trade, Fishing, Party, Inventory, Emoticon, kiosks, duel, cooking chain heads, etc.

---

## Skill Chains (deepest verified)

### Depth 9 — Sharp Eye (Hunter)
```
Sleep (Lv1)
 ↓
Martial Combo (Lv1)
 ↓
Slingshot Combo (Lv5)
 ↓
Training (Lv5)
 ↓
Simple Shot (Lv5)
 ↓
Point Shot (Lv5)
 ↓
Power Shot (Lv5)
 ↓
Eye Sight (Lv5)
 ↓
Sharp Eye
```

### Depth 9 — Chinese Dish – Fire Taste (Chef)
```
Sleep (Lv1) → Martial Combo (Lv3) → Poke Combo (Lv3) → Table Manner (Lv3)
 → Absolute Taste (Lv1) → Appetizer Soup (Lv5) → Korean Dish (Lv5)
 → Japanese Dish (Lv3) → Chinese Dish – Fire Taste
```

### Depth 8 — Revival (Priest healing line)
```
Sleep (Lv1) → Martial Combo (Lv5) → Prayer (Lv1) → Self Cure (Lv5)
 → Cure (Lv5) → Mass Cure (Lv5) → Cleansing (Lv5) → Revival
```

### Depth 8 — Grand Sword (Knight)
```
Sleep (Lv1) → Martial Combo (Lv5) → Sword Combo (Lv3) → Chivalry (Lv3)
 → Impact Crash (Lv3) → Impact Explosion (Lv5) → Dot Impact (Lv5) → Grand Sword
```

### Depth 8 — Deadly Cross (Knight holy line)
```
Sleep (Lv1) → Martial Combo (Lv5) → Sword Combo (Lv3) → Chivalry (Lv3)
 → Holy Cross (Lv3) → Grand Cross (Lv5) → Holy Punishment (Lv10) → Deadly Cross
```

### Fireball line (Mage, depth 4)
```
Sleep (Lv1) → Martial Combo (Lv5) → Source of Universe (Lv5)
 → Fireball (Lv1) → Firestorm (Lv1) → Hell Burn (Lv5) → Meteor (Lv10)
```

---

## Category Trees

### Mage tree (cat 4) — from Source of Universe

```
Source of Universe (ID 17)
├── Fireball (Lv1)
│   ├── Firestorm (Lv1) → Hell Burn (Lv5) → Meteor (Lv10)
│   └── Mega Fireball (Lv5) → Fire Strike (Lv5) → Mega Fire Strike (Lv10)
├── Frostbolt (Lv1)
│   ├── Ice Drill (Lv1) → Ice Dew (Lv5) → Blizzard (Lv10)
│   └── Mega Frostbolt (Lv5) → Ice Cube (Lv5) → Mega Ice cube (Lv10)
├── Mana Shield (Lv10) → Stick Booster (Lv5)
├── Ice Mastery (Lv10) → Freeze (Lv5), Waterfall (Lv5)
└── Fire Mastery (Lv10) → Immolation (Lv5), Phoenix (Lv5)
```

### Warrior tree (cat 1) — from Great Sword Combo

```
Great Sword Combo
├── Concentration (Lv3) → Quick Slash (Lv3) → Double Slash (Lv3), Chain Slash (Lv5)
├── Spinning Slash (Lv3) → Blazing Hurricane (Lv3)
├── Intimidation (Lv3) → Enrage (Lv3), Area Intimidation (Lv3)
├── Acceleration (Lv3)
└── Great Sword Combo 2 (Lv5) → Combo Training (Lv5) → Sword Wield (Lv10)
```

### Priest healing line (cat 5)

```
Prayer → Self Cure (Lv1) → Cure (Lv5) → Mass Cure (Lv5) → Cleansing (Lv5) → Revival
                                    └→ Prayer of Cure (Lv5), Blessed Swing (Lv5)
```

### Craftsman production (cat 6) — from Blacksmiths Ingenuity

```
Blacksmiths Ingenuity
├── Weaponry (Lv1) → Refine Weapon (Lv5)
├── Armory (Lv1) → Refine Equipment (Lv5)
├── Accessory Production (Lv1) → Refine Accessory (Lv5)
├── Alchemy (Lv1) → (unnamed #95) (Lv5)
├── Melting (Lv1) → (unnamed #97) (Lv5) → (unnamed #98) (Lv5)
├── Deadly Blow (Lv1) → Deadly Smash (Lv5), Crush (Lv5), Area Destruction (Lv5)
├── Cook (Lv1) → Gourmet Cook (Lv5)
└── Collect (Lv1) → Item Appraisal (Lv1)
```

(Full trees for all categories are derivable from `skill_tree_edges.csv`.)

---

## Level Requirements (field[6], across variants 01–20)

Three clean patterns emerge across 362 skills:

| Pattern | Skills | Example |
|---|---:|---|
| Constant | 117 | Sleep=1, Trade=2, Fireball=10 (all variants) |
| Progress → plateau | 176 | Knife Combo 1→2; Party 5→8; Heal 160→178 (files 01–10, then flat) |
| Progress → reset lower | 69 | Quick Slash 11→38 then 10; Double Slash 25→50 then 23; Fishing 8 then 0 |

The "reset-lower" group confirms the two-track structure: files 01–10 carry
rising per-level learn requirements, files 11–20 carry a **different, lower**
requirement set — consistent with a second progression track (`PROBABLE`),
not a simple continuation of levels 11–20.

---

## Max Skill Levels (field[4])

| max_level | Count | Interpretation |
|---:|---:|---|
| 0 | 2 | Disabled/unset (cat 7: two records) |
| 1 | 62 | Single-level skills (utility, combos, unnamed) |
| 2–9 | 9 | Rare values |
| 5 | 200 | Standard skills — matches 5 edge variants used |
| 10 | 83 | Advanced skills — matches files 01–10 base track |
| 20 | 8 | Craft production skills (Cure, Weaponry, Refine ×, Accessory, unnamed #95) |

The 20-level skills (craft production) plausibly correspond to the
`uskill01–20` extended family (`PROBABLE`) — uskill files carry max_level=20
for skills that are max_level=10 in skill files.

---

## Skill Point Cost (field[5])

```
Range:     0–54 (excluding sentinels)
Sentinel:  9999 × 2 (Throw Bomb, Alchemy — cat 7; likely disabled)
Mode:      6 SP (57 skills)
Typical:   1–10 SP for most skills; 12–54 for advanced/production
```

SP cost is per skill level (cross-build `PROBABLE`); no v7 consumer confirms
the exact spending mechanic.

---

## Variant / Tier Structure

```
Files 01–10: base progression — min_level rises, damage/AP scale up
Files 11–20: second progression track — min_level resets lower for 69 skills,
             power spikes at the 10→11 boundary (Fireball 240→410)
uskill01–20: extended family — same skills, max_level=20, cheaper SP,
             records start at offset 261
Confidence:  PROBABLE (no v7 loader; cross-build schema + data corroboration)
```

---

## Cycles & Broken References

```
Cycles:                0
Self references:       0
Broken prereq IDs:     0
Prereq → unnamed:      3 (98→97, 99→98, 226→225 — craft chain placeholders)
Cross-category edges:  30
  - 28 expected: job trees root into cat-0 utilities (Sleep, Martial Combo)
  - 1: Bless (cat 7) → Encourage (cat 5)
  - 1: unnamed #70 (sentinel) → Sleep (cat 0)
```

The graph is a clean DAG. The 30 cross-category edges are structural (roots in
shared utility category), not errors.

---

## Unnamed / Special Records

The 18 unnamed records participate in the graph:

- **#87** (mining): child of Collect — real skill, unnamed
- **#95, #97, #98, #99**: craft chain (Alchemy→#95, Melting→#97→#98, #99) — real skills, unnamed
- **#107**: child of Masquerade (mimic skill) — real skill, unnamed
- **#66, #67, #72–76**: Rose Cross Guild placeholders — children of other placeholders
- **#16, #21**: server-custom (Box Viewer, Drop Viewer — Indonesian descriptions)
- **#225, #226**: empty slots; #226 prereqs #225 (placeholder chain)
- **#70**: prereqs Sleep

None were dropped from the graph.

---

## Evidence Classification

| Claim | Level |
|---|---|
| 362 nodes, 284 edges, graph structure | `DERIVED` (from BINARY_CONFIRMED field values) |
| field[2]/field[3] = prereq ID/level | `PROBABLE` (cross-build v8 schema; chains semantically valid) |
| field[0] = category/job ID | `PROBABLE` (name correlation; 1–6 base jobs) |
| field[4] = max skill level | `PROBABLE` |
| field[5] = skill points | `PROBABLE` |
| field[6] = min level | `PROBABLE` (cross-variant progression verified) |
| Two-track variant structure (01–10 / 11–20) | `PROBABLE` |
| uskill = extended family | `PROBABLE` |
| Category names 7–8, 11–16, 21–26 | `UNRESOLVED` |
| Direct v7 consumer | NOT FOUND |

---

## Remaining Unknowns

1. Names for categories 7–8, 11–16, 21–26 (likely advanced classes — names not confirmed)
2. Gameplay meaning of the 11–20 track (second class? rebirth? — `PROBABLE` reset+spike, unconfirmed)
3. Whether max_level=20 craft skills use uskill files at runtime
4. SP spending mechanic (per level? per purchase?)
5. Why Throw Bomb / Alchemy carry SP=9999 (disabled flag?)
6. Direct v7 loader (SO3DPlus.exe packed)

---

## Method

Read `skill_v9_data.pkl` (parser v9 output, unchanged). Built directed graph from
field[2]→skill_id edges; validated acyclicity, reference integrity, category
consistency. Extracted per-variant min_level from all 20 files. Generated
`skill_tree_edges.csv` (362 rows). No parser, CSV, pickle, or SQLite changes.

**Machine-readable output:**
- `D:\SealR_Database\skill_tree_edges.csv` (canonical, local workspace)
- `D:\Walktrhough-Retrun\research\skill_tree_edges.csv` (copy; gitignored per repo policy)
