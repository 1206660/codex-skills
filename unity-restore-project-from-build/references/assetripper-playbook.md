# AssetRipper Playbook

Prefer this workflow when the user provides only a Unity build.

## Goal

Export the maximum viable Unity project into a separate recovery directory without damaging the original build.

## Minimal sequence

1. Create a scratch directory for tools and logs.
2. Download a recent `AssetRipper_win_x64.zip` release if no local copy exists.
3. Extract it to a temp folder.
4. Launch `AssetRipper.GUI.Free.exe` with:

```powershell
AssetRipper.GUI.Free.exe --headless=true --port 54321 --log-path <log-path>
```

5. Load the game folder through the local web endpoint:

```text
POST /LoadFolder
Content-Type: application/x-www-form-urlencoded
path=<absolute game folder>
```

6. Export the recovered Unity project:

```text
POST /Export/UnityProject
Content-Type: application/x-www-form-urlencoded
path=<absolute restore output folder>
```

7. Inspect the log until export completes.
8. Verify the output contains `ExportedProject/Assets`, `Packages`, and `ProjectSettings`.
9. Stop the AssetRipper process when finished.

## Useful API endpoints

- `/openapi.json`
- `/LoadFolder`
- `/Export/UnityProject`

## Expected output shape

Typical recovery output:

```text
RestoredProject/
  ExportedProject/
    Assets/
    Packages/
    ProjectSettings/
  AuxiliaryFiles/
```

`AuxiliaryFiles` often contains dumped assemblies and path maps.

## What AssetRipper usually recovers well

- scenes
- prefabs
- textures and sprites
- materials
- animation clips and controllers
- fonts
- many serialized `MonoBehaviour` assets
- `StreamingAssets`
- decompiled C# from `Assembly-CSharp` and similar game assemblies

## What often still needs manual repair

- `Packages/manifest.json` third-party dependencies
- custom shaders
- plugin setup
- importer details
- some script names, comments, or formatting
- exact original folder structure and naming

## Communication rules

Say clearly:

- this is a recovered project, not guaranteed original source
- IL2CPP restoration yields decompiled C#
- the next step is opening the project in the matching Unity version and fixing package or compile errors
