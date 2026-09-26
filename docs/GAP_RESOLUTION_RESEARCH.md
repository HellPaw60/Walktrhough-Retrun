# Unresolved Gap Resolution Research

**Date:** 2026-09-26
**Baseline:** commit `d150ea2` (post skill-tree integration, docs normalized)
**Method:** evidence-first audit of every UNRESOLVED gap using raw client data

---

## Executive Summary

Dari 11 gap yang diaudit, **3 gap ter-resolve dengan BINARY_CONFIRMED evidence**, **2 gap naik ke CLIENT_FACT**, dan sisanya tetap UNRESOLVED/DEFERRED dengan negative-evidence trail yang konkret.

Terbesar: **drop source ditemukan** — `minimap/drop_1/2/3.edt` adalah drop table asli client (pipe-delimited, format persis `drop.py` schema), terverifikasi semantik via Piya→Piya's Unfertilized Egg dan 7/7 match dengan wiki. **field[30]/[31]/[37] ter-resolve** sebagai buff_3/buff_4/buff_5 IDs (join 100% ke buff.edt). **field[0] job mapping naik ke CLIENT_FACT** via `skilltree.men` UI yang memuat job_001–job_031/131/231 — persis nilai field[0].

---

## Evidence Matrix

| Area | Evidence Found | Evidence Type | Result | Confidence | Remaining Gap |
|---|---|---|---|---|---|
| Actual drop source | `minimap/drop_1.edt` (4,006 rows), `drop_2.edt` (3,991), `drop_3.edt` (3,985) — EDT-encoded pipe-delimited `(item_id, cumulative_rate)` pairs; monster field13 → row N; Piya (field13=1) → row 2 berisi item 1050 "Piya's Unfertilized Egg" + 6 item lain, 7/7 match wiki_loot_id=1 | **BINARY_CONFIRMED** (client file + semantic verify + cross-ref) | **RESOLVED** | `BINARY_CONFIRMED` | Rate encoding: cumulative per-million (cap 868750 = 86.875%); exact roll semantics untested runtime |
| field[30] | 3/3 nonzero values join buff.edt: Shatter Armor→218(급습), Time Bomb→218(급습), Blade Waltz→167(제압1) | **BINARY_CONFIRMED** (cross-table join, same proof as f23/f27) | **RESOLVED: buff_3_id** | `BINARY_CONFIRMED` | — |
| field[31] | 5/5 float-decoded values join buff.edt: 296(충격여파)×2, 951(이동불가), 1038(포츈쿠키-스턴), 1258(트리플애로우-독) — float encoding matches v8 schema note | **BINARY_CONFIRMED** | **RESOLVED: buff_4_id (float-encoded)** | `BINARY_CONFIRMED` | — |
| field[37] | 2/2 nonzero values join buff.edt: Throw Shield→350(쾌검), Disintegrate→150(토네이도1); kedua skill tidak punya f23/f27 — f37 satu-satunya buff link mereka | **BINARY_CONFIRMED** | **RESOLVED: buff_5_id (v7-specific 5th slot)** | `BINARY_CONFIRMED` | — |
| field[0] job mapping | `interface/skilltree.men` UI memuat `job_001`–`job_006`, `job_008`, `job_009`, `job_011`–`job_016`, `job_019`, `job_021`–`job_026`, `job_029`, `job_031`, `job_131`, `job_231` + `2ndjobLock` — **persis** nilai field[0]; struktur tab: job N = tab1, N+10 = tab2, N+20 = tab3 | **CLIENT_FACT** (UI resource names) | **UPGRADED: PROBABLE → CLIENT_FACT** untuk struktur job-tree; nama job tetap PROBABLE | `CLIENT_FACT` (structure) | Nama job spesifik (Warrior/Knight/...) tetap `PROBABLE` (name-correlation) |
| Exact 10→11 meaning | `2ndjobLock` di skilltree.men = sistem 2nd job ada di UI; tab structure (N, N+10, N+20) = 3 tab per job; TAPI tidak ada teks yang menyebut files 11–20 secara langsung | `CLIENT_FACT` (UI element exists) | **TETAP PROBABLE** — bukti struktural menguat (tab ke-2/ke-3 = advanced job), tapi link langsung file 11–20 ↔ tab 2 belum terbukti | `PROBABLE` (diperkuat) | Runtime mapping file→tab |
| DropRate | Ada di drop_1/2/3.edt sebagai rate per item (cumulative per-million) | **BINARY_CONFIRMED** (rate values ada) | **UPGRADED: DEFERRED → tersedia di client** — rate terbaca; namun validasi runtime (apakah benar-benar per-million roll) belum ada | `BINARY_CONFIRMED` (values) / `PROBABLE` (semantics) | Konfirmasi runtime |
| Quest chain | flag.edt (717 quest identity) tidak memuat next_quest field; quest_dialog_nodes tidak punya transition consumer; script.dat/scripts.dat (4.7MB) tidak ter-decode dengan EDT codec — format berbeda, belum dibongkar | Negative finding | **TETAP UNRESOLVED** | `UNRESOLVED` | script.dat/scripts.dat format unknown; konsumen chain tidak ditemukan |
| Quest → Monster | quest.edt tidak punya field monster_id eksplisit; action_id (10,845 unique) tetap tanpa definition table | Negative finding | **TETAP UNRESOLVED** | `UNRESOLVED` | action_id semantics tidak ditemukan di client |
| Map connection | map_info.edt = deskripsi teks map (47 map, nama + monster list) — TIDAK ada data portal/warp; 114 file .mdt tidak ter-decode dengan EDT codec (format berbeda); mappack.edt tidak ada di client | Negative finding | **TETAP UNRESOLVED** | `UNRESOLVED` | .mdt format unknown; tidak ada file warp/teleport ditemukan |
| Quest objective | action_id tidak punya consumer/enum/handler di client yang bisa ditemukan statis (exe packed) | Negative finding | **TETAP UNRESOLVED** | `UNRESOLVED` | SO3DPlus.exe packed |
| 1,635 NPC names | Semua client sources exhausted: npctalk.edt = dialog text (bukan nama); npc*.edt lain tidak ada; monster.edt tidak memuat nama; string table tidak ada. **Semua 1,635 NPC punya nama di wiki via ID match** (642 distinct names) | Negative (client) + positive (wiki) | **TETAP UNRESOLVED untuk client-confirmed**; wiki names tersedia sebagai `EXTERNAL_REFERENCE` untuk semua 1,635 | `UNRESOLVED` (client) / `EXTERNAL_REFERENCE` (wiki option) | Client tidak menyimpan nama NPC ini |
| Direct v7 loader | SO3DPlus.exe: 3 sections, `.text` rawsize=0 (virtual only), `.data` entropy 8.00, 347 strings semuanya garbage, no RTTI, no imports readable — **packed** (kemungkinan besar GameGuard-related packer). sealres.dll hanya memuat `skill.edt` (v4), bukan skill01–20. uskill01–20 tidak direferensikan binary manapun yang bisa dibaca | Negative finding (documented) | **TETAP UNRESOLVED** | `UNRESOLVED` | Exe packed; unpacking tidak dilakukan (di luar scope statis yang aman) |

