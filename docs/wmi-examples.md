#WMI Examples

Below is a list of WMI query examples, provided by the community.

---

#### Fetch CPU temp from OpenHardwareMonitor

Scope: ```\\.\ROOT\OpenHardwareMonitor```

Query: ```SELECT value FROM Sensor WHERE Name LIKE "%CPU Core%" AND SensorType = "Temperature"```

#### Fetch CPU temp from OpenHardwareMonitor for AMD

Scope: ```\\.\ROOT\OpenHardwareMonitor```

Query: ```SELECT value FROM Sensor WHERE Name LIKE "%CPU Package%" AND SensorType = "Temperature"```

#### Fetch CPU temp from LibreHardwareMonitor

Scope: ```\\.\ROOT\LibreHardwareMonitor```

Query: ```SELECT value FROM Sensor WHERE Name LIKE "%CPU Core%" AND SensorType = "Temperature"```

#### Fetch CPU temp from LibreHardwareMonitor for AMD

Scope: ```\\.\ROOT\LibreHardwareMonitor```

Query: ```SELECT value FROM Sensor WHERE Name LIKE "%CPU Package%" AND SensorType = "Temperature"```

#### Fetch GPU temp from LibreHardwareMonitor

Scope: ```\\.\ROOT\LibreHardwareMonitor```

Query: ```SELECT value FROM Sensor WHERE Name LIKE "%GPU Core%" AND SensorType = "Temperature"```

#### Fetch CPU temp from AIDA64

Scope: ```\\.\ROOT\WMI```

Query: ```SELECT Value FROM AIDA64_SensorValues WHERE ID="TGPU1"```

#### Fetch USB device (dis)connected

Query: ```SELECT DeviceID FROM Win32_USBHub WHERE DeviceID LIKE "USB\\VID_19F7&PID_0003%"```

---

More examples can be found in the [hass-workstation-server documentation](https://github.com/sleevezipper/hass-workstation-service/blob/master/documentation/WMIQuery.md).