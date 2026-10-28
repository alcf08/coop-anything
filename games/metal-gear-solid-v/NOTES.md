# Metal Gear Solid V: The Phantom Pain: co-op notes

Research-only pass (no code written, no game installed, no reverse engineering done by us).
All findings are from public pages, the GitHub API, and web search. Dates are when verified.

## Game identity

| | Value |
|---|---|
| Game | Metal Gear Solid V: The Phantom Pain |
| Release | 2015 |
| Engine | Fox Engine (Konami, closed/proprietary) |
| Scripting | Lua 5.1, embedded; most game logic is scripted |
| Store tested | none by us; mods target the Steam release (AppID 287700) |
| Exe hash | unknown (not computed; no install touched) |

Not yet in coop-anything's DIRECTORY.md before this pass. Related game not covered here:
MGSV: Ground Zeroes (separate game, same engine); no co-op project found for it, not searched
in depth.

## What the owner wants

Play The Phantom Pain in co-op with friends. Only a 2-player route exists today (Dynamite).
Nothing supports 3+ players in a playable build yet.

## Research log

### 2026-10-28 Phase 0-1: triage + research (web only)

Repo cloned at the current `main` tip. No MGSV issue or PR checked on the hub yet via `gh`
(web search used instead this session).

#### Found (with sources)

| Find | What it really is | License | Last activity | Players | Use for us |
|---|---|---|---|---|---|
| [unknown321/dynamite](https://github.com/unknown321/dynamite) | The real one. 2-player co-op mod, "early development stage". Real mission playthroughs on video (missions 5, 15). PC Gamer: "a functioning Metal Gear Solid 5 co-op mod". Ships a `masterserver/` and a `hook/`. Requires unmodified Steam copy (JP not supported) and a game-version **rollback** before install (issue #136). | GPL-3.0 **+ Commons Clause** (no selling) | pushed 2026-09-29; created 2025-07-20; v0.6.0 video 2025-10-31; 476 stars, 55 open issues | 2 | Path 1 target: revive/finish. Bug reports in its GitHub issues. |
| [Hound-Protocol/Joint-Operations](https://github.com/Hound-Protocol/Joint-Operations) | Aims for up to 16 players (1 host + 15 clients) in the open world, Windows + Linux/Proton. Heavy org/docs/research scaffolding; README states **"Nothing is playable yet."** First build 0.1 (2 players) is *planned*. Plans to use the game's own online functionality + Steamworks SDK. | GPL-3.0-only (docs CC BY-SA 4.0) | pushed 2026-10-25; created 2026-10-21; 0 stars, 0 issues | up to 16 (planned) | Watch. Public research/decision notes may be useful. Clean-room IP policy (won't copy Dynamite code). |
| [alex-ppg/mgs-v-multiplayer-mod](https://github.com/alex-ppg/mgs-v-multiplayer-mod) | Placeholder repo. One-line README; co-op stated as the first target. No code yet. | CC0-1.0 | created + pushed 2026-10-25; 0 stars, 0 issues | unknown | Watch; nothing usable yet. |
| MGSV Modding Wiki: [Infinite Heaven](https://www.nexusmods.com/metalgearsolidvtpp/mods/1226), [V_Framework](https://mgsvmoddingwiki.github.io/V_Framework/), [IHHook](https://mgsvmoddingwiki.github.io/IHHook), [Zeta](https://mgsvmoddingwiki.github.io/Zeta) | The modding stack, not co-op itself. Infinite Heaven = big Lua mission framework. V_Framework = Lua layer exposing UI/sound/soldiers/bosses/vehicles ("breaks the wall between Lua and the exe"), needs IH + game 1.0.15.3/1.0.15.4. IHHook = `dinput8.dll` proxy adding Lua C API. Zeta = in-game mod manager/framework. | varies | current | The reason co-op is feasible here at all: native hook (IHHook) + rich Lua control (IH/V_Framework). |
| [TinManTex/MockFox](https://github.com/TinManTex/MockFox) | Runs the game's Lua + IH scripts outside the game in a Lua interpreter; used to generate IH docs. | see repo | - | Offline Lua analysis without launching the game. |

#### Observed (facts from sources)

- Fox Engine is closed/proprietary (Konami); no decomp or source port exists.
- MGSV is heavily Lua 5.1 scripted; Lua lives in `data1.dat`/patch `.dat` and inside `fpk` files.
  This is what the IH/V_Framework stack hooks.
- FOB (Forward Operating Base) is asynchronous PvP infiltration; Metal Gear Online (MGO) is a
  separate, unrelated multiplayer mode. Neither is co-op campaign.
- Dynamite is the only build anyone reports actually playing co-op in.

#### Derived (not verified by us)

- This is a **Path 1 (revive/finish)** situation, not build-from-zero: a functional 2-player
  mod exists. The honest play-now answer for an owner is Dynamite.
- Dynamite's `masterserver/` + "online connection" requirement implies hosted matchmaking and/or
  relay, with the game instances synced host-authoritatively. The exact transport is **not
  confirmed** from public docs read so far.
- A co-op campaign on the Fox Engine almost certainly rides on the Lua layer (IH/V_Framework) +
  a native hook (IHHook-style `dinput8.dll` proxy) for state/entity sync, since there is no
  usable built-in co-op netcode and no source.

#### Unknown (next to check)

- Dynamite's exact networking model (direct-IP? relay? P2P + master server for matchmaking?) and
  its supported game build number / the exact version to roll back to (issue #136).
- What Dynamite actually syncs beyond "two players in the same mission": world state, Mother Base,
  items, save/progression ownership. Needs reading its docs (`webui/docs/docs.md`, `docs/MAKING_OF.md`)
  and/or a test install.
- Whether Dynamite builds on Infinite Heaven or hooks the game independently.
- License practicality: GPL-3.0 + Commons Clause allows forking/derivatives (non-commercial);
  fine for reviving, not for selling.
- Ground Zeroes: any co-op attempt (not searched in depth).

## Sources

- https://github.com/unknown321/dynamite
- https://www.pcgamer.com/games/action/after-10-years-theres-a-functioning-metal-gear-solid-5-co-op-mod-so-you-and-a-friend-can-be-played-like-a-damn-fiddle-together/
- https://github.com/Hound-Protocol/Joint-Operations
- https://github.com/alex-ppg/mgs-v-multiplayer-mod
- https://mgsvmoddingwiki.github.io/V_Framework/
- https://mgsvmoddingwiki.github.io/IHHook
- https://www.nexusmods.com/metalgearsolidvtpp/mods/1226
- https://github.com/TinManTex/MockFox
