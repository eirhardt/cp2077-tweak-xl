# Local 2.5 build

Private mirror of [psiberx/cp2077-tweak-xl](https://github.com/psiberx/cp2077-tweak-xl) (MIT).

| Branch | Contents |
|---|---|
| `master` | Upstream as of Oct 23, 2026 (1.11.5, no 2.5 support) |
| `pr22` | Community 2.5 port, [PR #22](https://github.com/psiberx/cp2077-tweak-xl/pull/22) by KelpHect, at `fee3a94`. Not merged or reviewed upstream. Chosen over PR #23 because it matches the Codeware PR #63 SDK lineage. |
| `local-build` | `pr22` plus one build fix (below). This is what's installed in the game. |

## Build fix

The prebuilt TiltedCore package references `mi_malloc_size`, which mimalloc 3.x removed,
so linking failed. `xmake.lua` pins mimalloc 2.x:

```lua
add_requireconfs("tiltedcore.mimalloc", {version = "2.x", override = true})
```

## Build

Visual Studio 2022 Build Tools (C++) and xmake. Clone to a short path (long SDK file
names exceed the Windows path limit) with `git -c core.longpaths=true clone --recursive`.

```powershell
xmake f -m release -y
xmake -y
pwsh -File tools\dist\package-mod.ps1 -ReleaseBin build\windows\x64\release\TweakXL.dll
```

Output: `build\dist\TweakXL-1.11.5.zip`, extract into the game folder.

Switch back to the official release once TweakXL ships 2.5 support.
