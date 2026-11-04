# Open Voice PE v5: Smart Listening

The current version, since November 2026. There is no wake word: you talk to the assistant the way you would talk to someone in the room ("Nova, what time is it?"), with no chime and no pause.

The Voice PE starts a request whenever it hears speech. The [Smart Listening](https://github.com/mike-nott/smart-listening) integration in Home Assistant reads each transcript and decides whether it was meant for the assistant. Only those requests are answered; TV, radio and conversations that mention the assistant are ignored. It also allows natural follow-ups after a reply, without the name.

This folder is the device file for that setup. Everything else (the firmware package, the integration, the classifiers and how to tune them) is documented in the [Smart Listening repository](https://github.com/mike-nott/smart-listening).

## What the device file adds

- The **Smart Listening firmware package**, included straight from the Smart Listening repository after the stock package
- **Replies on the room's Sonos**, through the same Home Assistant script as [v4](../v4-stock-package/), with replies sent as MP3

## Using it

1. Install and set up the [Smart Listening integration](https://github.com/mike-nott/smart-listening#installation), and create an Assist pipeline that uses its conversation agent
2. Create the Sonos script from [v4](../v4-stock-package/voice_assistant_sonos.yaml), or remove the `on_tts_end` and `media_player` sections to keep replies on the Voice PE
3. For each Voice PE, set `name` and `friendly_name`, and install the firmware
4. On each device page, set **Assistant** to the Smart Listening pipeline and turn on the **Smart Listening** switch
5. Allow the device to perform Home Assistant actions (see the [main README](../README.md#common-setup))

## Wake words

Wake words still stop a ringing timer or an announcement, and tell Smart Listening that the name was said when speech detection clips the start of a sentence. The package lowers the stock models' cutoffs while Smart Listening is on. If you add your own models, list them in the package's `smart_listening_cutoffs` script: copy the package into `/config/esphome/packages/` and include it with `!include` instead of `github://`.

The package is included from `@main`. To stay on a fixed version, copy it locally as above.
