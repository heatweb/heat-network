# Heat Network MQTT Protocol
Heat network protocol based on standardised network control and reporting methodology.

The devices folder contains information for each type of device.

Protocol Rules:

* camelCase naming.
* 5 levels of MQTT topic for a network (networkId / publisherId / deviceId / dataType / key).
* Underscore _ character not to be used in naming. It is reserved for replacing / character in non MQTT systems.


