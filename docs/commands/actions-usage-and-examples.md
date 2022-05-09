# Actions Usage and Examples

Normal commands contain a predefined value, which you can execute by toggling the command in Home Assistant. The limitation here is that you can't pass variables, say if you'd want to set the volume to a specific volume. Your only options would be volume up, volume down or make 100 buttons for every step..

By using **actions**, you can pass additional variables when launching your command through an automation or script. The name action comes from the fact that HASS.Agent uses HA's built-in [action MQTT topic](https://www.home-assistant.io/docs/mqtt/discovery/).

**Not all commands support actions**. For instance, there's no point passing an action to a `Shutdown` command (come to think of it, there is: the amount of seconds to wait before shutting down, but you get the idea).

At the time of writing, these commands support actions: `Custom`, `Custom Executor`, `Launch Url` and `Powershell`

You can easily see the current supported commands through the interface, by looking at this column:

![image](https://user-images.githubusercontent.com/81011038/161010961-b8f4204b-1751-45a0-9304-ded508227b05.png)

To use actions, you need to create an automation or script that publishes to the command's action topic. HASS.Agent will then run the command, passing your variables to it.

This is an example automation, which uses the state of an input_boolean to trigger (I use that for testing). It will launch [BowPad](https://github.com/stefankueng/BowPad), my favorite notepad replacement. 

Notice the escaped quotes: `\"` and escaped backslashes: `\\` - it's your responsibility to get those right!

```yaml
- alias: tests - custom command with action
  description: "Test custom command with an action"
  initial_state: true
  mode: single
  trigger:
    platform: state
    entity_id: input_boolean.test_custom_command_with_action
    to: "on"
  action:
    - service: mqtt.publish
      data:
        topic: "homeassistant/switch/test-vm/test-vm_custom/action"
        payload: "\"C:\\Program Files\\BowPad\\BowPad.exe\""
    - delay:
        seconds: 2
    - service: input_boolean.turn_off
      data:
        entity_id: input_boolean.test_custom_command_with_action
```

This is the corresponding HASS.Agent configuration:

![image](https://user-images.githubusercontent.com/81011038/161011893-21ff75d6-ec5b-4d54-9f92-d924c1f6f7b7.png)

As you can see, I've left the command part empty, since we're sending that from Home Assistant. 

Another option is to add your command to HASS.Agent, and then send additional parameters from Home Assistant. To stick with the above example, in this case I would configure BowPad here:

![image](https://user-images.githubusercontent.com/81011038/161015700-70d79c36-f07b-40ed-a60b-fefbb0f121ff.png)

And then use the automation to launch a text file in BowPad:

```yaml
- alias: tests - custom command with action
  description: "Test custom command with an action"
  initial_state: true
  mode: single
  trigger:
    platform: state
    entity_id: input_boolean.test_custom_command_with_action
    to: "on"
  action:
    - service: mqtt.publish
      data:
        topic: "homeassistant/switch/test-vm/test-vm_custom/action"
        payload: "\"D:\\Staging\\TestFiles\\test_file.txt\""
    - delay:
        seconds: 2
    - service: input_boolean.turn_off
      data:
        entity_id: input_boolean.test_custom_command_with_action
```

To help you find the correct topic, there's a `show mqtt action topic` button when you've selected a supported command:

![image](https://user-images.githubusercontent.com/81011038/161012746-870fe99f-f1bb-4b18-95c5-97088bbd4f38.png)

This will show you the exact topic, and the option to copy the value to your clipboard:

![image](https://user-images.githubusercontent.com/81011038/161013081-9aeeda25-ce30-436b-9740-730cdefde5b9.png)

**Make sure you've entered the correct type and name of your command before copying this!**

---

If you've built a great command action, please share! You can mail it to lab02research@outlook.com or tell me on [Discord](https://discord.gg/nMvqzwrVBU), and I'll add it to this page for everlasting fame 😎 