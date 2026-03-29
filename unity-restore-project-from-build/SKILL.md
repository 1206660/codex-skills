---
name: unity-restore-project-from-build
description: Use when recovering a Unity project from a built player, extracted game folder, or partial data dump instead of a source repository. Triggers on requests to reconstruct a Unity engineering workspace from files such as `GameAssembly.dll`, `globalgamemanagers`, `sharedassets*.assets`, `StreamingAssets`, `*_Data`, Addressables bundles, or IL2CPP metadata. Default goal: detect whether the input is a real Unity source project or only a build, recover the maximum viable Unity project with tools like AssetRipper, preserve the recovered output separately from the original files, and state clearly which parts are approximate rather than original.
---

# Unity Restore Project From Build

Use this skill when the input is a Unity player build, not a normal Unity repository.

Default outcome:

`detect build vs source -> identify Unity version and backend -> export recovered project -> document limits`

Do not claim full source restoration unless the repository already contains the original `Assets`, `Packages`, and `ProjectSettings`.

## Workflow

### 1. Determine what you actually have

Inspect the root first.

A real Unity source project usually contains:

- `Assets/`
- `Packages/`
- `ProjectSettings/`

A built player usually contains:

- `<GameName>.exe` or platform binary
- `<GameName>_Data/`
- `GameAssembly.dll`
- `UnityPlayer.dll`
- `globalgamemanagers`
- `sharedassets*.assets`
- `resources.assets`
- `StreamingAssets/`

If the workspace is only a build, say so early and switch to recovery mode.

### 2. Extract version and backend clues

Before exporting anything, identify:

- Unity editor version
- scripting backend: Mono or IL2CPP
- major packaging systems such as Addressables, AssetBundles, Wwise, FMOD

Fast checks:

- parse `globalgamemanagers` for the Unity version string
- inspect `ProjectVersion.txt` if a source project exists
- inspect `ScriptingAssemblies.json`
- check for `GameAssembly.dll` plus `il2cpp_data/Metadata/global-metadata.dat`

Read `references/detection-checklist.md` if you need the concrete indicators.

### 3. Preserve the original input

Never export back into the original build directory.

Create a sibling or child recovery directory such as:

```text
RestoredProject/
  ExportedProject/
  AuxiliaryFiles/
```

Keep any temporary tools in a separate scratch folder such as `.codex_tmp/`.

Do not rename, move, or overwrite original player files unless explicitly asked.

### 4. Use a recovery tool, not ad hoc guessing

Prefer AssetRipper for full-project recovery from Unity builds.

Typical flow:

1. obtain a recent Windows build of AssetRipper if one is not already available
2. launch it in headless mode on a fixed local port
3. load the game folder through the local web API
4. export a Unity project into the recovery directory
5. capture the export log

If the recovered project comes from IL2CPP, expect:

- decompiled `Assembly-CSharp` output rather than original source
- partial package reconstruction
- some manual repair after import into Unity

Read `references/assetripper-playbook.md` before driving AssetRipper or explaining the expected output.

### 5. Verify the exported project shape

Confirm the recovered output contains at least:

- `Assets/`
- `Packages/`
- `ProjectSettings/`

Then sample-check for:

- recovered scenes under `Assets/Scenes`
- decompiled scripts under `Assets/Scripts`
- copied `StreamingAssets`
- auxiliary dumped assemblies or path maps

Do not stop after "export completed" if the output is obviously incomplete.

### 6. Record what was restored and what was not

Always leave a short restore note beside the recovered project.

Include:

- source input path
- detected Unity version
- scripting backend
- export tool and version
- output paths
- major limitations
- next steps for opening the project in Unity

Be explicit that a recovered project is approximate when:

- scripts were decompiled from IL2CPP
- third-party packages are missing from `manifest.json`
- shader or importer settings may need repair
- asset references may need manual relinking

### 7. Recommend the first follow-up actions

Default next steps:

1. install the detected Unity version in Unity Hub
2. open the recovered `ExportedProject`
3. allow a full reimport
4. fix missing packages and plugins
5. address console errors before deeper editing

## Non-Goals

This skill does not imply:

- bypassing DRM or license restrictions
- claiming byte-for-byte restoration of the original repository
- recreating proprietary source comments, history, or exact folder names
- silently overwriting the only copy of a build

## References

- For quick build-vs-source and backend detection cues: `references/detection-checklist.md`
- For the concrete AssetRipper recovery sequence and expected caveats: `references/assetripper-playbook.md`
