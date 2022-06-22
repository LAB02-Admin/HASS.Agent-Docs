# ActiveWindowSensor - Multiple Processes

This example allows you to monitor multiple processes, by using an ActiveWindowSensor. It was created by @DivanX10 ([#87](https://github.com/LAB02-Research/HASS.Agent/issues/87)).

---

I have created sensors that work by keywords. We enter the keywords into the `input_text` helpers. If there are matching words in the ActiveWindowSensor, it will be on, if there are none, it will be off. Keywords are indicated by | . This will allow you not to have to create a bunch of sensors with different processes in HASS.Agent itself, and you can flexibly automate brightness adjustment, ambilight, color effects on coolers:

![image](https://user-images.githubusercontent.com/64090632/174195048-fa9b5c5f-961e-4ba4-95b9-714f2f05457b.png)

Create `input_text` helpers, in which you can define which game, video player or browser is running:

```
input_text:
  pc_livingroom_games_keywords:
    name: 'Living room: Computer. Games. Keywords'
    min: 0
    max: 100000
    icon: mdi:google-controller

  pc_livingroom_videoplayer_keywords:
    name: 'Living room: Computer. Video player. Keywords'
    min: 0
    max: 100000
    icon: mdi:video

  pc_livingroom_browser_keywords:
    name: 'Living room: Computer. Browser. Keywords'
    min: 0
    max: 100000
    icon: mdi:web
```


Create the sensors that will show the category that we are using at the moment:

```
#Sensor Living Room: Computer. Game
sensor:
  - platform: template
    sensors:
      hassagent_pc_livingroom_running_game:
        friendly_name: "Living room: Computer. Game"
        icon_template: mdi:google-controller
        value_template: >
            {% set activewindow = states("sensor.pc_livingroom_activewindow") %}
            {% set games = states("input_text.pc_livingroom_games_keywords") %}
            {% if activewindow | string is search(find=games) %} on
            {% else %} off
            {% endif %}


#Sensor Living Room: Computer. Video Player
  - platform: template
    sensors:
      hassagent_pc_livingroom_running_videoplayer:
        friendly_name: "Living room: Computer. Video Player"
        icon_template: mdi:video
        value_template: >
            {% set activewindow = states("sensor.pc_livingroom_activewindow") %}
            {% set videoplayer = states("input_text.pc_livingroom_videoplayer_keywords") %}
            {% if activewindow | string is search(find=videoplayer) %} on
            {% else %} off
            {% endif %}


#Sensor Living Room: Computer. Browser
  - platform: template
    sensors:
      hassagent_pc_livingroom_running_browser:
        friendly_name: "Living room: Computer. Browser"
        icon_template: mdi:web
        value_template: >
            {% set activewindow = states("sensor.pc_livingroom_activewindow") %}
            {% set browser = states("input_text.pc_livingroom_browser_keywords") %}
            {% if activewindow | string is search(find=browser) %} on
            {% else %} off
            {% endif %}
```
