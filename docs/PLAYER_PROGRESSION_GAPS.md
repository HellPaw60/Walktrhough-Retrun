# Player Progression Gaps

## COMPLETE

| Area | Detail |
|---|---|
| Skill tree integration | Prerequisite graph + representative chains integrated into WALKTHROUGH.md |

## NOT YET BUILT

| Area | Reason |
|---|---|
| Player-facing skill recommendations | Requires gameplay evidence beyond dependency structure |

## UNRESOLVED

| Area | Evidence | Reason |
|---|---|---|
| Quest→Monster | `UNRESOLVED` | No exact reference in quest.edt binary |
| Quest chain | `UNRESOLVED` | Flag transition consumer not found |
| Map connections | `UNRESOLVED` | 1,247 edges need validation, no warp data |
| 1,635 NPC names | `UNRESOLVED` | Not in client binary string tables |

## KNOWN LIMITATIONS

- Quest→NPC: `PROBABLE` — 3,674 candidates evaluated; 1,235 valid rows after removing false positives (NPC 2/3 = monsters); 94 NPC IDs with textual evidence (names in quest dialog); 10 NPC IDs with multi-layer corroboration (text + dialog + location); no runtime consumer found.

- NPC placement: `CLIENT_FACT` — 1,300 entries across 98 maps
- NPC names: `PROBABLE` — 293 resolved, 1,635 unresolved
- Skills binary dataset: AVAILABLE — 7,240 records (362/file × 20 files; 344 named + 18 unnamed), fixed 38-field layout, chain-verified
- Skills semantic dataset: AVAILABLE with evidence levels — field[23]/field[27]/field[30]/field[31]/field[37] buff joins `BINARY_CONFIRMED`; prereq/element/projectile/variant mapping `PROBABLE` (cross-build v8 schema). See `SKILL_V7_SEMANTIC_RESEARCH.md`
- Drop tables: `BINARY_CONFIRMED` — client drop tables ditemukan (drop_1/2/3.edt, 11,982 rows total); monster→item mapping tersedia
- DropRate: rate values `BINARY_CONFIRMED` di drop files; roll semantics `PROBABLE`

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
