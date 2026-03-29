# Detection Checklist

Use this checklist to avoid confusing a Unity build with a Unity source project.

## Source project indicators

- `Assets/`
- `Packages/`
- `ProjectSettings/`
- `ProjectSettings/ProjectVersion.txt`
- editable source files already present such as `.cs`, `.unity`, `.prefab`, `.mat`

## Build indicators

- `<GameName>.exe` or platform launcher
- `<GameName>_Data/`
- `GameAssembly.dll`
- `UnityPlayer.dll`
- `globalgamemanagers`
- `sharedassets*.assets`
- `resources.assets`
- `level*`
- `StreamingAssets/`

## IL2CPP indicators

- `GameAssembly.dll`
- `<GameName>_Data/il2cpp_data/`
- `<GameName>_Data/il2cpp_data/Metadata/global-metadata.dat`

## Mono indicators

- `<GameName>_Data/Managed/Assembly-CSharp.dll`
- no IL2CPP metadata directory

## Version clues

- parse `globalgamemanagers` for strings like `2020.3.44f1`
- inspect recovered `ProjectSettings/ProjectVersion.txt` if available
- some tools will surface the version after loading the game

## Packaging clues worth calling out

- `StreamingAssets/aa/` often means Addressables
- `.bundle` files usually indicate AssetBundles or Addressables content
- `GeneratedSoundBanks` usually means Wwise
- FMOD often appears via `FMOD` folders or plugin DLL names
