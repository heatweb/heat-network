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

E.g. Typical boiler data points:  
```
heatweb/myHeatNetwork/energycentre/boiler1/system/deviceType = Gas Boiler
heatweb/myHeatNetwork/energycentre/boiler1/system/name = Boiler 1
heatweb/myHeatNetwork/energycentre/boiler1/sensor/tF = 73.5
heatweb/myHeatNetwork/energycentre/boiler1/sensor/tR = 51.9
heatweb/myHeatNetwork/energycentre/boiler1/hmeter/kw = 523.32
heatweb/myHeatNetwork/energycentre/boiler1/hmeter/tF = 73.1
heatweb/myHeatNetwork/energycentre/boiler1/hmeter/tR = 51.3
heatweb/myHeatNetwork/energycentre/boiler1/gmeter/m3Gas = 16353499.1
```

One of the main benefits provided by structuring labels in MQTT is it allows data to be filtered using wildcards, and for security rules (both read and write) to be applied using a wildcarded topic. 

E.g. All sensor data points from the energy centre:  
```
heatweb/myHeatNetwork/energycentre/+/sensor/#  
```

This allows, for example, a users acces to be limited to heat meter data on a particular heat network, and for data from different network operators to be transported in parallel over the same architecture.  MQTT servers can be linked using bridges, allowing independently operated services to share data in near real-time (less than 1 second latency) according to topic based access rules. 

A standard dictionary is provided in this project to label data and provide compatibility and interoperability between networks.  This has been updated to cover the full range of HNTAS data points foe heat network elements.

This project is based upon http://www.heatweb.co.uk/w/index.php?title=Heat_Network_Protocol, underpinning the communications for Heatweb's SBRI UK Innovate funded project to establist an open-source standard for energy (heat networks and HVAC) systems. The protocol has been in use since 2015.


## Protocol Rules

* Unique topic per data point.
* A minimum of 6 levels of MQTT topic for each data point (schemaId / networkId / nodeId / deviceId / dataType / key).
* Additional topic levels are ignored by the protocol, but can be used (as prefix levels) for additional levels of hierarchy for access control. 
* The standard data types include "system", "design", "setpoint", "json", "modbus", "acceptance", "set" (change a setting) & "cmd" (command). The list can be expanded, however data should be assigned a standard type if one fits.
* BMS data types include "sensor" (analogue in), "digin" (digital in), "setpoint", and "driver" (output).
* Meter data types include "meter" (combined), "hmeter" (heat), "cmeter" (cooling), "emeter" (electrcity) and "gmeter" (gas).
* Each device will typically be assigned the following standard points to represent its type, name and state:<br>
   +/+/+/+/system/name = The name of the device<br>
   +/+/+/+/system/deviceType = device classes, e.g. "pump", "heatwebNode" or "ASHP" (Air Source Heat Pump)
* Data points can be applied to multiple entities, using the 'global' keywork, for example:<br>
   heatweb/myNetwork/global/network/system/name = The name of the heat network, and applies to all elements.<br>
   heatweb/myNetwork/global/hiu/setpoint/tDHW = The default hot water setpoint on HIUs.<br>


## Connecting to MQTT Services

To access data on an MQTT server you will need:

* server address
* server port number
* type of connection (encrypted/unencrypted)
* user name
* user password
* list of topics you have permission for

## Device Types

* boilers
* boiler
* heat pump
* buffer
* hiu
* block
* substation
* pumps
* pump
* pressure
* network
* energy centre
* bms
