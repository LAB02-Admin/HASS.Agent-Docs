# Actionable Notifications

**Important: this feature requires HASS.Agent 2022.13.0-beta3 and the beta notification integration. Read the release logs for more info**

Thanks [@fillefilip8](https://github.com/fillefilip8) for implementing this!

### Introduction

HASS.Agent's actionable notifications work the same way as the companion apps do. Read HA's docs for more info, [found here](https://companion.home-assistant.io/docs/notifications/actionable-notifications/#building-actionable-notifications).

It basically works as follows:

* You send a notification, combined with a few actions
* Each of those actions have a unique name, eg. `yes` or `no`
* It's even better to give them more descriptive names, eg. `turn_garden_light_on` or `activate_alarm`
* Don't worry: that's not what you see in the notification, you can set anything you want there
* When the notification shows, it'll have buttons containing the actions
* As soon as you click one of the buttons, the action name will get triggered
* Every automation that has that trigger will activate

If that still sounds a bit complex, just follow the examples below and it'll start making sense (hopefully).

### Preparation

You need a notifier entity ready, check the [examples](https://hassagent.readthedocs.io/en/latest/notifications/notification-usage-and-examples/) for info on how to do that.

### Actionable Notification

Let's prepare the notification. For our test, we'll create a script to handle that. In the GUI editor, configure it like this:

