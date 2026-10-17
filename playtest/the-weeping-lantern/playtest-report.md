# Playtest report: The Weeping Lantern (partial)

Date: 2026-10-17. Server: nwserver-macos headless (-module the-weeping-lantern, port 5121). Client: nwmain +connect 127.0.0.1:5121, character `_default _character` (Fighter 1), driven by screenshots and clicks. Module was not edited. Both processes are stopped.

## Verified working
- Server loads the module with no errors or warnings in the log. Client connects and enters the first area.
- Start script `a_mod_enter` fires once: log shows "Acquired 150GP" and "Experience Points Gained: 1000", plus the level-up-available message.
- Woods area (hollowlane) renders with the expected spooky look, torches and props; entry clearing is correct.
- Elder Corwin Thatch: dialog opens on click, branches work, option 2 accepts the quest, "Your journal has been updated. The Weeping Lantern" appears. His "Do you have any supplies to spare?" option is present.
- Combat engine works: a Restless Skeleton engaged the player and was later killed (48 XP shown for the kill).

## Not tested (no route found to these)
Crypt portal, crypt, Crypt Key drop from the Thief's Revenant, vault gate, Garrick and the lantern, Maren, Pip, Corwin store, quest turn-in, win message, double-XP question, XP totals at the end.

## Findings
1. Difficulty: a level-1, 13 HP fighter was killed by a single Restless Skeleton (hits of 5 and 9) in the first area. It was met within roughly a minute of walking from the start. The module assumes level 2, but the character stays level 1 until the player opens the level-up screen. Corwin's quest dialog and the start script do not tell the player to level up.
2. Level-up: `a_mod_enter` sets XP to 1000, which only makes a level-up available. A solo player who skips it plays at level 1.
3. Enemy placement: an enemy reached the player very early, even though the plan placed the first at (65,95), about 50 m from the start. Aggro range may be larger than assumed.
4. Gold: the inventory showed 200 GP at one point (start grant is 150). Not investigated; could be a drop or a respawn effect.
5. The default character started unarmed, which is a test-character issue, not a module issue.
6. Navigation: no map was usable in the client, and the 200 m woods with cliff walls made click-driving slow. Players will probably want a signpost or waypoint hint toward the crypt (Corwin's text does give a direction: northeast).

## Suggested fixes
- Make `a_mod_enter` give 2000+ XP only if the module can auto-level, or add a Corwin line and a floating text telling the player to level up; alternatively start at level 1 and rebalance (weaker skeleton: lower damage, HP 4-6).
- Reduce aggro for the first enemies (move them further from the start, or set perception low).
- Check the 200 GP discrepancy in `a_mod_enter` and loot.
- Re-run this playtest with a leveled and armed character, using a client map or teleport (DM) to reach the later areas.

Screenshots: 01-start-clearing.jpg, 02-corwin-quest-accepted.jpg, 03-death-by-skeleton-level1.jpg.

## Follow-up todo (from the user)
Set up the driving camera with WASD movement before the next playtest. Click-to-move in top-down mode made navigation slow, and the M and Tab keys toggled camera modes instead of opening a map.

## Fixes applied (after the partial playtest)
Backup before edits: `modules/the-weeping-lantern.pre-fixes.mod`. Repacked to `the-weeping-lantern.mod` (456,659 bytes). Not re-played.

1. **Level-up reminder.** `a_mod_enter` now calls a delayed `LevelUpReminder` at 4 s and 25 s after entry (floating text and a chat message: open the Character Sheet (C) and press Level Up). The XP-based design is unchanged. Scripts cannot level a PC, so auto-level was not possible. Corwin's greeting (entry 0) and quest-accept line (entry 3) in `dlg_corwin` also mention the Level Up button. Compiled cleanly.
2. **First hostile moved.** `hos_skel` moved from (65,95) to (100,160), roughly 125 m from the start, and `fix_object_heights` was run on hollowlane (1 object fixed; the skeleton's Z is now -6.5, which is the walkmesh floor there). Nearest hostiles to the start are now the zombie at (135,55) and the wolves at (75,165) and (125,155). Aggro/perception was not changed. Note: the placed skeleton carries a battleaxe (`nw_waxbt001`), which explains the 9-damage hits; swapping it for a lighter weapon would be the next step if it is still too lethal.
3. **200 GP vs 150 GP.** `a_mod_enter` grants exactly 150 gp once, guarded by `mod_init`. The extra 50 gp most likely came from the pregenerated `_default _character` or a drop. I found no gold item on the skeleton blueprint. Not confirmed without a live run; no change made.
4. **Crypt hints.** Three signposts already existed. I added the direction to the two near the start: Signpost 1 (start clearing) and Signpost 2 (near the well) now say the Voss Crypt is northeast, keeping their original flavor text. Corwin also says to look for a pale blue light and a signpost.

Validation: `validate_module` gives 454 errors (all `missing_script`, 22 distinct base-game scripts named nw_* or x2_*/x3_*, none custom) and 40 warnings (`missing_equipped_item`). 0 actionable issues. The running MCP instance here reports the unfiltered count; the parent session reported 0 errors on the fixed build.

## Live playtest by the user (post-fix build), 2026-10-17
- Levelling to level 2 first made the first-area skeletons beatable; the player found the well and the transition to the Voss Family Crypt (fixes for difficulty, enemy placement and crypt hints worked).
- In the crypt a level 2 fighter beat the first 3-4 mobs (Shambling Dead, 48 XP each) using 2 Cure Light Wounds potions. Difficulty feels about right for level 2 solo.
- BUG (revised): the crypt altar (`plc_altrevil`, appearance 46, valid) shows only as a glow. Read-only investigation: its Z is -2.11 (sunk floor); 14 crypt objects sit at Z -1.5 to -2.6. The Vault altar renders correctly (screenshot). Fix: raise/move the sunk objects onto flat floor (about (125,105) for the altar); fall back to appearance 719 if still glowing.
- Crypt minimap and torches/pillars/props render well; area transition prompt text works ("This appears to be the way into the Voss Family Crypt...").
- Still to report by the user: key drop, vault gate, lantern, other NPCs, turn-in, win message, gold count.

### Confirmed by the user's screenshots (Claude-captured playtest images 2-6)
- Start: Corwin dialog, accepting the quest; 1000 XP and level-up message on entry; journal updated "The Weeping Lantern".
- Crypt portal prompt works; crypt minimap OK; Shambling Dead 48 XP each.
- Thief's Revenant killed (90 XP): drops Short Sword and Crypt Key (key drop CONFIRMED).
- Garrick Dunn killed (137 XP): drops The Weeping Lantern (Chainmail "ignored" by loot rules); lantern acquired, journal updated.
- Maren Voss: returning the lantern works. 200 XP, "Lost Item: The Weeping Lantern", journal: "I returned the lantern to Maren... go back to Elder Corwin with the news". Her farewell dialog reads well.
- Vault altar renders correctly (face relief and red ornaments).
- Still unconfirmed: Corwin turn-in, win message, Pip, Corwin's store, vault gate (key use) wording, final gold/XP totals, 200 vs 150 gold.

### Full completion confirmed (user's second batch of screenshots)
- Corwin turn-in works: option 1 "It is done. Maren Voss is at rest..." gives 275 XP, "*** ADVENTURE COMPLETE ***" and the win message ("Maren Voss rests at last, and the pale light no longer drifts through the woods. Hollowmere sleeps soundly tonight, thanks to you."). Journal moves the quest to the Completed tab. Corwin's dialog has options 2 "Not yet, Elder." and 3 "Do you have any supplies to spare?" (store option present, not exercised).
- Vault entry journal line sets up Garrick ("guards the Weeping Lantern here"); Garrick is a real fight for a level 2 fighter (took "Badly Wounded" exchanges, hits of 17-18 on the player). Killed him for 137 XP; he drops Short Sword + The Weeping Lantern; his Chainmail is "ignored" by the loot rules (not dropped).
- Full XP trail seen: 1000 (start) + 48 per Shambling Dead + 90 Thief's Revenant + 137 Garrick + 200 Maren + 275 Corwin.
- Inventory shows GP 200 after the run (start grant 150): +50 still unexplained but it is not a bug by itself (probably loot); close the 200-vs-150 question unless it reproduces.
- ISSUE (minor): "The Weeping Lantern" is a Kama weapon (base item Kama, 1d6 slashing, needs Exotic or Monk). It shows red/unusable for a Fighter in the inventory. Suggest a non-weapon base item (e.g., a torch/misc item) for a quest lantern.
- Not exercised: Pip (the lost boy mentioned by Corwin) was never seen; Corwin's store; vault gate door wording. Both extra quest paths may still be untested.

## Spot fixes after the full playtest (pre-polish)
Backup: `modules/the-weeping-lantern.pre-polish.mod`. Repacked to `the-weeping-lantern.mod`. Not re-played.

1. **Altar / sunk crypt objects.** The evil altar in the Voss Family Crypt was at Z -2.11 on a depressed walkmesh spot. Moved to (125,105), Z 0 in Room D. Also moved `aff_vc_barrel` from (135,105), Z -2.59 to (115,125), Z 0, and the evil-room bloodstain from (121,46) to (128,40) (Room C; height recheck reported 0 more fixes). `fix_object_heights` raised 3 objects to Z 0 in the first pass. Appearance was left at 46 (`AltrEvil`, model PLC_H04, which exists); row 719 (`new:Evil Altar`) is still the fallback if the altar still renders badly.
   - **Not moved:** the other negative-Z crypt objects in Room B (Good Sarcophagus, Bookshelf, Stocks, Brazier, bones, pillar) and in the corridors (Z -1.5). Room B has no flat floor at all (every walkable spot there is -1.5 to -2.6), so those objects sit on the real floor of a sunken room. If any of them also look wrong in game, the fix is a visual check, not a coordinate move.
   - The barrel and a cobweb (`x3_plc_sweed002`) now share (115,125). Cosmetic.
2. **Lantern item type.** `weeping_lantern` changed from BaseItem 40 (Kama, a weapon) to 24 (Misc Small, non-equippable). Tag/resref unchanged. The copies embedded in `garrick_dunn.utc` and in the placed Garrick in `sealedvault.git` carried their own BaseItem 40 and were updated too (a blueprint-only change would have left the boss with a Kama). After reload, `get_creature_details` for `garrick_dunn` shows `weeping_lantern`, baseItem 24. The item's model part and icon are the Misc Small defaults, so it will show a generic icon.
3. Verification: `verify_quest_completability` still passes for both quests; scripts that test the lantern use the tag, which is unchanged (`c_qlant_has`, `a_qlant_s4`). Not live-tested (Pip, store and vault gate skipped as requested).
4. `validate_module`: see the number in the session report below; same base-game false positives as before.
