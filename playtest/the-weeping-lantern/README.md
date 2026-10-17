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

## Status and follow-ups

The full quest chain was played to "ADVENTURE COMPLETE" on 2026-10-17. Remaining work (replay after the altar and lantern fixes, Pip, Corwin's store, vault gate wording, the 200 vs 150 GP question, a WASD-camera rerun) is tracked in https://github.com/norrisaftcc/game-nwn-menhir/issues/61.

**Binaries policy:** the three `.mod` files here are a one-off example of generated output. Do not add more generated `.mod` files to this repo; store module source instead (Nasher, or exploded GFF-JSON from `nwn_erf` + `nwn_gff`) and keep built modules out of git.
