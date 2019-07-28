# Heat Network MQTT Protocol
Heat network protocol based on standardised network control and reporting methodology.

The devices folder contains information for each type of device.

## Protocol Rules

* camelCase naming.
* 5 levels of MQTT topic for a network (networkId / publisherId / deviceId / dataType / key).
* Underscore [_] character not to be used in naming. It is reserved for replacing [/] character in non MQTT systems.
* Each device will be assigned the following standard sub-topics to represent its state:<br>
  +/+/+/stat/state = on, off, ok, warning, or alarm<br>
  +/+/+/stat/alarms = the total number of current alarms
* Each device will typically be assigned a setting to represent its type (from this protocol):<br>
  +/+/+/settings/deviceType
* Each device can optionally be assigned a setting to represent its type version:<br>
  +/+/+/settings/deviceVersion

## Device Types

* boilerGroup
* boiler
* hiu
* block
* substation
