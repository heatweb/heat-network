# Heat Network MQTT Protocol
A heat network protocol based on standardised network control points and reporting methodology.

At its core this is a dictionary to describe data using standardised identifiers, to provide compatibility between systems and easy sharing of data.

The devices folder contains information for each type of device.

This project is based upon http://www.heatweb.co.uk/w/index.php?title=Heat_Network_Protocol, and forma part of Heatweb's SBRI UK Innovate funded project to establist an open-source standard for energy (heat networks and HVAC) systems.


## Protocol Rules

* Unique topic per data point.
* 5 levels of MQTT topic for each data point (networkId / nodeId / deviceId / dataType / key).
* The standard data types are "dat" (default), "stat", "alarm", "system", "settings", "json", "set" (change a setting) & "cmd" (command). The list can be expanded, however data should be assigned a standard type if one fits.
* BMS data types include "sensor" (analogue in), "digin" (digital in) and "driver" (output).
* Meter data types include "meter" (heat), "cmeter" (cooling), "emeter" (electrcity) and "gmeter" (gas).
* Each device will typically be assigned the following standard sub-topics to represent its type, name and state:<br>
  +/+/+/stat/state = on, off, ok, standby, warning, or alarm<br>
  +/+/+/system/name = The name of the device<br>
  +/+/+/system/deviceType = device classes, e.g. "pump", "heatwebNode" or "airSource heatPump"
* The networkId can contain dashes to create a network hirearchy, as would normally be described by further topic levels. <br>
  E.g. myNetwork-block1/riser1node/riser1node/system/deviceType
* The publisherId is normally the device itself, the communications node the device is connected to, or is part of, but may be a different device publishing relevant data or commands to the device.


## Examples

 network_id / node_id / device_id / data_group / key = value<br>
 myHeatNetwork/plantroomController1/boiler1/sensor/tOut = 65.6   (boiler output temperature)<br>
 myHeatNetwork/plantroomController1/boiler1/alarm/tOut = ok <br>
 myHeatNetwork/plantroomController1/boiler1/dat/run = 1   <br>
 myHeatNetwork/ab34c23Z/hiu0198984733686/dat/fC = 3.2   {HIU, tap flow rate)<br>
 myHeatNetwork/ab34c23Z/hiu0198984733686/settings/prepayRun = 1   {HIU, prepay isolation)<br>
 myHeatNetwork/ab34c23Z/hiu0198984733686/set/prepayRun = 0   {HIU, command to isolate on prepay)<br>
 myHeatNetwork/ab34c23Z/hiu0198984733686/hm/kwh24h = 13.3   {HIU heat meter, energy used over past 24 hours)<br>
 myHeatNetwork/ab34c23Z/blockB2/hm/kwh24h = 1204.3   {bulk heat meter, energy used over past 24 hours)

## Overview

MQTT has been selected as a modern, light-weight, open protocol for network use, allowing for user management, assess rules, and wildcards. It is the standard IoT protocol.

All data requires describing:

* A top level identifier for the system - a network id 
* Where (which controller or location) the data comes from - a node id
* What device it refers to - a device id
* The type of data 
* A name - a data key
* A value

A 5 level MQTT topic allows all these basic requirements to be described in a way that can be filtered using wildcards for managing subscribtions.

The protocol data can easily be stored in software via a nested JSON object, or in a file system via a directory structure.

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
