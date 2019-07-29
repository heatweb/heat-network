# Heat Network MQTT Protocol
Heat network protocol based on standardised network control and reporting methodology.

The devices folder contains information for each type of device.

## Protocol Rules

* camelCase naming.
* 5 levels of MQTT topic for a network (networkId / publisherId / deviceId / dataType / key).
* The standard data types are "dat" (readings), "stat" (statistics, calculations), "alarm", "system", "settings", "set" & "cmd" (command). The list can be expanded, however data should be assigned a standard type if one fits.
* Underscore [_] character not to be used in naming. It is reserved for replacing [/] character in non MQTT systems.
* Each device will be assigned the following standard sub-topics to represent its state:<br>
  +/+/+/stat/state = on, off, ok, warning, or alarm<br>
  +/+/+/stat/alarms = the total number of current alarms
* The deviceId should typically be a standard device type, as listed below, followed directly by an index number where there are more than one, for example "substation1".
* Each device can optionally be assigned a system setting to represent its type version:<br>
  +/+/+/system/deviceVersion<br>
  This can be a version described in this protocol, or the URL of a suitable JSON file following the format described here. Values will overwrite default values inferred from the deviceId.

## Device Types

* boilerGroup
* boiler
* hiu
* block
* substation
