# GB-Link Companion for Android

An Android companion application that combines **mGBA emulation** with save data from a **physical Game Boy Advance cartridge**, connected through a GB-Link USB adapter and a GBA or GBA SP.

The application loads ROMs from phone storage, reads the matching cartridge's save before a new play session, and writes in-game saves back to the cartridge. Players can disconnect GB-Link during gameplay and reconnect later to synchronize their progress.

This project is an Android adaptation of [GB-Link Companion](https://github.com/Shinygati/GB-Link-Companion), created by **Shinygati**. It reuses the original project's cartridge helper, protocol work, and icon assets, with an Android interface and an embedded mGBA frontend.

> **Development status:** version **0.1.1 alpha**. The application builds and passes the automated checks described below. End-to-end validation on a real Android phone, GB-Link adapter, and cartridge remains outstanding.

## Features

- **Integrated mGBA:** play GBA ROMs within the app, without launching a separate emulator application.
- **Cartridge-backed saves:** read the real save at the start of each new session and write updated saves back with read-back verification.
- **Disconnected play:** unplug GB-Link without stopping emulation. Pending saves remain on the phone, with write-back prompts when exiting, returning to the main screen, or reconnecting.
- **Automatic backups:** retain timestamped cartridge-save backups and detected mGBA in-game saves, including saves made while disconnected.
- **Conflict detection:** check the cartridge header, revision, save type, and expected save contents before writing. Keep both copies when the cartridge save has changed independently.
- **Cartridge tools:** export saves, restore compatible raw `.sav` files, and dump cartridge ROMs to the phone library or an exported file.
- **Multiple control options:** use touch controls, Android-compatible gamepads, or the connected GBA's buttons.
- **Bundled firmware:** check the adapter's reported version and offer installation of official **GB-Link RP2040 firmware 2.2.7** over USB. Firmware installation validates the bundled image, backs up affected flash sectors, and verifies writes before rebooting.

Supported save formats are SRAM/FRAM 32 KB, SRAM 64 KB, flash 64 KB and 128 KB, and EEPROM 512 B and 8 KB.

## Hardware and software requirements

| Component | Requirement |
| --- | --- |
| Android device | Android 8.0 or newer, with USB host/OTG support |
| USB connection | A compatible USB OTG cable or adapter |
| GB-Link | The official RP2040-based GB-Link adapter |
| Handheld | A powered Game Boy Advance or GBA SP |
| Link cable | A Game Boy Color link cable compatible with GB-Link |
| Cartridge | A matching GBA cartridge with a supported save chip |
| ROM | An uncompressed `.gba` file stored on the phone |

GB-Link communicates through the handheld's link port; the adapter is not a standalone cartridge reader. The ESP32-based Switch bridge is not part of this setup.

## Getting started

1. Install the Android APK. A development build is included in the delivered local `dist/` folder.
2. Open the app and add a `.gba` ROM from phone storage.
3. Insert the matching cartridge into the GBA or GBA SP and connect the link cable and GB-Link adapter.
4. Connect GB-Link to the Android device over USB OTG and grant the app USB access.
5. Switch on the handheld while holding **START + SELECT**, then select **Connect GB-Link** to start the cartridge helper.
6. Select **Play with cartridge save**. The app checks the cartridge, backs up and reads its save, and starts mGBA.
7. Use the game's normal **Save** command. When connected, saves are written back and verified. When disconnected, the app retains a pending copy and prompts for write-back later.

A pending phone session must be synchronized or explicitly resumed before a new cartridge read can replace it. Save states are not used for cartridge synchronization.

Backups and pending saves can be exported from the app. Export important saves before uninstalling the application or clearing its storage.

Detailed operation and recovery information is available in the [Android guide](android/README.md).

## Adapter firmware

This build includes the unmodified official [GB-Link firmware 2.2.7 release](https://github.com/GB-Link/GBLink-Firmware/releases/tag/v2.2.7), which upstream marks as a **prerelease**.

- Source commit: `1b49b765c350d2ecd7dd4ed79e5428d9848c7443`
- UF2 SHA-256: `958458ac70d393f5423706baf6cbe8621a7701ecd6f237b9b8e3c92579f483cd`

A different or unknown reported firmware version triggers an installation choice. Firmware is not installed automatically. The installer uses the RP2040's USB BOOTSEL/PICOBOOT interface and supports manual BOOTSEL entry when the installed firmware cannot reboot the adapter. The bundled UF2 can also be exported for manual recovery.

## Current scope

The Android application focuses on GBA gameplay, cartridge saves, backups, ROM dumping, and adapter firmware management. The original desktop application's Pokémon status and second-screen features are omitted.

GB/GBC cartridge mode, Pokémon Banks, Wonder Cards, flash-cartridge ROM programming, screenshots, and desktop integrations have not been ported. The retained `b2s/` source tree contains upstream tools and helper functionality beyond the Android app's current interface; their presence does not imply Android support for those features.

## Build from source

The supplied project includes the mGBA and GB-Link firmware source trees under `android/third_party/`.

Requirements:

- JDK 17
- Android SDK 35
- Android NDK `27.2.12479018`
- CMake `3.22.1`
- Android Studio, or the Android command-line build tools

Open the `android/` directory in Android Studio, or build from PowerShell:

```powershell
cd android
.\gradlew.bat assembleDebug
```

On Linux or macOS:

```sh
cd android
sh ./gradlew assembleDebug
```

The debug APK is generated at `android/app/build/outputs/apk/debug/app-debug.apk`. The project builds native mGBA libraries for ARM64, ARMv7, and x86-64.

The local `dist/` directory contains the supplied development APK and its checksum. It is excluded from Git; distributable APKs should be attached to repository releases. No signing keys, personal ROMs, or save files are included in this project.

## Testing and validation

The current build passed:

- **546 deterministic host checks** covering transport framing, save integrity, interrupted writes, pending-session recovery, conflicts, backups, and firmware image validation and verification.
- **10 checks against the real mGBA JNI frontend on Windows**, covering save loading, SRAM changes, rendered video, save events, and core reloads.
- Android Java/resource compilation, native builds for the three supported architectures, APK alignment, and v2/v3 signature verification.

Run the host checks with JDK 17 or newer and Python 3:

```sh
python android/tests/run_tests.py
```

These checks do not establish physical hardware compatibility. Android UI behavior, cartridge transfers, and firmware installation still require end-to-end hardware testing. See the [validation record](android/VALIDATION.md) for the remaining checks.

## Project layout

| Directory | Contents |
| --- | --- |
| `android/` | Android application, Gradle wrapper, tests, and technical documentation |
| `android/app/src/main/java/` | Android interface, USB transport, cartridge protocol, and save journal |
| `android/app/src/main/cpp/` | mGBA JNI frontend and save-event integration |
| `android/third_party/mgba/` | mGBA 0.10.5 source |
| `android/third_party/gblink-firmware/` | Official GB-Link firmware 2.2.7 source |
| `b2s/` | Upstream GBA helper source, build tools, and related tests |
| `docs/upstream/` | Preserved original companion README for provenance and historical acknowledgments |
| `dist/` | Local development APK and checksums; excluded from Git |

## Credits and licensing

This project builds on the work of **Shinygati**, **Jeffrey Pfau (endrift) and the mGBA contributors**, the **GB-Link and Celio-Link teams**, **vrodin**, **Raspberry Pi**, and the authors of the helper's fonts and underlying components.

See [CREDITS.md](CREDITS.md) for component-level attribution, source references, and licensing information. Original copyright statements and license files are retained in their respective source trees.

The Android adaptation is distributed under the **GNU General Public License v3.0**; see [LICENSE](LICENSE). Third-party components retain their respective licenses, including mGBA's MPL-2.0, Burn2Slot's MIT license, Raspberry Pi's BSD-3-Clause license, and the font licenses included with the GBA helper.

GB-Link Companion for Android is an independent community project. It is not affiliated with or endorsed by the GB-Link or mGBA teams, Nintendo, Creatures Inc., or GAME FREAK inc. No commercial game ROMs or Nintendo BIOS files are distributed with the application.
