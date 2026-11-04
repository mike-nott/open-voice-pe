# Open Voice PE v2: microWakeWord

> **Archived (April 2025).** Kept for reference; not maintained. See the [main README](../README.md) for the current version.

A full copy of the official Voice PE firmware (stock release 25.4) with two changes:

- **A custom wake word.** A microWakeWord model trained for the assistant's name runs on the device, alongside the stock models. Wake word detection is back on the Voice PE, instead of streaming audio to openWakeWord in Home Assistant as v1 did.
- **Every sound on the room speaker.** The wake chime, the timer alarm and the assistant's replies play on a Home Assistant media player (`media_player.living_room` in the file). The Voice PE's own speaker is silenced by moving its audio output to an unused pin.

## Using it

1. Put your model's `my_wake_word.json` and `.tflite` files in `/config/esphome/`
2. Replace `media_player.living_room` with your speaker (it appears in several places)
3. Copy a `wake.mp3` chime to `/config/www/` (it is played from `/local/wake.mp3`)
4. Allow the device to perform Home Assistant actions (see the [main README](../README.md#common-setup))

It was built with ESPHome 2025.3 and 2025.4, and may not build with current versions.
