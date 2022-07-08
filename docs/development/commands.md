# Commands

### General

Commands can be found in the `HASS.Agent.Shared` project, under `HomeAssistant\Commands`. They're derived from `AbstractCommand` (found under `Models\HomeAssistant`), which itself is derived from `AbstractDiscoverable`.

There are a few 'base' commands, currently:

* CustomCommand
* InternalCommand
* KeyCommand
* MultipleKeysCommand
* PowershellCommand

Which in turn have derived commands in their respective subfolders. 

Commands have the following functions:

* Constructor: initialize the command which its name, entity type, id (keep default) and configuration
* `GetAutoDiscoveryConfig()`: returns it's discovery model to send to Home Assistant
* `TurnOn()`: execute the command
* `TurnOnWithAction()`: only if the command supports [actions](https://hassagent.readthedocs.io/en/latest/commands/actions-usage-and-examples/), executes the command with the user provided action
* `GetState()`: returns its current state (on/off)
* `TurnOff`: turns the command off

### New Command

When you're adding a new command, there are a few steps to follow. There are quite a few steps - feel free to just add your command to `HASS.Agent.Shared` -> `HomeAssistant\Commands`, and then ask the dev to test and integrate it for you. Creating the command itself is the most helpful part!

* Decide if there's a command which you can use as a base, or create a new type
* Use a clear name, and make it as general as possible (so don't make a **LaunchNotepad** command)
* Open `HASS.Agent.Shared` -> `Enums\CommandType.cs` and add your new command
  * Don't localize your description, that'll be done in the final release by the dev
* Open `HASS.Agent` -> `Commands\CommandsManager.cs` and add your command to `LoadCommandInfo()`
  * Again, don't localize, just use a regular string instead of `Languages_*`
* Follow the steps in the **Storage** section below
* If your command requires special configuration: 
  * Open the code of the form `Forms\Commands\CommandsMod.cs`
  * Modify `LoadCommand()`, `BtnStore_Click()`, `SetType()` as needed
  * Add a `Set{commandname}Gui()` function to create the interface you need
  
### Storage

Commands are stored and retreived by the application using them, so either **HASS.Agent** or **HASS.Agent.Satellite.Service**. It's handled by `StoredCommands.cs` (found under `Settings`).

When stored, a command's type is `ConfiguredCommand`. When loaded in memory, its type is `AbstractCommand`. To accomodate this, the functions `ConvertConfiguredToAbstract()` and `ConvertAbstractToConfigured()` were added. 

If you created a **derived command**, so no new base type, just add your command to `ConvertConfiguredToAbstract()`, using the right derived command type. If you added a new base type, add it to `ConvertAbstractToConfigured()`.

First add storage capabilities for your command to **HASS.Agent** to test. When it's working properly, add it to **HASS.Agent.Satellite.Service**'s `StoredCommands.cs` as well.
