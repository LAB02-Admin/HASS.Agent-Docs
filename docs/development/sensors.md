# Sensors

### General

Sensors can be found in the `HASS.Agent.Shared` project, under `HomeAssistant\Sensors`. They're derived from `AbstractSingleValueSensor` and `AbstractMultiValueSensor` (found under `Models\HomeAssistant`), which themselves are derived from `AbstractDiscoverable`.

There are a few 'base' sensors, currently:

* PerformanceCounterSensor
* WmiQuerySensor

Which in turn have derived sensors in their respective subfolders. 

Most sensors however are **SingleValue** sensors (ie. they return a single value), found under `HomeAssistant\Sensors\SingleValue`. 

Another possibility is a **MultiValue** sensors (ie. they return 2:n values), found under `HomeAssistant\Sensors\MultiValue`. MultiValue sensors consist of sub-sensors, which can have the following types:

* DataTypeBoolSensor
* DataTypeDoubleSensor
* DataTypeIntSensor
* DataTypeStringSensor

Sensors have the following functions:

* Constructor: initialize the sensor which its name, entity type, id (keep default) and configuration
* `GetAutoDiscoveryConfig()`: returns it's discovery model to send to Home Assistant
* `GetState()`: returns its current state
* `UpdateSensorValues()`: updates the values of a multivalue sensor's sensors

### New Sensor

When you're adding a new sensor, there are a few steps to follow. There are quite a few steps - feel free to just add your sensor to `HASS.Agent.Shared` -> `HomeAssistant\Sensors`, and then ask the dev to test and integrate it for you. Creating the sensor itself is the most helpful part!

* Decide if there's a sensor which you can use as a base, or create a new type using `AbstractSingleValueSensor` or `AbstractMultiValueSensor`
* Use a clear name, and make it as exact as possible
* Open `HASS.Agent.Shared` -> `Enums\SensorType.cs` and add your new sensor
  * Don't localize your description, that'll be done in the final release by the dev
* Open `HASS.Agent` -> `Sensors\SensorsManager.cs` and add your sensor to `LoadSensorInfo()`
  * Again, don't localize, just use a regular string instead of `Languages_*`
* Follow the steps in the **Storage** section below
* If your sensor requires special configuration: 
  * Open the code of the form `Forms\Sensors\SensorsMod.cs`
  * Modify `LoadSensor()`, `BtnStore_Click()`, `SetType()` as needed
  * Add a `Set{sensorname}Gui()` function to create the interface you need

### Storage

Sensors are stored and retreived by the application using them, so either **HASS.Agent** or **HASS.Agent.Satellite.Service**. It's handled by `StoredSensors.cs` (found under `Settings`).

When stored, a sensor's type is `ConfiguredSensor`. When loaded in memory, its type is `AbstractSingleValueSensor` or `AbstractMultiValueSensor`. To accomodate this, the functions `ConvertConfiguredToAbstractSingleValue()`, `ConvertConfiguredToAbstractMultiValue()`, `ConvertAbstractSingleValueToConfigured()` and `ConvertAbstractMultiValueToConfigured()` were added. 

If you created a **derived sensor**, so no new base type, just add your sensor to `ConvertConfiguredToAbstractSingleValue()` or `ConvertConfiguredToAbstractMultiValue()`, using the right derived sensor type. If you added a new base type, add it to `ConvertAbstractSingleValueToConfigured()` or `ConvertAbstractMultiValueToConfigured()`.

First add storage capabilities for your sensor to **HASS.Agent** to test. When it's working properly, add it to **HASS.Agent.Satellite.Service**'s `StoredSensors.cs` as well.