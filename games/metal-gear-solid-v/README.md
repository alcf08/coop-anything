# Metal Gear Solid V: The Phantom Pain (2015)

| | |
|---|---|
| Engine | Fox Engine (Konami, closed/proprietary; Lua 5.1 scripting) |
| Versions tested | none by us (research only); mods target the Steam release, AppID 287700 |
| Projects | [Dynamite](https://github.com/unknown321/dynamite) (2p), [Joint Operations](https://github.com/Hound-Protocol/Joint-Operations) (planned 16p), [alex-ppg mod](https://github.com/alex-ppg/mgs-v-multiplayer-mod) (placeholder) |
| QA level | none (no testing done by us) |
| Last updated | 2026-10-28 |

## Where things stand

Two players can play The Phantom Pain together today using **Dynamite**, an early but
functioning co-op mod (PC Gamer called it "a functioning Metal Gear Solid 5 co-op mod",
with full mission playthroughs on video). It is explicitly early-development and buggy, needs
an unmodified Steam copy (not the Japanese version), and requires rolling the game back one
patch before installing. There is **no playable route for 3+ players**: Joint Operations aims
for 16 but "nothing is playable yet," and alex-ppg's repo is a one-line placeholder. We did
research only this pass — nothing here was installed, reverse-engineered, or tested by us.

## Existing work

- **[Dynamite](https://github.com/unknown321/dynamite)** by unknown321 — 2-player co-op, "early
  development stage." 476 stars, 55 open issues; last push 2026-09-29 (created 2025-07-20); v0.6.0
  gameplay video dated 2025-10-31. Requires an unmodified Steam copy (JP unsupported), an online
  connection, free space, and a friend; you **must roll the game back to the previous version**
  before installing (issue [#136](https://github.com/unknown321/dynamite/issues/136)). Ships a
  `masterserver/` and a `hook/` component. License: **GPL-3.0 with the Commons Clause** (derivatives
  fine, selling not). Docs: `docs/MAKING_OF.md` and `webui/docs/docs.md` (shipped with the build).
  This is the one build players and press report actually playing.
- **[Joint Operations](https://github.com/Hound-Protocol/Joint-Operations)** by Hound Protocol —
  aims for up to 16 players (1 host + 15 clients) in the open world, Windows + Linux/Proton, using
  the game's own online functionality and the Steamworks SDK. As of 2026-10-25 it is organization,
  documentation, website and desk research only; the README states **"Nothing is playable yet"** and
  the first (2-player) build, 0.1, is planned. GPL-3.0-only (docs CC BY-SA 4.0); clean-room IP policy
  (will not copy Dynamite's code).
- **[alex-ppg/mgs-v-multiplayer-mod](https://github.com/alex-ppg/mgs-v-multiplayer-mod)** — a
  placeholder repo created 2026-10-25, one-line README, co-op named as the first target, no code yet.
  CC0-1.0.

## How the game works (for co-op)

- **Engine:** Fox Engine is closed and proprietary; there is no decompilation or source port, so
  any co-op work is native hooking plus scripting, not code changes to an open engine.
- **Scripting:** most game logic is **Lua 5.1**, embedded in the executable, loaded from
  `data1.dat`/patch `.dat` files and from inside `fpk` archives. This is the lever the modding scene
  pulls on.
- **Modding stack that makes co-op feasible:**
  [Infinite Heaven](https://www.nexusmods.com/metalgearsolidvtpp/mods/1226) (large Lua mission
  framework), [V_Framework](https://mgsvmoddingwiki.github.io/V_Framework/) (a Lua layer exposing
  UI, sound, soldiers, bosses and vehicles that the game didn't expose; needs IH and game 1.0.15.3 or
  1.0.15.4), and [IHHook](https://mgsvmoddingwiki.github.io/IHHook) (a `dinput8.dll` proxy DLL that
  adds a Lua C API to the statically-compiled game Lua). [MockFox](https://github.com/TinManTex/MockFox)
  can run the game's Lua and IH scripts outside the game for offline analysis.
- **Netcode:** the shipped game has no co-op campaign netcode. FOB is asynchronous PvP; Metal Gear
  Online is a separate mode. So co-op means running single-player logic on a sync layer added by the
  mod (host authority with state/entity sync is the likely model). Dynamite's exact transport is not
  yet confirmed from its public docs.
- **Save/progression, world state, Mother Base:** how these are owned and synced in co-op is
  **unknown** to us — not yet read from Dynamite's docs or verified.

## What we changed

Nothing. This pass is research and directory/write-up only.

## QA

No tests were run by us. QA level: `none`. Reported play (Dynamite) is from the author's gameplay
videos and press coverage, not structured testing.

## How to play

For 2 players today: install **Dynamite** per its own documentation
(https://github.com/unknown321/dynamite) — unmodified Steam copy (not JP), roll the game back one
patch first (issue #136), then follow the shipped user docs. The owner runs these steps on their own
machine; coop-anything does not distribute the game or the mod.

## Known issues

- Dynamite is early/buggy by its author's own description ("expect bugs"); 55 open issues.
- 2 players only. No working option for 3+.
- Requires a game-version rollback, which can be undone by Steam updating the game.
- Japanese Steam version unsupported.

## Next steps

- Read Dynamite's `webui/docs/docs.md` and `docs/MAKING_OF.md` to pin down: supported build number,
  networking model, and exactly what is synced (world state, Mother Base, items, progression).
- Mine Dynamite's GitHub issues into a bug list (turn crash reports into repro steps), with the
  owner's OK before any test install.
- Confirm whether Dynamite builds on Infinite Heaven or hooks independently.
- Keep an eye on Joint Operations' 0.1 (2-player) build and update the directory when it ships.
- If reviving/extending Dynamite: follow `playbook/reviving-abandoned-mods.md` and set up isolated
  test installs + bot-driven QA (`playbook/automated-qa.md`) rather than relying on a human playtest.
