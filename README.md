# homeassistant-apsystems_ecur
This is a custom component for [Home Assistant](http://home-assistant.io) that adds support for the APSystems ECU-R solar energy monitoring unit [APsystem](http://www.apsystems.com).

## Background & acknowledgement
This integration queries the local ECU-R every 1 minute for new data. This was done without a public API, and by reverse engineering the protocol the APSystems ECU phone app uses when setting up the array.

This couldn't have been done without the hardwork of @checking12 and @HAEdwin on the home assistant forum, and all the other people from this forum (https://gathering.tweakers.net/forum/list_messages/2032302/1)

Currently this is only been tested on QS1 inverters, but should work on YC600 too

## Setup
Copy contents of the apsystems_ecur/ directory into your <HA-CONFIG>/custom_components/apsystems_ecur directory (```/config/custom_components``` on hassio)

Your directory structure should look like this:
```
   config/custom_components/apsystems_ecur/__init__.py
   config/custom_components/apsystems_ecur/const.py
   config/custom_components/apsystems_ecur/sensor.py
   config/custom_components/apsystems_ecur/APSysstemsECUR.py
   config/custom_components/apsystems_ecur/manifest.json
```

## Configuration
Add the following snippet into your ```configuration.yaml```  replace [IPADDR] with the IP address of your ECU-R device.  _NOTE_ testing has shown this only works on the WiFi IP address.  If you use the Ethernet IP of the ECU-R you will get no response from port 8899 on the ECU-R.

```

apsystems_ecur:
    host: [IPADDR]

```

## Data available
The component supports getting data from the array as a whole as well as each individual invertor.

### Array Level Sensors
* sensor.ecu_current_power - total amount of power (in W) being generated right now
* sensor.ecu_today_energy - total amount of energy (in kWh) generated today now
* sensor.ecu_lifetime_energy - total amount of energy (in kWh) generated from the lifetime of the array

### Inverter Level Sensors
There will be this set of sensors for every inverter you have in your system, UID will be replaced by the UID of the inverter discovered

* sensor.inverter_[UID]_frequency - the AC power frequency in Hz
* sensor.inverter_[UID]_voltage - the AC voltage in V
* sensor.inverter_[UID]_temperature - the temperature of the invertor in your local unit (C or F)
* sensor.inverter_[UID]_signal - the signal strength of the zigbee connection
* sensor.inverter_[UID]_power_ch_[1-4] - the current power generation (in W) of each channel of the invertor - number of channels will depend on inverter model

## TODO
1. Code cleanup - it probably needs some work
1. Further testing on more systems

