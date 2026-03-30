# Content Layout

Use this as the default level and mission content shape when the repository does not already have a strong alternative:

```text
levels/
missions/
encounters/
resources/
  missions/
scenes/
  levels/
  missions/
Scripts/
  Missions/
  Game/
```

## Directory intent

- `levels/`: authored level data, map payloads, tile layouts, spawn and geometry definitions
- `missions/`: mission metadata, objective chains, trigger tables, encounter sequencing
- `encounters/`: reusable encounter payloads when the project supports encounter reuse
- `resources/missions/`: shared `.tres` or `.res` assets for mission runtime
- `scenes/levels/`: level scene composition
- `scenes/missions/`: mission-specific overlays, directors, or helper scenes
- `Scripts/Missions/`: mission controllers, mission state, sequencing logic
- `Scripts/Game/`: shared gameplay runtime unrelated to a single mission

## Mapping rule

Keep ids aligned:

- `level_01` data maps to `level_01.tscn` or equivalent
- `mission_01` data maps to a mission controller or mission resource with the same root id

Use one id family across data, scenes, and scripts whenever practical.
