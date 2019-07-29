# Heat Network MQTT Protocol with Node-RED
Heat network protocol based on standardised network control and reporting methodology.

The devices folder contains information for each type of device.

## Protocol Rules

* camelCase naming.
* 5 levels of MQTT topic for a network (networkId / publisherId / deviceId / dataType / key).
* The standard data types are "dat" (readings), "stat" (statistics, calculations), "alarm", "system", "settings", "set" & "cmd" (command). The list can be expanded, however data should be assigned a standard type if one fits.
* Underscore "_" character not to be used in naming. It is reserved for replacing "/" character in non MQTT systems.
* Each device will be assigned the following standard sub-topics to represent its state:<br>
  +/+/+/stat/state = on, off, ok, warning, or alarm<br>
  +/+/+/stat/alarms = the total number of current alarms
* The deviceId should typically be a standard device type, as listed below, followed directly by an index number where there are more than one, for example "substation1".
* Each device can optionally be assigned a system setting to represent its type version:<br>
  +/+/+/system/deviceVersion<br>
  This can be a version described in this protocol, or the URL of a suitable JSON file following the format described here. Values will overwrite default values inferred from the deviceId.

## How to use this protocol

This project folder on GitHub contains JSON data files that describe the protocol, devices, and other useful data.  These files can be imported directly info software such as Node-RED. Example Node-RED flows are provided under the examples folder to get you up and running handling live data in a matter of minutes. 

If you already have your MQTT heat network up and running, the example flows include real-time dashboards to accesss data on the network. These can be used for remote monitoring, commissioning, or to enable custom alarm routes to be programmed.  You will need server to know the server details and your personal credentials for access.

This protocol can also be used in conjunction with an array of apps for mobile devices, providing the topics required to access readings, or to send commands. Server details and personal credentials for access will need to be setup and provided by the network operator. 

## Device Types

* boilerGroup
* boiler
* heatpump
* buffer
* hiu
* block
* substation
* pumpGroup
* pump
* pressure
* gas
* network
* plantroom
* onewire
* meter
* panel
* bypass
* filter
