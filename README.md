# Heat Network MQTT Protocol

MQTT is a publish/subscribe protocol, where a central MQTT server acts as a post office where data is published using unique topics.  Data is immediately recieved by subscribers, which will typically include a database ingest service that listens to data on the MQTT network, processes it and files it in a database.  Each MQTT topic is assigned a Quality Of Service tag, that ranges from fire and forget, to confirm receipt of data by holding it on the server until all known subscribers have confirmed receipt, even after a temporary disconnection of subscribers or publishers from the network.

MQTT is the standard IoT open-protocol, and is available in the [BMS] control systems running equipment within heat networks. It is one of the most robust and widely deployed protocols in use, and sits behind many well known messaging platforms.  MQTT runs on almost any device or platform, including browsers and many mobile Apps, requiring very little overhead, with services capable of handling millions of communications per second. 

This heat network protocol is used to describe data points within a system such as a heat network, in a way that makes best use of the MQTT protocol and the benefits provided by its use of topics.  

While this protocol is targetted at MQTT, the structure of topics also defines the organisation of data within a database, or directory structure.

The basic concept is to describe a data point using a structured label.  The standard uses a standard 6 levels in the label, seperated by a '/' character. The levels are as follows:

* A Schema ID, that uniquely identifies a database partition (schema).
* A Network ID, that uniquely identifies a [heat] network (or site) within a schema.
* An Element ID, that uniquely identifies an element, node or entity within a network.  This may be, for examle, an energy centre, a substation, a distribution network, or a property (consumer connection).
* A Device ID, that uniquely identifies a device within an element. Devices can be physical entities, such as HIUs, MVHR systems, DHW systems, hydraulic circuits, boilers or pumps.
* A Data Type, that describes the type of data. Data types are standardised, but allow for custom layers of data. 
* A Data Name, that describes the data point. Names are standardised, however a degree of mapping may be expected when working with data sources that are not labelled to the chosen standards. 

E.g.:  heatweb/myHeatNetwork/energycentre/boiler1/sensor/tF = 73.5 

One of the main benefits provided by structuring labels in MQTT is it allows data to be filtered using wildcards, and for security rules (both read and write) to be applied using a wildcarded topic. 

E.g.:  heatweb/myHeatNetwork/energycentre/+/sensor/#  (all sensor data points from the energy centre)

This allows, for example, a users acces to be limited to heat meter data on a particular heat network, and for data from different network operators to be transported in parallel over the same architecture.  MQTT servers can be linked using bridges, allowing independently operated services to share data in near real-time (less than 1 second latency) according to topic based access rules. 

A standard dictionary is provided in this project to label data and provide compatibility and interoperability between networks.  This has been updated to cover the full range of HNTAS data points foe heat network elements.

This project is based upon http://www.heatweb.co.uk/w/index.php?title=Heat_Network_Protocol, underpinning the communications for Heatweb's SBRI UK Innovate funded project to establist an open-source standard for energy (heat networks and HVAC) systems. The protocol has been in use since 2015.


## Protocol Rules

* Unique topic per data point.
* A minimum of 6 levels of MQTT topic for each data point (schemaId / networkId / nodeId / deviceId / dataType / key).
* Additional topic levels are ignored by the protocol, but can be used (as prefix levels) for additional levels of hierarchy for access control. 
* The standard data types include "system", "design", "setpoint", "json", "set" (change a setting) & "cmd" (command). The list can be expanded, however data should be assigned a standard type if one fits.
* BMS data types include "sensor" (analogue in), "digin" (digital in), "setpoint", and "driver" (output).
* Meter data types include "meter" (combined), "hmeter" (heat), "cmeter" (cooling), "emeter" (electrcity) and "gmeter" (gas).
* Each device will typically be assigned the following standard sub-topics to represent its type, name and state:<br>
  +/+/+/+/system/name = The name of the device<br>
  +/+/+/+/system/deviceType = device classes, e.g. "pump", "heatwebNode" or "ASHP" (Air Source Heat Pump)


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

* A globally unique top level identifier for the system - a network id 
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
