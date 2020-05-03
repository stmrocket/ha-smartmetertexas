## Node-Red Implementation of the Smart Meter Texas API

Access your current meter reading and daily usage from the Smart Meter Texas API!  The Node-Red flow requests a meter read every 30 minutes and reports the results over MQTT.  There is also a package for Home Assistant providing a number of useful sensors for use in your home.

---
### Prerequisites:
* An electric meter enrolled with Smart Meter Texas
* Your Smart Meter Texas Username, Password, ESIID, and Meter Number
* An MQTT Server
* Node-Red and the following additional nodes:
   * node-red-contrib-config
   * node-red-contrib-https
---
### Installation:
1. Make sure your system meets the above prerequisites.
2. Import the __smart_meter_texas.json__ file into your Node-Red instance.
3. If you are using Home Assistant, place the __smartmetertexas.yaml__ file in your config/packages directory.  More information of using packages can be found [HERE](https://www.home-assistant.io/docs/configuration/packages/#create-a-packages-folder "HERE").
4. Continue with configuration!
---
### Configuration:
1. Open the imported Node-Red flow and open the Configuration node.  You will need to fill in your Smart Meter Texas Username, Password, ESIID, and Meter Number.  Click 'Done' when you are finished.
2. Open the 'MQTT: Send Reading' node and configure your MQTT server information.
3. When you are finished configuring the nodes, click the 'Deploy' button to start the flow with your new configuration.
---
### Usage:
The Node-Red flow will request a meter read at 30 minute intervals since the last successful read.  It will then request the results of the meter read every 30 seconds until they are available.  Both the current meter reading ais reported over MQTT with the topic smt/reading.  

Note that the API limits each ESIID to two reads per hour (not read requests).  The limit is based on the time when the reading is successfully retrieved from the meter.  Increasing the frequency of reads will result in an error until an hour elapses from the oldest request.

---
__Disclaimer:__ This information is not provided by, nor endorsed by Smart Meter Texas.  As this API is unpublished, it could break at any time.  