# evennia-mob-spawner-test-world

Tiny synthetic world content used by the `evennia-mob-spawner` demo gamedir as the place to spawn test mobs into. Built by [`evennia-world-builder`](https://github.com/FullCircleMUD/evennia-world-builder).

This repo is **fixture data**. Companion to [`evennia-mob-spawner-test-yaml`](https://github.com/FullCircleMUD/evennia-mob-spawner-test-yaml) (the spawn-rule fixtures). Together they exercise the cross-library seam: world-builder writes `mob_area:<key>` tags on rooms; mob-spawner queries them.

## Layout

```
definitions.yaml              # levels: [zone, room]
index.yaml                    # points at test_world (folder)
test_world/
  index.yaml                  # lists all 6 rooms
  hub.yaml                    # neutral starting room, no mob_area tag
  woods_clearing.yaml         # mob_area:woods_wolves
  deep_woods.yaml             # mob_area:woods_wolves
  meadow.yaml                 # mob_area:meadow_rabbits
  warren_entrance.yaml        # mob_area:kobold_warren
  warren_den.yaml             # mob_area:kobold_warren AND mob_area:kobold_warren_den
```

## Topology

```
              woods_clearing ←→ deep_woods
                    ↑
                    │ north
                    │
       hub ←─east→ meadow
        │
        │ south
        ↓
   warren_entrance ←→ warren_den
```

The hub is deliberately un-tagged so players can stand somewhere without triggering spawn checks.

## Tags chosen to match the spawn rules

Tags in this repo are placed under category `mob_area`. The keys match the `area_tag` and `den_room_tag` values used by spawn rules in `evennia-mob-spawner-test-yaml`:

| Tag key | Rooms | Used by which rule |
|---|---|---|
| `woods_wolves` | `woods_clearing`, `deep_woods` | Wolf, WolfFat |
| `meadow_rabbits` | `meadow` | Rabbit |
| `kobold_warren` | `warren_entrance`, `warren_den` | Kobold (`area_tag`), KoboldChieftain (`area_tag`) |
| `kobold_warren_den` | `warren_den` only | KoboldChieftain (`den_room_tag` — single-room pin) |

The two `mob_area` tags on `warren_den` are both under the same category (per the spawn library's architecture decision #16) — no second tag category is introduced.

## Using it

Against a local clone (`evennia-world-builder` installed in the demo gamedir's venv):

```bash
# From examples/demo_game/ with venv active
wb_build all
```

`WORLDBUILDER_READER_KWARGS` in the demo gamedir's `settings.py` points at this repo's clone via a sibling-directory path (`../../../evennia-mob-spawner-test-world`).

## License

BSD-3-Clause. See [LICENSE](LICENSE).
