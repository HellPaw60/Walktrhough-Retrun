# Known Gaps

## Unresolved

| Area | Status | Reason |
|---|---|---|
| Quest chain | UNRESOLVED | No flag transition consumer found |
| Quest → NPC | UNRESOLVED | Equality only, no runtime evidence |
| Quest → Monster | UNRESOLVED | No exact reference in binary |
| NPC names | UNRESOLVED | Not in client binary (server-side?) |
| Skill data | UNRESILVED | Binary format not fully parsed |
| Actual drop source | UNRESOLVED | File drop*.scr not found in client |
| DropRate | DEFERRED | Not investigated per project scope |

## Known Limitations

- **NPC names:** Client only has NPC IDs; names likely server-side
- **Skill descriptions:** Binary SkillFile v7 format partially decoded
- **Quest chain:** quest_node_mapping uses talk_id equality (not consumer-tested)
- **Drop tables:** Wiki used as external reference, not client-confirmed

## External Dependencies

- Wiki API: https://wiki.sealreturn.com/api (external cross-reference only)
- Legacy DB: D:\SealR\_extract_db (historical reference)
