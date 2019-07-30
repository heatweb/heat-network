# MQTT Parser

This flow takes incoming MQTT messages and uses data pulled from this repository to:

* Provide descriptions of topics
* Provide units of measurement
* Setup default values
* Setup calculated values
* Implement manufacturer specific topics using the *system/deviceVersion* topic
* Save incoming data into local memory along with a timestamp
