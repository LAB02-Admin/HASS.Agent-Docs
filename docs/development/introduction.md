# Introduction

These pages are meant as a quick introduction into how HASS.Agent is developed, in case you want to help out (or use code for your own projects).

Use the subpages on the left to learn more about specific parts.

**Do not use this guide if you just want to manually build and use HASS.Agent. Please see [these instructions](https://hassagent.readthedocs.io/en/latest/installation/#3-build-from-scratch) on how to do that.**

----

### Overview

All development is done in the [HASS.Agent.Staging](https://github.com/LAB02-Research/HASS.Agent.Staging) repo. It contains a solution with all three sub-projects:

| Project | Description |
|---|---|
| HASS.Agent | Main client, containing the UI, runs in userspace, by default without elevation |
| HASS.Agent.Satellite.Service | Windows client, runs under SYSTEM account |
| HASS.Agent.Shared | Library, published as a nuget, contains all commands, sensors, shared functions and enums |

Its purpose is to more easily develop beta releases, as the shared library will be instantly compiled and referenced.

### General Notes

1) Make sure any regular instance of HASS.Agent is shut down. If you're just planning to tinker with HASS.Agent, you can leave the regular service active, otherwise turn it off as well as the named pipe will be in use. Start the beta service with elevated privileges.

2) It's best to have `enable extended logging` enabled, which will also reflect on the satellite service (as long as it's started in console mode instead of service mode). But that'll also generate false positives, so primarily focus on the issue at hand.

3) A lot has been done for localization and screenreader support. Don't bother about either during beta development; just use regular strings and mess about the UI as you like. The dev will implement it for the release candidate and make sure it's done in a uniform matter.

4) I'm a big fan of the [KISS principle](https://en.wikipedia.org/wiki/KISS_principle), especially since it's a project I maintain in my freetime. If I don't *need* a design pattern, I don't use it. By keeping it simple, I can easily tinker with whatever I want, as fast as possible. However, if you do want to build something complex because that's what you prefer or see fit, knock yourself out, just give me a heads up.
