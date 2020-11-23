# Heat Network MQTT Protocol with Node-RED
Heat network protocol based on standardised network control and reporting methodology.

The devices folder contains information for each type of device.

This project is based upon http://www.heatweb.co.uk/w/index.php?title=Heat_Network_Protocol


## Protocol Rules

* camelCase naming.
* 5 levels of MQTT topic for a network (networkId / publisherId / deviceId / dataType / key).
* The standard data types are "dat" (readings), "stat" (statistics, calculations), "alarm", "system", "settings", "json", "set" & "cmd" (command). The list can be expanded, however data should be assigned a standard type if one fits.
* Each device will be assigned the following standard sub-topics to represent its type and state:<br>
  +/+/+/stat/state = on, off, ok, warning, or alarm<br>
  +/+/+/system/deviceType = device classes, e.g. "pump"

## How to use this protocol

This project is organised to enable systems to automatically:

* Provide descriptions of topics
* Provide units of measurement
* Setup default values
* Setup calculated values
* Implement manufacturer specific topics using the *system/deviceVersion* topic
* Link to web based documentation such as manufacturers installation instructions

This project folder on GitHub contains JSON data files that describe the protocol, devices, and other useful data.  These files can be imported directly info software such as Node-RED. Example Node-RED flows are provided under the examples folder to get you up and running handling live data in a matter of minutes. 

If you already have your MQTT heat network up and running, the example flows include real-time dashboards to accesss data on the network. These can be used for remote monitoring, commissioning, or to enable custom alarm routes to be programmed.  You will need server to know the server details and your personal credentials for access.

![Image of Yaktocat](https://www.heatweb.co.uk/w/images/6/6b/Dashd1.jpg)

This protocol can also be used in conjunction with an array of apps for mobile devices, providing the topics required to access readings, or to send commands. Server details and personal credentials for access will need to be setup and provided by the network operator. Topics and Node-RED flows are provided within this protocol to enable pairing between users and devices using tapping functions (hot tap signals).

To access data on an MQTT server you will need:

* server address
* server port number
* user name
* user password
* list of topics you have permission for (this protocol helps there) 

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
