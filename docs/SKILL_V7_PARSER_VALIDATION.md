# Skill v7 Parser Validation Report — v9

**Date:** 2026-09-26
**Parser Version:** 9 (deterministic fixed-layout chain)

---

## 1. Final Accuracy

### Per-File Counts (chain-verified)

| File | Header Max ID | Chain Records | Named | Unnamed | EOF Coverage |
|---|---|---|---|---|---|
| skill01.edt | 363 | 362 | 344 | 18 | 109,294/109,294 exact |
| skill02.edt | 363 | 362 | 344 | 18 | 109,294/109,294 exact |
| skill03–20.edt | 363 | 362 | 344 | 18 | 109,295/109,295 exact |
| **Total** | — | **7,240** | **6,880** | **360** | **20/20 files exact** |

All files: sequential IDs 1–362, zero gaps, zero non-sequential transitions.

---

## 2. The Fixed Layout (BINARY_CONFIRMED)

```
offset  size  field
+0      4     skill_id (uint32)
+4      32    name (fixed 32-byte field, null-padded)
+36     152   fields (38 × uint32)
+188    4     desc_len (uint32)
+192    var   description (desc_len bytes)
→ next record at +192+desc_len (no trailing padding)
```

**Proof:** chain parse from offset 260 walks 362 records per file and lands on
exactly the final byte of each of the 20 files. A single misaligned record would
desynchronize the chain and produce garbage at the next record boundary — this
never happened, in any file.

---

## 3. Parser Version History (full audit trail)

| Version | Records/file | Root cause of error |
|---|---|---|
| v2/v3 | 333 | Cascade bug: garbage in-description text (`Rose Cross Guild Hurray!C–M`, `Seal Online G`) matched record heuristics → swallowed 11 real records (71, 77, 240–245, 264, 308, 309); strict desc validation rejected 239/263/307 |
| v4 | 341 | Recovered the 11, lost 239 (UTF-8 desc), 263 (empty desc), 307 (short desc) |
| v5/v6 | 342–344 | Relaxed validation re-introduced cascade via 44-field garbage matches |
| v8 | 344 | Field-count window heuristic; 13 records misparsed at wrong desc boundaries (e.g. ID 356: field[32]=1 + next byte `d` accepted as desc_len=1/desc='d'). "331/344 38-field layouts" and "19 missing IDs" were artifacts of this scan |
| **v9** | **362** | **Fixed layout — no scanning, no heuristics. Chain-verified.** |

### Why every earlier parser undercounted

All prior versions required a **non-empty, printable name** to even attempt a
record match. The 18 unnamed records (empty 32-byte name field) were invisible
to them — and several cascade/mismatch bugs traced back to the scanner jumping
*through* those empty-name regions. The fixed layout has no such blind spot.

---

## 4. The 18 Unnamed Records (recovered by v9)

| ID | desc_len | Description | Note |
|---|---|---|---|
| 16 | 81 | Box Viewer (Indonesian) | Server-custom |
| 21 | 62 | Drop Viewer (Indonesian) | Server-custom |
| 66 | 24 | Rose Cross Guild Hurray! | Placeholder |
| 67 | 24 | Rose Cross Guild Hurray! | Placeholder |
| 70 | 13 | Seal Online | Placeholder |
| 72 | 24 | Rose Cross Guild Hurray! | Placeholder |
| 73 | 24 | Rose Cross Guild Hurray! | Placeholder |
| 74 | 24 | Rose Cross Guild Hurray! | Placeholder |
| 75 | 24 | Rose Cross Guild Hurray! | Placeholder |
| 76 | 24 | Rose Cross Guild Hurray! | Placeholder |
| 87 | 164 | mining skill | Unnamed real skill |
| 95 | 107 | guardian production | Unnamed real skill |
| 97 | 116 | disintegrate items | Unnamed real skill |
| 98 | 163 | atom registration | Unnamed real skill |
| 99 | 149 | spirit extraction | Unnamed real skill |
| 107 | 199 | monster mimic | Unnamed real skill |
| 225 | 0 | (empty) | Deleted slot |
| 226 | 0 | (empty) | Deleted slot |

All have field[0] = 0xFFFFFFFF. Field blocks are structurally valid 38×uint32.

---

## 5. Field Statistics (denominator = 362, all records)

| Statistic | Result |
|---|---|
| field[34] == skill_id | 65/362 (offset drift -4/-5/-7 ⇒ internal index, not ID copy) |
| field[36] == 0 | 362/362 |
| field[37] == 0 | 360/362 (exceptions: 350, 150) |
| field[17] ∈ {0,1} | 360/362 (exceptions: 10, 2) |
| All records 38 fields | 362/362 |

Denominator note: earlier reports used 331 (v8's accidental 38-field subset).
Correct denominator is **362** — every record has every field.

---

## 6. Output

- CSV: `D:\SealR_Database\skill_v7_parsed.csv` (7,240 rows, 44 columns, 1,796,615 bytes)
- Pickle: `D:\SealR_Database\skill_v9_data.pkl`

Description-content verification (v9 data): descriptions are byte-identical
across all 20 files for 361/362 records. The sole exception is **Reload
(ID 322)** — 5 unique descriptions across files (4% → 8% → 12% → 16% → 20%
headshot-cooldown reset chance, plateau at 20% from file 05).

---

## 7. Validation

| Check | Result |
|---|---|
| Chain parse exact EOF, 20/20 files | PASS |
| Sequential IDs 1–362, zero gaps | PASS |
| 344 named + 18 unnamed = 362 | PASS |
| Fixed 38-field layout for all records | PASS |
| CSV regenerated from v9 chain data | PASS |
| No raw client artifacts published | PASS |
