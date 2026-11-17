<!-- DRAFT for github.com/danielcorredorm/CaptureTheFlagMultiplayerDemo — not part of the website build.
     Copy it to the repo root as README.md. Everything here was read from the repo's own source and config. -->

# Capture the Flag — multiplayer demo (Unreal Engine 4.27, C++)

[![Gameplay video](https://img.youtube.com/vi/CHmQ_FNKYJk/hqdefault.jpg)](https://youtu.be/CHmQ_FNKYJk)

A first-person, two-team capture-the-flag game over Steam sessions: host or find a match, wait in a lobby, then play a timed round where each team tries to carry the other team's flag to its own zone. I built it in C++ for a technical test in 2022.

**More projects:** https://danielcorredorm.dev/projects/

## Highlights

| System | Where | What it shows |
|---|---|---|
| Sessions | [`Engine/TaskGameInstance`](Source/CTFTask/Engine/TaskGameInstance.cpp), [`UI/TaskMenu`](Source/CTFTask/GameFramework/UI/TaskMenu.cpp), [`UI/TaskServerList`](Source/CTFTask/GameFramework/UI/TaskServerList.h) | Online Subsystem session interface: create, find (presence search) and join sessions, with a server list in the menu; Steam as the platform service, LAN when the subsystem is `NULL` |
| Match flow | [`Game/TaskGameModeMainMenu`](Source/CTFTask/GameFramework/Game/TaskGameModeMainMenu.cpp), [`Game/TaskGameModeLobby`](Source/CTFTask/GameFramework/Game/TaskGameModeLobby.cpp) | Startup → main menu → lobby → gameplay with seamless `ServerTravel`; the lobby starts the match when the session is full and sends everyone back to the menu after 180 s if it never fills |
| Teams | [`Game/TaskGameModeGameplay`](Source/CTFTask/GameFramework/Game/TaskGameModeGameplay.cpp), [`Game/TaskPlayerState`](Source/CTFTask/GameFramework/Game/TaskPlayerState.h) | The server balances players into Red and Blue on login and removes them on logout; the team replicates on the player state and drives the character's team color |
| Flag | [`Actors/TaskFlag`](Source/CTFTask/GameFramework/Actors/TaskFlag.cpp), [`Game/TaskFlagZone`](Source/CTFTask/GameFramework/Game/TaskFlagZone.cpp) | Replicated actor: pickup through a server RPC, attached to a socket while carried, physics and collision switched in `OnRep_bIsEquipped`, dropped on death, reset to its start transform after a capture |
| Score, timer, result | [`Engine/TaskGameState`](Source/CTFTask/Engine/TaskGameState.cpp) | Team scores, match time and the end-of-match announcement are replicated properties whose `OnRep` updates the HUD |
| Combat and respawn | [`Character/TaskCharacter`](Source/CTFTask/GameFramework/Character/TaskCharacter.cpp), [`Actors/TaskProjectile`](Source/CTFTask/GameFramework/Actors/TaskProjectile.cpp) | Projectiles spawned on the server, damage through `ApplyDamage`, replicated health and elimination, respawn at a random player start |
| Aim sync | [`Character/TaskCharacter`](Source/CTFTask/GameFramework/Character/TaskCharacter.h) | Server and multicast RPCs keep the first-person aim rotation in sync for other players |

## Build and play

1. Unreal Engine **4.27** and Visual Studio with the C++ game development workload.
2. Right-click `CTFTask.uproject` → **Generate Visual Studio project files**, build **Development Editor | Win64**, then open the project.
3. The game starts on the `Startup` map; the editor opens on `gameplay`. Steam is the default online platform (see `Config/DefaultEngine.ini`).

| Action | Key |
|---|---|
| Move / look | WASD / mouse |
| Fire | Left mouse button |
| Pick up the flag | E |
| Jump | Space |
| Crouch | Left Ctrl |

## Notes

- Built against a delivery deadline for a technical test, so it is a compact demo, not a finished game.
- Based on Unreal Engine's first-person template (arms mesh, projectile).
- No license: the source is shared to read and study; all rights reserved.

## Contact

Daniel Corredor — Senior Gameplay Engineer, Unreal Engine · [Portfolio](https://danielcorredorm.dev) · [LinkedIn](https://www.linkedin.com/in/danielcorredorm2787)
