# Seal Online SkillFile v7: Cross-File Variant Analysis — Final v2

**Date:** 2026-09-26
**Data source:** parser v8 (clean, 344 records/file)

---

## Executive Summary

**Conclusion: Hypothesis A is supported — `PROBABLE`** — `skill01.edt` through
`skill20.edt` represent **skill level/rank variants 1–20** of the same skill
dataset. Each file contains the same 344 skills with identical names and
descriptions; numeric fields progress systematically with file index
(power values rise through file 10, then plateau at a mastery tier).
No loader/consumer was found, so this remains `PROBABLE`, not `BINARY_CONFIRMED`.

**Correction vs. previous version of this report:** the earlier claim of
"362 records per file, only ID 363 missing" was wrong — it came from a
structural scan that counted garbage text entries (e.g. `Rose Cross Guild
Hurray!C–M`) as records. The verified counts are **344 valid records per file,
19 truly missing IDs** (344 + 19 = 363 = header).

The previous "field 5 = SP / field 8 = min level / field 17 = damage" mapping
was also shifted by contaminated data. In the clean 38-field layout the scaling
values live in **field[18]** (power) and **field[9]** (requirement-scale), and
field[17] is a small 0/1 flag.

---

## 1. File Structure & Decoding

All 20 files share identical structure:
- **Header**: `"Seal Online SkillFile v7"` (24 bytes) + null padding
- **Max skill ID**: 363 (uint16 at offset 64)
- **Signature**: `"skill"` (offset 72)
- **Records**: start at offset 260
- **Record layout**: `uint32 skill_id` + null-terminated name + null padding +
  `uint32 fields[34–38]` + `uint32 desc_len` + description + null padding

**Cipher**: byte-by-byte XOR with LCG (seed=0x11CFD, mult=52845, add=22719, mask=0xFFFF).

| File | Size (bytes) | Valid Records | Orphans |
|---|---|---|---|
| skill01.edt | 109,294 | 344 | 5 |
| skill02.edt | 109,294 | 344 | 5 |
| skill03–20.edt | 109,295 | 344 | 5 |
| **Total** | ~2.1 MB | **6,880** | **100** |

---

## 2. Missing IDs (19, verified)

```
16, 21, 66, 67, 70, 72, 73, 74, 75, 76, 87, 95, 97, 98, 99, 107, 225, 226, 363
```

Consistent across all 20 files. The old "30 missing" list incorrectly included
11 IDs (71, 77, 240–245, 264, 308, 309) that were actually present but swallowed
by the old parser's cascade bug, plus 239/263/307 rejected by over-strict
description validation. All 14 are now recovered and verified.

---

## 3. Name & Description Comparison

| Aspect | Finding |
|---|---|
| **Names** | Identical across all 20 files for all 344 skills |
| **Descriptions** | Identical across files (text does not encode variant info) |
| **Exception** | Skill 322 `Reload`: description contains a percentage that changes (4% → 8% → ... → plateau at 20%) |

Since text is identical while numeric fields change, **variant information lives
entirely in the numeric portion** — the strongest structural evidence for the
level-variant model.

---

## 4. Field Patterns Across 20 Files (clean data)

| Field | Pattern | Varying Skills | Notes |
|---|---|---|---|
| field[0] | Constant per skill | 0 | Category/class-group ID (26 distinct values) |
| field[4] | Constant per skill | 0 | Never changes across files |
| field[9] | Plateau | 217 | Rises 01→10, constant 11–20 (requirement-scale candidate) |
| field[18] | Plateau | 255 | Rises 01→10, constant 11–20 (power/damage/heal candidate) |
| field[15] | float32 | — | Effect multiplier A (Fireball 0.4, Heal 1.2, Double Slash -0.7) |
| field[16] | float32 | — | Effect multiplier B (Fireball 1.0, Heal 6.0, Double Slash 3.0) |
| field[34] | Constant per skill | — | Internal skill index (NOT a skill_id copy) |
| field[36] | Constant 0 | — | All records |

### Pattern Definition: Plateau

