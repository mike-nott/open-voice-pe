# Open Voice PE v3: stock package

Used until November 2026, and still the version to use if you want to keep wake words.

Earlier versions were full copies of the official firmware, so every stock release had to be merged by hand. v3 is a short device file that includes the official firmware as a package and adds only what is different:

- **Custom wake words** added to the stock models
- **Replies on the room's Sonos.** When a reply is ready, the Voice PE calls a Home Assistant script with the audio URL. The script takes a snapshot of the Sonos, plays the reply, waits for it to finish and restores what was playing, music included
- **The Voice PE's own speaker is muted** while the reply plays, and its volume is restored afterwards

## Files

- [`open-voice-pe.yaml`](open-voice-pe.yaml): the device file. One per Voice PE
- [`voice_assistant_sonos.yaml`](voice_assistant_sonos.yaml): the Home Assistant script that plays replies

## Using it

1. **Helpers**: for each room, create a Toggle helper such as `input_boolean.voice_tts_active_living_room`. The script turns it on while a reply is playing, and the device waits for it before restoring its volume
2. **Script**: create a script from `voice_assistant_sonos.yaml` (Settings → Automations & scenes → Scripts → Add script → Edit in YAML). Add a line per Voice PE to both maps: its ESPHome name, its Sonos and its helper
3. **Device file**: for each Voice PE, set `name`, `friendly_name` and the helper's entity id, and list your wake word models (JSON and `.tflite` in `/config/esphome/`)
4. Install the firmware, and allow the device to perform Home Assistant actions (see the [main README](../README.md#common-setup))

The package is pinned to stock release 26.9.0. Change the version in `packages:` to move to a newer release.

Not using Sonos? Replace the `sonos.snapshot` and `sonos.restore` steps in the script; the rest works with any media player.
