# The Weeping Lantern: generated adventure and playtest

A small level 1-2 haunted adventure produced by running the `/create-adventure` pipeline on macOS arm64, then playtested live and spot-fixed. Contains full spoilers.

| Path | What it is |
|---|---|
| `nwn-mcp-analysis.md` | Analysis of how the tool and pipeline worked, with diagrams and recommendations |
| `playtest-report.md` | Playtest findings, fixes applied, and open questions |
| `modules/the-weeping-lantern.mod` | Final module |
| `modules/the-weeping-lantern.pre-fixes.mod` | Before the first playtest fixes (level-up reminder, enemy move, hints) |
| `modules/the-weeping-lantern.pre-polish.mod` | Before the altar and lantern-item fixes |
| `screenshots/` | Evidence from the partial automated playtest |

The `.mod` files are binary (open with nasher or the toolset). Originals remain in `~/Documents/Neverwinter Nights/modules/` and `~/Documents/dev/nwn/playtest/`.

`adventure.md` and `*.log` are gitignored in this repo, so the adventure document and server/client logs are not included; they stay in `~/Documents/dev/nwn/`.
