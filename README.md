# Open Voice PE

Custom firmware and setups for the [Home Assistant Voice Preview Edition](https://github.com/esphome/home-assistant-voice-pe), as they have evolved in one home with nine Voice PEs. Each version is kept in its own folder, so you can see how it got here and reuse whichever piece fits your setup.

**Current version: [v4, Smart Listening](v4-smart-listening/).** No wake word: you talk to the assistant naturally, and Home Assistant decides which speech was meant for it.

## Versions

| Version | When | Wake word | Replies play on | What changed |
| --- | --- | --- | --- | --- |
| [v1, openWakeWord](v1-openwakeword/) | March 2025 | openWakeWord, in Home Assistant | A media player chosen per device | First custom firmware: wake word detection moved off the device, replies on a room speaker |
| [v2, microWakeWord](v2-microwakeword/) | April 2025 | A custom microWakeWord model, on the device | The room's speaker, with the Voice PE's own speaker silenced | Back to on-device wake words, with a model trained for the assistant's name; every sound (wake chime, timers, replies) on the room speaker |
| [v3, stock package](v3-stock-package/) | Until November 2026 | Several custom models, one per assistant, added to the stock ones | The room's Sonos, through a Home Assistant script that resumes what was playing | A short device file on top of the official firmware package instead of a full copy, so stock updates arrive without merging |
| [v4, Smart Listening](v4-smart-listening/) | November 2026 | None (wake word models only give a hint) | The room's Sonos, as in v3 | The device listens for speech and Home Assistant judges each transcript: no ghost wakes, no chime, natural follow-ups |

v1 and v2 are full copies of the firmware from 2025, and may not build with current ESPHome versions. v3 and v4 use the official firmware as a package, pinned to stock release 26.9.0.

## Which one to use

- **To stop ghost wakes, or to talk without a wake word**: [v4](v4-smart-listening/), with the [Smart Listening](https://github.com/mike-nott/smart-listening) integration
- **To keep wake words, with replies on a Sonos**: [v3](v3-stock-package/)
- **For reference**: v1 and v2 show earlier approaches. They are not maintained

## Common setup

All versions are ESPHome configurations for a Voice PE that has been adopted in the ESPHome Device Builder add-on. Each device needs a `secrets.yaml` in the ESPHome config folder with:

```yaml
wifi_ssid: "your-wifi-ssid"
wifi_password: "your-wifi-password"
api_encryption_key: "your-api-encryption-key"
```

Custom wake word models (v2 and v3) are microWakeWord models you train yourself: put each model's JSON file and its `.tflite` file in `/config/esphome/`.

For replies on another speaker (all versions), allow the device to perform Home Assistant actions: **Settings → Devices & services → ESPHome → your Voice PE → Configure → Allow the device to perform Home Assistant actions**.

## Related projects

- [Smart Listening](https://github.com/mike-nott/smart-listening): the Home Assistant integration behind v4
- [MCP Assist](https://github.com/mike-nott/mcp-assist): a conversation agent for Home Assistant
- [Smart Announcements](https://github.com/mike-nott/smart-announcements): announcements routed to the room each person is in

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
