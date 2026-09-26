# Skill v7 Parser Validation Report — v8

**Date:** 2026-09-26
**Parser Version:** 8 (final)

---

## 1. Parser Accuracy

### Per-File Counts (exact, verified)

| File | Header Max ID | Valid Records | Orphans | Missing |
|---|---|---|---|---|
| skill01.edt | 363 | 344 | 5 | 19 |
| skill02.edt | 363 | 344 | 5 | 19 |
| skill03.edt | 363 | 344 | 5 | 19 |
| skill04.edt | 363 | 344 | 5 | 19 |
| skill05.edt | 363 | 344 | 5 | 19 |
| skill06.edt | 363 | 344 | 5 | 19 |
| skill07.edt | 363 | 344 | 5 | 19 |
| skill08.edt | 363 | 344 | 5 | 19 |
| skill09.edt | 363 | 344 | 5 | 19 |
| skill10.edt | 363 | 344 | 5 | 19 |
| skill11.edt | 363 | 344 | 5 | 19 |
| skill12.edt | 363 | 344 | 5 | 19 |
| skill13.edt | 363 | 344 | 5 | 19 |
| skill14.edt | 363 | 344 | 5 | 19 |
| skill15.edt | 363 | 344 | 5 | 19 |
| skill16.edt | 363 | 344 | 5 | 19 |
| skill17.edt | 363 | 344 | 5 | 19 |
| skill18.edt | 363 | 344 | 5 | 19 |
| skill19.edt | 363 | 344 | 5 | 19 |
| skill20.edt | 363 | 344 | 5 | 19 |
| **Total** | — | **6,880** | **100** | — |

**Reconciliation: 344 valid + 19 missing = 363 = header. EXACT.**

---

## 2. Parser Bug History (why counts changed)

| Version | Records/file | Bug |
|---|---|---|
| v2/v3 (old) | 333 | Cascade bug: garbage records (`Rose Cross Guild Hurray!C–M`, `Seal Online G`) accepted → swallowed 11 real records (71, 77, 240–245, 264, 308, 309). Over-strict desc validation rejected 3 more (239, 263, 307) |
| v4 | 341 | Field-limit fix recovered 11 records, but lost 239, 263, 307 (UTF-8 desc, empty desc, short desc) |
| v5/v6 | 342–344 | Relaxed desc validation recovered some, but empty-desc fallback re-introduced cascade (garbage 44-field match swallowed records again) |
| **v8 (final)** | **344** | Inline empty-desc check (fields 36–39 + next-record validation) + field-count window 32–40 for desc matches. Clean field distribution, no contamination |

### Recovered records (14 total, all verified)

| ID | Name | Why previously missed |
|---|---|---|
| 71 | Prayer of Protection | Swallowed by garbage cascade |
| 77 | Cleansing | Swallowed by garbage cascade |
| 240 | Sharp Eye | Swallowed by garbage cascade |
| 241 | Double Shot | Swallowed by garbage cascade |
| 242 | Multi Shot | Swallowed by garbage cascade |
| 243 | Overdrive | Swallowed by garbage cascade |
| 244 | Running Fire | Swallowed by garbage cascade |
| 245 | Fake | Swallowed by garbage cascade |
| 264 | Ironblood | Swallowed by garbage cascade |
| 308 | Warrior's Inspiration | Swallowed by garbage cascade |
| 309 | Knight's Command | Swallowed by garbage cascade |
| 239 | Eye Sight | UTF-8 bytes in description |
| 263 | Unknown Skill | desc_len = 0 (empty) |
| 307 | Royal Food | desc_len = 10 (short) |

---

## 3. Missing IDs (19 per file, consistent)

```
16, 21, 66, 67, 70, 72, 73, 74, 75, 76, 87, 95, 97, 98, 99, 107, 225, 226, 363
```

Not parser failure — these IDs have no record data in any file (binary-verified).

---

## 4. Record Layout Verification

```
uint32  skill_id         ✓ (1–362, all valid)
char[]  name             ✓ (printable ASCII, uppercase first, 3–50 chars)
uint8[] null padding     ✓ (variable)
uint32  fields[34–38]    ✓ (count distribution: 32×1, 34×2, 36×1, 37×9, 38×331)
uint32  desc_len         ✓ (0–499)
char[]  description      ✓ (ASCII/UTF-8, length = desc_len)
uint8[] null padding     ✓ (variable)
```

Note: there is **no** skill_id copy after desc_len — the old claim was an artifact
of contaminated parsing. field[34] is an internal index (matches skill_id only
53/331 times); field[36] is constant 0.

---

## 5. Orphan Records (100 total, 5/file)

All are `Rose Cross Guild Hurray!C/I/J/K/L/M` ID-24 entries embedded in
description-block regions — placeholder/legacy data, not real skills:
- skill01: @22365, @24033, @24249, @24465, @24681

Logged and excluded from valid counts.

---

## 6. Skill ID Analysis

| Metric | Value |
|---|---|
| Unique skill IDs | 344 |
| Unique skill names | 337 |
| ID range | 1–362 |
| Skills in all 20 files | 344 (100%) |
| Skills in only 1 file | 0 |

### Duplicate names (6 groups)

| Name | IDs |
|---|---|
| Accurate Appraisal | 352, 354 |
| Combo Master | 163, 189, 203 |
| Concentration | 28, 332 |
| Enhance Bow | 262, 319 |
| High Quality Sense | 323, 324 |
| Reload | 254, 322 |

---

## 7. Output

- CSV: `D:\SealR_Database\skill_v7_parsed.csv` (6,880 rows, 44 columns, 1.72 MB)
- Pickle: `D:\SealR_Database\skill_v7_data.pkl`

---

## 8. Validation

| Check | Result |
|---|---|
| All 20 files parsed | PASS |
| 344 + 19 = 363 exact reconciliation | PASS |
| Consistent counts across all files | PASS |
| All IDs in all 20 files | PASS |
| No field-count contamination (1–17) | PASS |
| Orphans logged separately | PASS |
| CSV regenerated from v8 data | PASS |
