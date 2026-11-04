# Open Voice PE v1: openWakeWord

> **Archived (March 2025).** This is the first version of Open Voice PE, kept for reference. It is not maintained; see the [main README](../README.md) for the current version.


**Open Voice PE** is a custom firmware for the [Home Assistant Voice Preview Edition](https://github.com/esphome/home-assistant-voice-pe) project, built with ESPHome on an ESP32-S3. It extends the official Voice PE firmware with additional features for enhanced voice assistant functionality in Home Assistant.

This firmware is designed for users who want to customize their Voice PE device with advanced wake word detection and flexible TTS playback options.

## Features
This custom firmware adds the following features to the official Home Assistant Voice PE:

- **Support for openWakeWord**: Enables local wake word detection using the `openWakeWord` engine, providing an alternative to the default wake word system for improved flexibility and privacy.
- **TTS Playback on Different Media Players**: Allows you to play voice assistant responses on a remote media player (e.g., a Home Assistant media player entity) instead of the local speaker, configurable via a dropdown in Home Assistant.

## Prerequisites
- **Official Home Assistant Voice PE**: This firmware is designed for the [Home Assistant Voice PE](https://github.com/esphome/home-assistant-voice-pe) device. Refer to the [official documentation](https://voice-pe.home-assistant.io) for hardware requirements, assembly instructions, and initial setup.
- **Home Assistant**: A running instance of Home Assistant (version 2025.3.0 or later recommended) with the ESPHome integration installed.
- **ESPHome**: ESPHome (version 2025.3.0 or later) for compiling and uploading the firmware.
- **Wyoming Satellite**: A configured Wyoming satellite in Home Assistant for voice assistant functionality (STT/TTS pipeline).

## Setup Instructions

### 1. Install ESPHome
- Follow the [ESPHome installation guide](https://esphome.io/guides/installing_esphome.html) to set up ESPHome on your system.
- If using the ESPHome Dashboard in Home Assistant, ensure it’s installed via HACS or manually.

### 2. Configure Secrets
- Create a `secrets.yaml` file in your ESPHome config directory with the following:
  ```yaml
  # Your Wi-Fi SSID and password
  wifi_ssid: "your-wifi-ssid"
  wifi_password: "your-wifi-password"

  # API encryption key for open-voice-pe
  api_encryption_key_open_voice_pe: "your-api-encryption-key"
  ```
- Generate a unique API encryption key using the ESPHome Dashboard or by running `esphome wizard`.

### 3. Copy Configuration
- Copy `open-voice-pe.yaml` to your ESPHome config directory.
- **Important**: The `name` in the `SUBSTITUTIONS` section (`open-voice-pe`) must match the file name (`open-voice-pe.yaml`). If you copy the contents of this file into a differently named file (e.g., `home-assistant-voice-09065b.yaml`), you must update the `name` to match the file name (e.g., `name: home-assistant-voice-09065b`), or rename the file to `open-voice-pe.yaml`. Using a mismatched `name` and file name can cause conflicts in ESPHome and Home Assistant.
- If you have multiple Open Voice PE devices, update the `name` to a unique value (e.g., `open-voice-pe-living-room`) and rename the file accordingly (e.g., `open-voice-pe-living-room.yaml`).

### 4. Configure Media Player for TTS Playback
- The `SELECT` section in `open-voice-pe.yaml` contains a list of media players for TTS playback. Edit the `options` list under `tts_target_media_player_select` to match the media players in your Home Assistant setup:
  ```yaml
  select:
    - platform: template
      name: "TTS target media player"
      id: tts_target_media_player_select
      options:
        - "media_player.living_room"
        - "media_player.kitchen"
        - "media_player.bedroom"
      # ... other properties
  ```
- Replace `media_player.living_room`, `media_player.kitchen`, etc., with your own media player entity IDs (e.g., `media_player.my_speaker`).

### 5. Compile and Upload
- Compile the firmware:
  ```
  esphome compile open-voice-pe.yaml
  ```
- Upload the firmware to your ESP32-S3:
  ```
  esphome upload open-voice-pe.yaml
  ```
- Note: If you renamed the file (e.g., to `open-voice-pe-living-room.yaml`), use that file name in the commands.

### 6. Integrate with Home Assistant
- Add the device to Home Assistant via the ESPHome integration:
  - Go to **Settings > Devices & Services > Integrations** in Home Assistant.
  - Click **Add Integration**, select **ESPHome**, and enter the device’s mDNS address (e.g., `open-voice-pe.local`).
- Configure the Wyoming satellite in Home Assistant to enable voice assistant functionality:
  - Follow the [Wyoming Satellite Setup Guide](https://github.com/rhasspy/wyoming-satellite) to set up STT and TTS pipelines.
  - Configure your preferred wake word in the Wyoming satellite settings (this firmware supports `openWakeWord` for local wake word detection).
- **Additional Configuration**: To enable TTS playback on remote media players:
  1. In Home Assistant, go to **Settings > Devices & Services > Integrations**.
  2. Find your ESPHome integration and click on the Open Voice PE device (e.g., `open-voice-pe`).
  3. In the device settings, locate the option labeled **"Allow the device to perform Home Assistant actions"**.
  4. Check this box and save the changes.
     - **Note**: If this option is not enabled, the `media_player.play_media` service call will fail, and TTS responses will not play on remote media players.

### 7. Test the Custom Features
- **Wake Word Detection with openWakeWord**:
  - Ensure your wake word is configured in the Wyoming satellite.
  - Test wake word detection by saying your wake word. Check the ESPHome logs (`esphome logs open-voice-pe.yaml`) for `openWakeWord` activity.
- **TTS Playback on Remote Media Player**:
  - Select a remote media player from the `TTS target media player` dropdown in Home Assistant.
  - Issue a voice command and verify that the response plays on the selected media player.

## Troubleshooting
- **Wake Word Not Detected**:
  - Check the ESPHome logs for errors related to `openWakeWord`.
  - Ensure the Wyoming satellite is configured with a wake word supported by `openWakeWord`.
  - Adjust the `noise_suppression_level` or `auto_gain` in the `VOICE_ASSISTANT` section if background noise is an issue.
- **TTS Not Playing on Remote Media Player**:
  - Verify that the selected media player in the `TTS target media player` dropdown exists in Home Assistant.
  - Check the logs for errors related to `homeassistant.service` calls in the `on_tts_end` handler.
  - Ensure the remote media player is online and supports the `media_player.play_media` service.
- **Device Not Connecting to Home Assistant**:
  - Ensure the API encryption key in `secrets.yaml` matches the one used in Home Assistant.
  - Verify Wi-Fi credentials in `secrets.yaml` are correct.
  - Check the mDNS address (`open-voice-pe.local`) resolves correctly on your network.

## Roadmap
The following features are planned for future releases:
- **Follow-Up Responses**: Enable the device to listen for additional commands after a response without requiring the wake word again.
- **Wake Word Arbitration**: Improve wake word detection reliability by arbitrating between multiple wake word engines or instances.

## License
This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.
