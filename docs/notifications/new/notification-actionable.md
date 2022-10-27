# Actionable Notifications

**Important: this feature requires at least [HASS.Agent 2022.13.0](https://github.com/LAB02-Research/HASS.Agent/releases/tag/2022.13.0).**

### Introduction

HASS.Agent's actionable notifications work the same way as the companion apps do. Read HA's docs for more info, [found here](https://companion.home-assistant.io/docs/notifications/actionable-notifications/#building-actionable-notifications).

It basically works as follows:

* You send a notification, combined with a few actions
* Each of those actions have a unique name, eg. `yes` or `no`
* It's even better to give them more descriptive names, eg. `turn_garden_light_on` or `activate_alarm`
* Don't worry: that's not what you see in the notification, you can set any title you want
* When the notification shows, it'll have buttons containing the actions
* As soon as you click one of the buttons, the action name will get triggered
* Every automation that has that trigger will activate

If that still sounds a bit complex, just follow the examples below and it'll start making sense (hopefully).

### Preparation

You need a notifier entity ready. If you use the legacy integration, check the [examples](https://hassagent.readthedocs.io/en/latest/notifications/notification-usage-and-examples/) for info on how to do that. Otherwise, you should have it autodetected after installing the [new integration](https://github.com/LAB02-Research/HASS.Agent-Integration).

### Actionable Notification

Let's prepare the notification. For our test, we'll create a script to handle that. In the GUI editor, configure it like this:

![image](https://user-images.githubusercontent.com/81011038/190641858-7c773f00-55cd-447b-acf6-de3938327200.png)

Or if you're into yaml:

```yaml
alias: Notification - Actionable Test
sequence:
  - service: notify.hass_agent_staging
    data:
      title: Test
      message: This is an actionable test message.
      data:
        actions:
          - action: "yes"
            title: "Yes"
          - action: "no"
            title: "No"
mode: single
icon: mdi:bell
```

We added two actions: `yes` and `no`, which will show as `Yes` and `No` in the notification. The `action` value should be descriptive, the `title` value is what the user gets to see.

### Action Automations

HA needs to know what to do with the triggered actions, so let's make automations to handle that.

Create a new automation in the GUI for our `yes` action, and configure the following trigger:

![image](https://user-images.githubusercontent.com/81011038/190642484-57fd2826-1d85-41b2-847b-4107403a1d32.png)

The `device_name` value helps us bind it to a specific device. Make sure you enter `hass_agent_notifications` as the `Event type`.

Then the action, it just sends another notification:

![image](https://user-images.githubusercontent.com/81011038/190642903-4e5107eb-5778-4582-a1e6-96ef1398dd32.png)

Save your automation.

Now create another automation for our `no` action, and configure the following trigger:

![image](https://user-images.githubusercontent.com/81011038/190643121-4facff6e-44b0-49b2-9e4c-a8b85420d685.png)

And the action:

![image](https://user-images.githubusercontent.com/81011038/190643282-407637b6-2362-4745-b6e5-2c8641bd914b.png)

Save your automation.

### Testing

Restart HA (or reload scripts & automations). When you run the test script, a notification like this should show on your PC:

![image](https://user-images.githubusercontent.com/81011038/190643738-724dac45-4d03-4a19-a0e6-3a59b5de0aad.png)

Click `Yes`, and another popup should show:

![image](https://user-images.githubusercontent.com/81011038/190643932-747b90ad-cec9-4ef0-828f-b4a324b99bf9.png)

Done, you've now got actionable notifications!

---

Thanks [@fillefilip8](https://github.com/fillefilip8) for implementing this!
