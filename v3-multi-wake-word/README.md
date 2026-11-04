# Open Voice PE v3: several wake words

> **Archived (July 2025).** Kept for reference; not maintained. See the [main README](../README.md) for the current version.

v2, made into one file for every room of the house:

- **Several custom wake words.** One microWakeWord model per assistant, so each person in the household can call their own assistant by name. The file lists three placeholders (`wake_word_1` to `wake_word_3`) next to the stock Okay Nabu model.
- **One setting per room.** The media player for replies and system sounds is a single substitution, `media_player_entity`, at the top of the file.

Everything else is as in [v2](../v2-microwakeword/): a full copy of stock firmware 25.4, with the Voice PE's own speaker silenced.

## Using it

1. Put each model's JSON and `.tflite` files in `/config/esphome/`, and rename the `wake_word_*` entries to match
2. Set `media_player_entity` for the room
3. Allow the device to perform Home Assistant actions (see the [main README](../README.md#common-setup))

It was built with the ESPHome versions of mid-2025, and may not build with current versions.
