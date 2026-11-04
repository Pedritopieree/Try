# GCUnlock Pro — LSPosed Module for Vivo GameCube

Replacement content for the Try repository.  
Based on analysis of GameCube Unlock 0.1.36 + classic GameCube unlock patterns.

## Requirements

- Vivo / iQOO with GameCube (`com.vivo.gamecube`)
- LSPosed (or LSPatch) — API 102 preferred
- Android 12+ (SDK 31+), OriginOS 4 / 5
- Tested target: **iQOO Z10 Turbo Pro (PD2453 / SM8735)**

## Build (GitHub Actions)

Push to `main` or run the workflow manually:

- Workflow: `.github/workflows/build.yml`
- Artifact: **GCUnlock-APK** → `app-debug.apk`

Local:

```bash
./gradlew assembleDebug
# app/build/outputs/apk/debug/app-debug.apk
```

## Install

1. Install the APK  
2. Enable the module in **LSPosed Manager**  
3. Scope:  
   - `com.vivo.gamecube`  
   - `com.vivo.gamewatch`  
   - `system`  
4. Reboot  

## Features

| Feature | Target area |
|--------|-------------|
| Super Resolution / AI / Extreme | VSRController, superresolution.*, isSupport* |
| 144 Hz / frame interpolation | RrmDisplayHooks, framedisplay.* |
| Thermal bypass | TemperatureHelper, ThermalFps/Graphics |
| Monster+ | gamemanipulation.* |
| Super Touch / HawkEye / Gyro | SrHooks capability force |
| Audio unlock | gamecustomsound.*, changevoice |

## Architecture

```
ModuleEntry (LSPosed API 102)
├── hooks/backend/   — VSR, RRM, Thermal FPS/Graphics
├── hooks/thermal/   — CubeThermal
├── hooks/profiles/  — FrameHooks, SrHooks
├── hooks/overlay/   — Monster+
├── hooks/general/   — Audio / sound
└── core/            — AppRule, ConfigBridge, ConfigSnapshot, DevicePolicy
```

## Re-derive after GameCube update

Obfuscated classes (`a`, `b`, `d`…) change every update.

```bash
adb pull /system/framework/vivo-services.jar
jadx -d out/ vivo-services.jar
grep -r "setVSR\|isSupportSuperResolution\|requestVSR" out/sources/
```

## Magisk companion (optional)

For full SQLite whitelist of all third-party games, use the Magisk module  
`vivo_gamecube_unlock_pro_v3.0.zip` together with this LSPosed module.

## License

Community / educational use. Not affiliated with Vivo or the original GameCube Unlock author.
