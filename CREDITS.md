# Credits and Third-Party Notices

GB-Link Companion for Android is derived from an existing open-source companion application and incorporates work from the projects listed below. Attribution describes the source and role of each component; it does not imply endorsement by its original authors.

## Original companion application

**Shinygati** created [GB-Link Companion](https://github.com/Shinygati/GB-Link-Companion), the original desktop application and the foundation for this Android adaptation.

The Android implementation was initially based on upstream commit `23638b2a741672db86f02402c54b76e7c57f61a3`. Reused work includes the GBA helper binary and sources, cartridge and multiboot protocol behavior, cartridge-save handling approaches, and icon assets. The original project's README and acknowledgments are preserved in [docs/upstream/GB-Link-Companion-README.md](docs/upstream/GB-Link-Companion-README.md). Paths within that historical document refer to the original desktop project.

The Android adaptation adds the Android interface, USB-host transport, embedded mGBA frontend, persistent save journal, disconnected-session recovery, save-backup workflow, and RP2040 firmware installation interface. It is distributed under GPL v3, with upstream and third-party notices retained.

## Components used by the Android application

| Project / authors | Contribution | License / notices |
| --- | --- | --- |
| [mGBA](https://github.com/mgba-emu/mgba) — Jeffrey Pfau (**endrift**) and contributors | Embedded GBA emulation, video, audio, input, and save memory | MPL-2.0; [source license](android/third_party/mgba/LICENSE), plus component notices within that tree |
| [GB-Link](https://gblink.io) and the [GB-Link / Celio-Link firmware contributors](https://github.com/GB-Link/GBLink-Firmware) | USB adapter, firmware, link protocols, and hardware integration | GPL v3 and the notices included in [the firmware source](android/third_party/gblink-firmware/) |
| [GB-Link Multiboot Web](https://github.com/GB-Link/gba-multiboot-web) — GB-Link team and contributors | Reference and upstream implementation for sending a helper to a real GBA | GPL v3; attribution retained in the upstream `b2s` documentation |
| [GB-Link Cart Doctor](https://github.com/GB-Link/gb-cart-doctor-web) — GB-Link team and contributors | Cartridge tools and transfer techniques incorporated by the upstream GBA helper | GPL v3; component-level notices remain authoritative |
| [Burn2Slot](https://github.com/vrodin/Burn2Slot) — **vrodin** | Foundation of the upstream GBA helper's cartridge, flash, and link implementation | MIT; [LICENSE-Burn2Slot](b2s/LICENSE-Burn2Slot) |
| [picotool](https://github.com/raspberrypi/picotool) and Pico SDK — **Raspberry Pi (Trading) Ltd.** and contributors | Reference implementation and definitions for the RP2040 PICOBOOT USB protocol used by the Android installer | BSD-3-Clause; [bundled notice](android/app/src/main/assets/NOTICE-picotool.txt) |

The GB-Link Cart Doctor transfer work also credits **FIX94's GBA Link Cable Dumper** in its upstream lineage. This attribution is preserved alongside the original project references; it should not be interpreted as a claim that every upstream tool is exposed by the Android interface.

### Pinned source versions

- **mGBA 0.10.5:** `26b7884bc25a5933960f3cdcd98bac1ae14d42e2`.
- **GB-Link firmware 2.2.7:** `1b49b765c350d2ecd7dd4ed79e5428d9848c7443`.

mGBA's source is retained with its original notices. The Android build generates a frontend translation unit that includes the save hook from `android/app/src/main/cpp/companion_hooks.h`. Firmware sources retain their upstream build workflow and Zephyr dependency manifest; Zephyr and its modules retain their own licenses and author notices.

## Fonts retained with the GBA helper

The upstream helper contains font and drawing assets, including code for screens the Android host does not enable. Their notices remain applicable to the retained sources and helper binary.

| Font / authors | Use in upstream helper | Notice |
| --- | --- | --- |
| **DejaVu Sans Mono**, DejaVu contributors and **Bitstream** | Helper's 8×12 font | [LICENSE-DejaVu](b2s/LICENSE-DejaVu); DejaVu modifications are identified as public domain in the retained notice |
| [Spleen](https://github.com/fcambus/spleen), **Frederic Cambus** | Small helper/status-screen font | BSD-2-Clause; [LICENSE-Spleen](b2s/LICENSE-Spleen) |
| [Misaki](https://littlelimit.net/misaki.htm), **Num Kadoma** | Kana glyphs in the helper | [LICENSE-Misaki](b2s/LICENSE-Misaki) |

## Historical desktop-project acknowledgments

The original desktop companion also credits the following projects and authors for functionality outside the current Android scope:

- **Wired-Ariel** and [gen3-poke-multiplayer](https://github.com/Wired-Ariel/gen3-poke-multiplayer), for multiboot reference work and inspiration for the desktop Wonder Card implementation.
- **kwsch** and [PKHeX contributors](https://github.com/kwsch/PKHeX), for Pokémon save and Bank functionality in the desktop application.
- **Lesserkuma** and [FlashGBX contributors](https://github.com/lesserkuma/FlashGBX), for flash-cartridge definitions used by the desktop tools through Cart Doctor.
- **pret contributors**, for the Pokémon decompilations used as references by the desktop status and Wonder Card features.
- The **GB-Link Wonder Cards contributors**, including **Goppier / Project Wonder**, **Blisy**, **Youpileouf**, and the other authors identified by the upstream collection.
- **No-Intro**, for the desktop project's ROM-verification data.
- **Gio contributors**, for the desktop UI toolkit.

These acknowledgments preserve the original project's provenance. Pokémon Banks, Wonder Cards, status screens, No-Intro verification, and the Gio desktop interface are not implemented as Android app features in this alpha. See the preserved upstream README and original repositories for their complete credits and licenses.

## License preservation

The repository's [LICENSE](LICENSE) governs the GPL v3 Android adaptation. Upstream materials retain their own copyright and licensing terms. The original companion license is also preserved in [docs/licenses/GB-Link-Companion-LICENSE](docs/licenses/GB-Link-Companion-LICENSE).

This credits document supplements, and does not replace, the full license texts and file-level notices in `android/third_party/`, `b2s/`, and `android/app/src/main/assets/`. Additional authors and component licenses identified in those source trees remain credited by their original notices.

Product names and trademarks belong to their respective owners. No affiliation or endorsement is implied.
