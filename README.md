## Node-Red Implementation of the Smart Meter Texas API

Access your current meter reading from the Smart Meter Texas API!  The Node-Red flow requests a meter read every hour and reports the results over MQTT or to a Home Assistant entity.  There is also a package for Home Assistant providing a number of useful sensors for use in your instance, if you wish not to use the native Energy Dashboard.

---
### Prerequisites:
* An electric meter enrolled with Smart Meter Texas
* Your Smart Meter Texas Username, Password, ESIID, and Meter Number
* An MQTT Server (Optional if using integration below)
* Home Assistant [Node-Red Companion Integration](https://github.com/zachowj/hass-node-red) (Optional if using MQTT)
* Node-Red and the following additional nodes:
   * node-red-contrib-https
   * node-red-contrib-credentials
   * node-red-contrib-home-assistant-websocket (only needed for those who want direct home assistant integration rather than MQTT)
---
### Installation:
1. Make sure your system meets the above prerequisites.
2. Import the __Smart Meter Texas **.json__ file into your Node-Red instance. You only need one or the other, not both.
3. If you are using MQTT and Home Assistant (not integration), place the __smartmetertexas.yaml__ file in your config/packages directory, or copy the sensor configuration lines in the file to your main configuration file. More information of using packages can be found [HERE](https://www.home-assistant.io/docs/configuration/packages/#create-a-packages-folder "HERE").
#### NOTE: To use the native Energy Dashboard, you only need to add the mqtt sensor to your configuration      and nothing else. If you wish to have your own utility meter entities, then copy the rest of the          entities along with the mqtt sensor.

5. Continue with configuration!
---
### Configuration:
1. Open the imported Node-Red flow and open the __Credentials__ node.  You will need to fill in your Smart Meter Texas Username, Password, ESIID, and Meter Number.  Optionally, you can change the minute when the meter will be polled every hour in the respective cron job nodes. It is advised to have the authentication cron run BEFORE the request read cron.  Click __Done__ when you are finished.
2. MQTT Users: Open the __MQTT: Send Reading__ node and configure your MQTT server information.
3. Node-Red Companion Integ. Users: Open the __SMT Current Reading__ node and set your home assistant instance.
4. When you are finished configuring the nodes, click the __Deploy__ button to start the flow with your new configuration.
5.(Optional): If you are using the optional Home Assistant sensors package, and do not wish to use the native Energy Dashboard, set your electricity cost in the entity __input_number.smt_energy_cost__.
---
### Usage:
The Node-Red flow will request a meter read at 30 minute intervals since the last successful read.  It will then request the results of the meter read every 30 seconds until they are available.  The current meter reading is reported over MQTT with the topic smt/reading or to the configured Home Assistant entity.  

Note that the API limits each ESIID to two reads per hour and 24 reads per day.  The limit is based on the time when the reading is successfully retrieved from the meter.  Increasing the frequency of reads will result in an error until the hour or day resets (depending on the error).

---
__Thanks:__ This code would not be possible without the previous work by [keatontaylor](https://github.com/keatontaylor/smartmetertexas-api).

---
__Disclaimer:__ This information is not provided by, nor endorsed by Smart Meter Texas.  As this API is unpublished, it could break at any time.  
