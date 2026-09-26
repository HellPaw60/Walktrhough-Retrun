# Seal Online SkillFile v7: Cross-File Variant Analysis — Final v3

**Date:** 2026-09-26
**Data source:** parser v9 (fixed-layout chain, 362 records/file)

---

## Executive Summary

**Conclusion: Hypothesis A is supported — `PROBABLE`** — `skill01.edt` through
`skill20.edt` represent **skill level/rank variants 1–20** of the same skill
dataset. Each file contains the same 362 records (344 named skills + 18 unnamed
placeholders); names are identical across files and descriptions are identical
**except Reload (ID 322)**, whose percentage changes `4% → 8% → 12% → 16% → 20%`
(plateau at 20% from file 05 onward); numeric fields progress
systematically with file index (power values rise through file 10, then plateau
at a mastery tier). No loader/consumer was found, so this remains `PROBABLE`.

**Corrections vs. previous versions of this report:**
1. "362 records" was previously claimed via a garbage-counting structural scan —
   the number is right but the method was wrong. It is now proven by fixed-layout
   chain parse (sequential IDs 1–362, exact EOF coverage in all 20 files).
2. "344 records / 19 missing" (v8 report) was an artifact — the "missing" IDs
   (16, 21, 66–76, 87, 95–99, 107, 225, 226) are real records with **empty name
   fields**, invisible to name-requirement parsers.
3. Field indexing is now stable: all records have exactly 38 fields. The old
   "field 5 = SP / field 8 = min level / field 17 = damage" mapping was shifted
   by misparses. Scaling values live in **field[18]** (power) and **field[9]**
   (requirement-scale).

---

## 1. Dataset (final)

| Metric | Value |
|---|---|
| Files | 20 |
| Records per file | 362 (sequential IDs 1–362) |
| Named skills | 344 |
| Unnamed records | 18 (empty name field, field[0]=0xFFFFFFFF) |
| Total records | 7,240 |
| Absent IDs | 1 (ID 363 = header capacity value) |

---

## 2. Text Comparison

| Aspect | Finding |
|---|---|
| Names | Identical across all 20 files (fixed 32-byte field) |
| Descriptions | Identical across files **except Reload (ID 322)** — the only textual variant in the dataset: `4% → 8% → 12% → 16% → 20%` chance to reset headshot cooldown, plateauing at 20% from file 05 onward. All other 361 records have byte-identical descriptions in all 20 files |
| Variant info | Lives in numeric fields; Reload is the sole exception where it also appears in text |

---

## 3. Field Patterns Across 20 Files (362 records)

| Field | Pattern | Varying records | Notes |
|---|---|---|---|
| field[0] | Constant per record | 0/362 | Category/class-group ID |
| field[4] | Constant per record | 0/362 | Never changes across files |
| field[9] | Plateau | 217/362 | Rises 01→10, constant 11–20 |
| field[18] | Plateau | 255/362 | Rises 01→10, constant 11–20 (power candidate) |
| field[15] | float32 | — | Multiplier A (0.0 / 0.5 / 1.0 / 0.7 / 1800.0 …) |
| field[16] | float32 | — | Multiplier B (0.0 / 3.0 / 1.0 / 6.0 / 5.0 …) |
| field[34] | Constant per record | — | Internal index (drifts from skill_id by -4/-5/-7 steps) |
| field[36] | Constant 0 | — | 362/362 |

### Statistical summary (362 records)

| Metric | Count |
|---|---|
| Records with any field change across files | 330 |
| Constant named records | 21 (e.g. Sleep ID 1) |
| Constant unnamed records | 11 |

---

## 4. Anchor Skill Progression (v9 data)

### Fireball (ID 18, Mage)

| File | f9 | f18 (power) |
|---|---|---|
| skill01 | 15 | 75 |
| skill05 | 19 | 135 |
| skill10 | 24 | 240 |
| skill11 | 24 | 410 |
| skill15 | 24 | 410 |
| skill20 | 24 | 410 |

### Double Slash (ID 31, Warrior)

f18: 600 → 744 → 1625 (file 10) → 770 (files 11–20 plateau)

### Heal (ID 155, Priest)

f9: 210 → 260 → 330 (plateau); f18: 2500 → 15000 → 0

### Sleep (ID 1)

Identical across all 20 files — utility skill, one of 21 constant named records.

---

## 5. Hypothesis Evaluation

| Hypothesis | Verdict | Evidence |
|---|---|---|
| A: skill01–20 = skill levels/ranks | **`PROBABLE`** | 330/362 records change; power rises then plateaus; text identical; 20-step progression |
| B: Server config tiers | Weak | Smooth per-skill progression fits level scaling |
| C: Client/platform variants | Rejected | No platform differences |
| D: Language variants | Rejected | No localization/language variant was observed. Reload (ID 322) changes percentage text across variants, but this is consistent with progression data rather than localization |
| E: Unrelated duplicates | Rejected | Systematic progression |

---

## 6. Variant Conclusion

**skill01–skill20 = skill level/rank variants — `PROBABLE`.**

- Files 01–10: base progression (power/requirement fields increase)
- Files 11–20: mastery tier (plateau; power spike at the 10→11 transition, e.g. Fireball 240→410)

Confidence remains `PROBABLE` (not `BINARY_CONFIRMED`) because no runtime
loader/consumer evidence exists.

---

## 7. Remaining Unknowns

1. No v7 loader/consumer found in client executable
2. Field semantics beyond structural roles remain `UNRESOLVED`
3. Prerequisite/skill-tree references not identified
4. Mastery-tier transition (10→11) not confirmed by runtime evidence
5. field[0] sub-class group names (7–31, 131, 231) unknown

---

## 8. Data Model (proposed, not yet applied)

```
skill
------
skill_id (PK, 1–362)
name (may be empty — 18 records)
description
category_id (field[0])

skill_variant
-------------
skill_id (FK)
variant_index (1–20 = file number)
field_0 … field_37 (38 × uint32 raw)
```

Canonical SQLite untouched pending decision.
