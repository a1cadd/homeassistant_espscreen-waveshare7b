# Waveshare ESP32-P4-WIFI6-Touch-LCD-7B for Tessera

This is a local Tessera/ESPHome board profile for the Waveshare **ESP32-P4-WIFI6-Touch-LCD-7B**.
It uses the official Waveshare 7B values:

- 1024 x 600 EK79007 MIPI-DSI panel, two lanes
- LCD reset GPIO33
- active-low PWM backlight GPIO32
- GT911 touch over I2C GPIO7/GPIO8, polled (no INT/RST pins)
- ESP32-C6 hosted Wi-Fi/BLE link using the shared P4/C6 profile
- 32 MB flash

## Files

- `packages/boards/waveshare-esp32p4-7b.yaml`: board hardware profile
- `checkout/wavesharep4wifi6lcd7b.yaml`: local checkout build entry
- `packages/wavesharep4wifi6lcd7b.yaml`: remote-package entry for an installed Tessera build

## Local build

From the repository root:

```sh
cp secrets.yaml.example checkout/secrets.yaml
# Edit checkout/secrets.yaml with the local Wi-Fi and generated API/OTA values.
esphome config checkout/wavesharep4wifi6lcd7b.yaml
esphome compile checkout/wavesharep4wifi6lcd7b.yaml
esphome upload checkout/wavesharep4wifi6lcd7b.yaml
```

For a Home Assistant installation, the normal Tessera Screen Manager profile-generation flow is preferred; this profile is intended to be added as a board entry or used as a local checkout profile.

## Important first-flash check

The shared P4/C6 hardware profile currently targets pre-revision-3 ESP32-P4 silicon (`engineering_sample: true`), matching the P4 panels documented by the source project. Check the first USB boot log. If the chip reports revision 3 or newer, add this override after the board package:

```yaml
esp32:
  engineering_sample: false
```

This profile has been syntax-checked, but it has not been flashed to physical hardware here. Before relying on it, verify the panel on USB with the display self-test, then check cold boot, touch orientation, brightness, and OTA.
