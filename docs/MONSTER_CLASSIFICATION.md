# Monster Classification

## Summary
- Total monsters: 9,999
- Valid for progression: 5,161
- Sentinel (>=10000): 188
- Special (tutorial/boss): 3,073
- Invalid/placeholder: 0

## Classification Rules
| Rule | Classification | Count |
|---|---|---|
| Level >= 10000 | SENTINEL | 188 |
| Tutorial names (Joan, Arus, etc.) | SPECIAL | many |
| Level >= 200 | SPECIAL | - |
| Level > 150 | ENDGAME | - |
| High level + low HP/ATK | WEAK | - |
| Normal monsters | VALID | 5,161 |

## Confidence
- Classification: DERIVED (heuristic)
- Source: monsters + names + levels
