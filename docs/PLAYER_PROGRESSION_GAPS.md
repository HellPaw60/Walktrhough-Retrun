# Player Progression Gaps

## UNRESOLVED

| Area | Evidence | Reason |
|---|---|---|
| Skill semantics | `UNRESOLVED` | Binary SkillFile v7 format not parsed; 0 rows in skills table |
| Quest→Monster | `UNRESOLVED` | No exact reference in quest.edt binary |
| Quest chain | `UNRESOLVED` | Flag transition consumer not found |
| Map connections | `UNRESOLVED` | 1,247 edges need validation, no warp data |
| 1,635 NPC names | `UNRESOLVED` | Not in client binary string tables |

## KNOWN LIMITATIONS

- Quest→NPC: `PROBABLE` — 3,674 candidates evaluated; 1,235 valid rows after removing false positives (NPC 2/3 = monsters); 94 NPC IDs with textual evidence (names in quest dialog); 10 NPC IDs with multi-layer corroboration (text + dialog + location); no runtime consumer found.

- NPC placement: `CLIENT_FACT` — 1,300 entries across 98 maps
- NPC names: `PROBABLE` — 293 resolved, 1,635 unresolved
- Skills: `UNRESOLVED` — 0 parsed (binary format not fully decoded)
- Drop tables: `UNRESOLVED`
- DropRate: `DEFERRED`

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
