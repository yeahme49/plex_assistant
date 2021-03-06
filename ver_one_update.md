# Plex Assistant Version 1.0.0
### Version 1.0.0 requires Home Assistant 2021.2.0 or higher.

**This release has many new features, improvements, and breaking changes.**

# Breaking Changes

The automation, intent, intent script, and sensor are no longer required to be setup by the user.<br>The component handles all of it automatically and there is no need for a sensor anymore.<br><br>
This means you need to remove those items from HA.<br>If you don't remove them it may cause issues and duplicate commands.<br><br>
Configuration is now handled in the UI so the old config method is no longer needed as well.

### Remove these things if they exist:

<details>
  <summary><b>Plex Assistant Config</b></summary>

Remove your entire plex assistant config, including `plex_assistant:`

```yaml
plex_assistant:
  url: 'http://192.168.1.3:32400'
  token: 'tH1s1Sy0uRT0k3n'
  default_cast: 'Downstairs TV'
  language: 'en'
  tts_errors: true
  aliases:
    Downstairs TV: TV0565124
    Upstairs TV: Samsung_66585
```
  
</details>

<details>
  <summary><b>Sensor</b></summary>

Remove the `plex_assistant` sensor.

```yaml
sensor: # Keep this line if other sensors are listed below it.
- platform: plex_assistant
```
  
</details>

<details>
  <summary><b>IFTTT Automation</b></summary>
  
```yaml
alias: Plex Assistant Automation
trigger:
- platform: event
  event_type: ifttt_webhook_received
  event_data:
    action: call_service
condition:
  condition: template
  value_template: "{{ trigger.event.data.service == 'plex_assistant.command' }}"
action:
- service: "{{ trigger.event.data.service }}"
  data:
    command: "{{ trigger.event.data.command }}"
```
  
</details>

<details>
  <summary><b>Intent</b></summary>

Keep `conversation:` and keep `intents:` if you have other intents.

```yaml
conversation: #### Keep this line
  intents:    #### and this one if you have other intents.
    Plex:
     - "Tell Plex to {command}"
     - "{command} with Plex"
```

</details>

<details>
  <summary><b>Intent Script</b></summary>
  
```yaml
intent_script: # Keep this line if you have other intent scripts below
  Plex:
    speech:
      text: "Command sent to Plex."
    action:
      - service: plex_assistant.command
        data:
          command: "{{command}}"
```
  
</details>


# Configuration

**You need to have Home Assistant's [Plex integration](https://www.home-assistant.io/integrations/plex/) setup in order to use Plex Assistant.**<br><br>
This will help with configuration and support, will allow for more improvements as Plex Assistant progresses, and requires less processing than the old method.

Configuration is now handled in the UI. [Find the configuration docs in the updated readme.](https://github.com/maykar/plex_assistant#configuration)


# New Features

* Configuration through the UI
* HA's `media_player` entities are now used for devices
* The entity's friendly name is now used for it's device name
* Media automatically resumes where you left off
* Continuous play when playing a show, ondeck, latest, etc.
* New commands "skip forward" and "skip back" to go to next or previous media item
* New random command to play a random item or selected items in a random order
* New "Start Script" feature to start Plex Clients that aren't currently open
* Replace any word or phrase in command with a replacement of your preference
* Jump forward/back works for all devices and amount (in seconds) is configurable
* Component listens for IFTTT and DialogFlow calls automatically (no more intents, scripts, or automations needed)
* Roman numerals are now handled
* DialogFlow instructions improved with uploadable template action
* Remote server support

# Thank you

For patiently answering my endless questions and putting up with my constant pestering/suggestions, I'd like to give a huge "Thank you!" to two individuals for their help in getting Plex Assistant to this point.

[@jjlawren](https://github.com/jjlawren) for their guidance and their work on Python-PlexAPI and HA's Plex integration. The ability to use media player entities, continuous playing of media items, and more wouldn't be possible without them.

[@ludeeus](https://github.com/ludeeus) for HACS, help with config flow, guidance on all things python, and always being willing to help.
