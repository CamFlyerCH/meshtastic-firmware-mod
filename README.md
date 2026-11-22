# Meshtastic Firmware Mod

This is the personal, experimental firmware fork maintained by **CamFlyerCH**. It is based on the official [Meshtastic firmware](https://github.com/meshtastic/firmware), an open-source LoRa mesh networking project. This fork is not an official Meshtastic release or supported distribution.

The purpose of this repository is to try router and mesh-behavior changes on current upstream firmware while keeping the earlier `master` branch available. The `develop` branch is the current experiment branch, based on upstream `develop`. Board definitions and PlatformIO configuration on that branch are intended to follow upstream unless explicitly changed.

## Fork-specific router behavior

The `develop` branch currently adds these routing policies:

1. **Hop-start limit in `LOCAL_ONLY` and `KNOWN_ONLY`:** successfully decoded radio packets with `hop_start > 5` are discarded when they are not addressed to this device and arrive on the default or an unknown channel. This applies to every decoded port type; addressed packets and known non-default channels are exempt.
2. **Selected ports are not rebroadcast in those modes:** ATAK Forwarder/Plugin, PAXCOUNTER, IP Tunnel, Audio, Private, Detection Sensor, Range Test, Remote Hardware, Telemetry, and NeighborInfo packets are not relayed. This does not block their local handling or delivery to the phone.
3. **Selected local broadcasts use zero hops on the default channel:** Audio, Detection Sensor, PAXCOUNTER, Serial, Range Test, Telemetry, ZPS, NeighborInfo, Powerstress, and Private broadcasts start with `hop_limit = 0` and `hop_start = 0`. This overrides the configured hop limit for those packets; there is no separate runtime toggle for this fork behavior.

The original firmware routing remains the foundation. Fork-specific code is marked inline in the affected source files so it can be distinguished from upstream logic.

## Build

This project uses PlatformIO with the pioarduino ESP32 platform configured for the Heltec V3 (`heltec-v3`). In VS Code, run the **pioarduino: Build** task. From a terminal with PlatformIO available, run:

```powershell
platformio run -e heltec-v3
```

The build produces firmware images under `.pio/build/heltec-v3/`. Building does not flash a connected device.

For general Meshtastic firmware development and device setup, see the [official documentation](https://meshtastic.org/docs/).
