# Leveling Area Analysis

## Maps by Level Band

| Map ID | Min Level | Max Level | Avg Level | Monster Count |
|---|---|---|---|---|
| 1 (Land's End) | 1 | 30 | ~15 | Many |
| 32 (Elim) | 6 | 40 | ~23 | Many |

## Progression Engine
- Formula: level_min_candidate = MIN(monster_level) - 2
- Formula: level_max_candidate = MAX(monster_level) + 2
- Evidence: map_spawns + monsters
- Confidence: DERIVED