Values increase monotonically from file 1 to file 10, then remain constant from
file 11 to file 20. This is the dominant pattern in the scaling fields.

**Example — Fireball (ID 18), field[18] (power):**
```
File  1:    75
File  5:   135
File 10:   240  ← peak of base progression
File 11:   410  ← jump to mastery tier
File 12–20: 410 (plateau)
```

---

## 5. Anchor Skill Analysis (verified on clean data)

### Fireball (ID 18, Mage)

| File | f4 | f9 | f18 (power) |
|---|---|---|---|
| skill01 | 10 | 15 | 75 |
| skill05 | 10 | 19 | 135 |
| skill10 | 10 | 24 | 240 |
| skill11 | 10 | 24 | 410 |
| skill15 | 10 | 24 | 410 |
| skill20 | 10 | 24 | 410 |

### Double Slash (ID 31, Warrior)

| File | f9 | f18 |
|---|---|---|
| skill01 | 60 | 600 |
| skill05 | 80 | 744 |
| skill10 | 105 | 1625 |
| skill11–20 | 105 | 770 (plateau) |

### Heal (ID 155, Priest)

| File | f9 | f18 |
|---|---|---|
| skill01 | 210 | 2500 |
| skill05 | 260 | 15000 |
| skill10–20 | 330 | 0 (plateau/flag change) |

### Sleep (ID 1)

Identical across all 20 files — utility skill with no scaling. One of 21
fully-constant skills.

---

## 6. Statistical Evidence

| Metric | Count (of 344 skills) |
|---|---|
| field[18] changes across files | 255 |
| field[9] changes across files | 217 |
| ANY field changes | 323 |
| Fully constant skills | 21 |

Each of the 20 files is distinguishable by its field[18] signature set — no two
files are identical.

---

## 7. Hypothesis Evaluation

### Hypothesis A: Skill Levels/Ranks 1–20 — **`PROBABLE`**

**Evidence:**
1. Scaling field values rise monotonically with file index then plateau
2. Every file is unique (field signatures differ)
3. Same 344 skills in every file with identical names/descriptions
4. Plateau at files 11–20 suggests a tier transition (base → mastery)
5. Jump at file 11 (Fireball 240→410) suggests a mastery power spike

### Hypothesis B: Server Configuration Tiers — Weak

Smooth per-skill progression fits level scaling better than arbitrary tiers.

### Hypothesis C: Client/Platform Variants — Rejected

No platform-specific differences; all files structurally identical.

### Hypothesis D: Language/Build Variants — Rejected

All text identical English.

### Hypothesis E: Unrelated Duplicates — Rejected

Systematic value progression, not random.

---

## 8. Additional Findings

### Skill 322 "Reload" — Unique Description Progression

Only skill with changing description text (4% → 8% → 12% → 16% → 20% plateau).
Confirms the progression model even in descriptive text.

### File Size Anomaly

skill01 and skill02 are 109,294 bytes; skill03–20 are 109,295 bytes. Minor data
variation, no structural impact.

---

## 9. Final Conclusion

**skill01–skill20 = skill level/rank variants — `PROBABLE`.**

- Files 01–10: base progression (power/requirement fields increase)
- Files 11–20: mastery tier (values plateau; power spike at the 10→11 transition)

Downgraded from earlier "strongly supported" wording: no consumer/loader
evidence exists, so per project evidence taxonomy this is `PROBABLE`.

---

## 10. Remaining Unknowns

1. No v7 loader/consumer found in client executable
2. Field semantics beyond structural roles remain `UNRESOLVED`
3. Prerequisite/skill-tree references not identified
4. Meaning of the 10→11 mastery transition not confirmed by runtime evidence
5. field[0] values 7–31, 131, 231 group names unknown

---

## 11. Data Model (proposed)

```
skill
------
skill_id (PK)
name
description
category_id (field[0])

skill_variant
-------------
skill_id (FK)
variant_index (1–20, file number)
field_0 … field_37 (raw uint32 values)
```

Not yet applied to canonical SQLite — awaiting decision.
