# ROG STRIX B760-G WIFI S OpenCore EFI

OpenCore EFI for an ASUS ROG STRIX B760-G WIFI S Hackintosh running macOS Tahoe.

> [!WARNING]
> This EFI is hardware-specific. Do not boot it unchanged on another computer. Generate your own SMBIOS values, review the USB map, make a backup, and keep a rescue USB before using it.

## Hardware

| Component | Model / configuration |
| --- | --- |
| Motherboard | ASUS ROG STRIX B760-G WIFI S |
| CPU | Intel Core i7-12700KF (12 cores / 20 threads) |
| GPU | NVIDIA GeForce RTX 4060 8 GB |
| Memory | 32 GB KingBank DDR5-6400 (2 × 16 GB) |
| Wi-Fi / Bluetooth | Intel AX211 |
| Ethernet | Intel 2.5 GbE, driven by AppleIGC |
| Audio | Realtek ALC1220 / S1220A, AppleALC layout-id 11 |
| Storage | WD Black SN770 NVMe (macOS and EFI) |
| SMBIOS | MacPro7,1 |

## Software

| Item | Version / status |
| --- | --- |
| macOS | Tahoe 26.7.1 (25G241) |
| OpenCore | 1.0.9 |
| Picker | OpenCanopy, custom 1920×1080 ROG/Fubuki theme |
| Default boot timeout | 5 seconds |
| NVIDIA graphics | NullMoth NVMTL/NVK driver; experimental |
| Wi-Fi | AirportItlwm with Tahoe/OCLP modern wireless components |

## Working

- Boots macOS Tahoe, Windows and recovery entries from the internal EFI partition.
- NVIDIA RTX 4060 framebuffer and Metal 3 through the experimental NullMoth driver.
- Intel AX211 Wi-Fi networking.
- iPhone is discoverable from macOS.
- iPhone Personal Hotspot can be discovered and connected.
- Intel Bluetooth firmware and patch kexts are included.
- OpenCanopy graphical picker with a custom ROG/Fubuki background.
- Five-second automatic boot; select an entry and press `Ctrl+Enter` to save it as the default.

## Known issues

- **AirDrop and Apple ecosystem continuity features do not work reliably.** The Mac can discover the iPhone and show it as an AirDrop target, but transfers remain waiting or fail. Features that depend on AWDL/Apple-native Wi-Fi behavior should be treated as unsupported with Intel AX211/AirportItlwm.
- **Personal Hotspot is an exception:** the iPhone hotspot can be discovered and used for network access.
- **RTX 4060 acceleration is experimental.** On Tahoe, WindowServer can become CPU-bound during animations, causing desktop motion to run below 60 FPS even though the display is set to 60 Hz.
- Audio may require the matching KDK and the appropriate OCLP Modern Audio root patch in addition to AppleALC.
- Root patches may need to be reapplied after a macOS update.
- RGB lighting control is not part of this EFI. The ASUS Aura controller is detectable as USB `0B05:19AF`, but OpenRGB changes were not reflected by the connected lighting during testing.

## Important: generate new SMBIOS values

The public `config.plist` has deliberately invalid placeholder identity values:

- `SystemSerialNumber`: `C02XXXXXXXXX`
- `MLB`: `C02XXXXXXXXXXXXXX`
- `SystemUUID`: all zeroes
- `ROM`: all zeroes

Generate a fresh `MacPro7,1` identity with GenSMBIOS or another trusted tool before booting. Never reuse somebody else's serial number, MLB, UUID or ROM.

## Tahoe notes

- This configuration disables `SecureBootModel` and uses SIP/AMFI-related settings required by the current root patches and experimental NVIDIA driver.
- The NullMoth boot arguments and kext requirements are already present in `config.plist`, but its user-space driver and Auxiliary Kernel Collection components are installed in macOS, not entirely inside this EFI.
- Modern wireless/audio root patches require a KDK that exactly matches the installed macOS build.
- Keep a known-good rescue EFI before applying root patches or changing GPU settings.

## Included core components

- Lilu 1.7.3
- VirtualSMC 1.3.9, SMCProcessor and SMCSuperIO
- WhateverGreen 1.7.2
- AppleALC 1.9.8
- AirportItlwm plus IOSkywalkFamily/IO80211FamilyLegacy components
- IntelBluetoothFirmware 2.5.0, IntelBTPatcher and BlueToolFixup
- AppleIGC 1.9
- CpuTopologyRebuild 2.0.2
- NVMeFix 1.1.4
- RestrictEvents 1.1.7
- USBToolBox with the included USB map

## Disclaimer

This repository is provided as a reference for the exact hardware above. OpenCore, kexts, root patches and third-party drivers remain the work of their respective authors. Review upstream documentation and licenses before redistribution or reuse.