---

## Detail Temuan Utama

### 1. Drop Source (RESOLVED — terbesar)

Tiga file di `minimap/` adalah drop table:

```
drop_1.edt: 4,006 rows — primary table (9,227 monster field13 di range ini)
drop_2.edt: 3,991 rows — 81 monster field13 di range 4007–7997 (jika concatenated)
drop_3.edt: 3,985 rows — 3 monster field13 di range 7998+ (jika concatenated)
```

Format per row (setelah EDT decode): pipe-delimited pairs `(item_id, cumulative_rate)`:

```
row 2 (Piya, field13=1):
3677|0|7586|0|1|534000|3|567750|4|592750|1028|652750|95|832750|1050|848750|1|858750|1020|868750|...
```

- Item 1050 = **Piya's Unfertilized Egg** (item_classification) — Piya drop egg-nya sendiri ✓
- 7/7 item match wiki_loot_id=1 (Geranium, Volcanic Rock, Forging Rock, Large Geranium, Egg, Piya's Unfertilized Egg, Pea Shell)
- Rates = cumulative thresholds, cap 868750/1000000 (86.875% total drop chance)
- Rascal Rabbit (field13=22) → row 22: item 23, 8, 10, 24, 15, 128 — konsisten rabbit drops
- Format sesuai `drop.py` schema dari unsealed project (row of (item_id, drop_rate) pairs, indexed by monster loot field)

**Pemakaian field13 → drop file mapping masih perlu kejelasan** (drop_1 alone vs concatenation), tapi data drop-nya sendiri terbaca dan terverifikasi semantik.

### 2. field[30]/[31]/[37] = buff_3/buff_4/buff_5 (RESOLVED)

Bukti identik dengan f23/f27 (103/103, 20/20): nilai nonzero join 100% ke buff.edt dengan nama semantik yang cocok:

- Shatter Armor: f23=275(방어구분쇄), f27=295(폭파술), **f30=218(급습)**, **f31=296(충격여파)** — 4 buff!
- Throw Shield: f23=0, f27=0, **f37=350(쾌검)** — sword-wind skill, buff 쾌검 (milik Windbreak) cocok dengan deskripsi "gust of sword wind"
- Disintegrate: **f37=150(토네이도1)** — satu-satunya buff link-nya

f31 float-encoding (int bits → whole float → buff id) persis seperti yang didokumentasikan v8 schema untuk stat 31.

### 3. field[0] = Job Tree ID (CLIENT_FACT via UI)

`skilltree.men` memuat elemen UI `job_001` sampai `job_231` — **nilai identik** dengan distribusi field[0]:

| Struktur | field[0] | UI | Interpretasi |
|---|---|---|---|
| Base job 1–6 | 1,2,3,4,5,6 | job_001–006 | 6 job dasar (tab 1) |
| Tab 2 job 1–6 | 11–16 | job_011–016 | skill lanjutan job 1–6 (tab 2) |
| Tab 3 job 1–6 | 21–26 | job_021–026 | skill lanjutan job 1–6 (tab 3) |
| Hunter family | 9, 19, 29 | job_009/019/029 | job 9 dengan 3 tab |
| Chef family | 31, 131, 231 | job_031/131/231 | job 31 dengan 3 tab |
| Job 8 | 8 | job_008 | single tab |
| **Category 7** | 7 | **TIDAK ADA di UI** | bukan job tree player — konsisten dengan anomali Bless/Throw Bomb/Alchemy (SP=9999, prereq_lv=0) |

`2ndjobLock` element mengonfirmasi sistem 2nd job ada di UI client.

**Implikasi:** "Category 7–8, 11–16, 21–26" yang selama ini UNRESOLVED namanya sekarang terstruktur: 11–16 dan 21–26 adalah **tab 2 dan tab 3 dari job 1–6** (bukan job terpisah). Nama job spesifik tetap PROBABLE (name-correlation), tapi struktur job-tree-nya CLIENT_FACT.

### 4. Negative Findings (didokumentasikan)

- **script.dat (2.6MB) + scripts.dat (2.1MB):** tidak ter-decode dengan EDT LCG codec — header berbeda, kemungkinan format script terkompilasi. Quest chain logic kemungkinan ada di sini tapi belum bisa dibaca.
- **114 file .mdt:** codec berbeda, bukan EDT. Map data (kemungkinan termasuk portal) tidak terbaca.
- **SO3DPlus.exe:** packed (`.text` virtual-only, entropy 8.0) — static analysis mentok. Unpacking tidak dicoba (di luar scope).
- **mappack.edt:** direferensikan sealres.dll tapi tidak ada di client extraction — file tidak tersedia.
- **NPC names:** client tidak menyimpan nama untuk 1,635 NPC (695 monster-range + 940 mid-range, 0 di range 4000+). Wiki punya nama untuk semua via ID match — tersedia sebagai EXTERNAL_REFERENCE.

---

## Files Baru yang Ditemukan

```
minimap/drop_1.edt  (894,451 bytes, 4,006 drop rows)  — PRIMARY drop table
minimap/drop_2.edt  (763,516 bytes, 3,991 drop rows)
minimap/drop_3.edt  (700,983 bytes, 3,985 drop rows)
interface/skilltree.men (139,524 bytes) — skill tree UI dengan job structure
```

---

## Status Changes Summary

| Area | Before | After |
|---|---|---|
| Actual drop source | `UNRESOLVED` | **`BINARY_CONFIRMED` — RESOLVED** (drop_1/2/3.edt) |
| DropRate | `DEFERRED` | **`BINARY_CONFIRMED` values tersedia** (rate di drop files); semantics `PROBABLE` |
| field[30] | `UNRESOLVED` | **`BINARY_CONFIRMED` — buff_3_id** (3/3 join) |
| field[31] | `UNRESOLVED` | **`BINARY_CONFIRMED` — buff_4_id float-encoded** (5/5 join) |
| field[37] | `UNRESOLVED` | **`BINARY_CONFIRMED` — buff_5_id** (2/2 join) |
| field[0] job mapping | `PROBABLE` | **`CLIENT_FACT`** (structure via skilltree.men); nama job tetap `PROBABLE` |
| Exact 10→11 meaning | `PROBABLE` | `PROBABLE` (diperkuat oleh tab structure + 2ndjobLock, tidak berubah) |
| Quest chain | `UNRESOLVED` | `UNRESOLVED` (script.dat belum terbaca — negative finding didokumentasikan) |
| Quest → Monster | `UNRESOLVED` | `UNRESOLVED` (action_id tanpa consumer) |
| Map connection | `UNRESOLVED` | `UNRESOLVED` (.mdt belum terbaca; map_info.edt = deskripsi saja) |
| Quest objective | `UNRESOLVED` | `UNRESOLVED` (exe packed) |
| 1,635 NPC names | `UNRESOLVED` | `UNRESOLVED` (client exhausted); wiki `EXTERNAL_REFERENCE` tersedia untuk semua |
| Direct v7 loader | `UNRESOLVED` | `UNRESOLVED` (exe packed — negative evidence trail lengkap) |

---

## Method

Semua temuan dari raw client data yang sudah terekstrak (`D:\SealR_Database\extracted\_extracted`). Tidak ada perubahan pada parser, pickle, CSV, SQLite, atau research docs yang ada. Drop table dan skilltree.men dibaca read-only dengan EDT codec yang sudah terbukti.
