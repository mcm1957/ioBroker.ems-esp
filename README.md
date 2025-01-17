![Logo](admin/ems-esp.png)
# ioBroker.ems-esp

[![NPM version](https://img.shields.io/npm/v/iobroker.ems-esp.svg)](https://www.npmjs.com/package/iobroker.ems-esp)
[![Downloads](https://img.shields.io/npm/dm/iobroker.ems-esp.svg)](https://www.npmjs.com/package/iobroker.ems-esp)
![Number of Installations (latest)](https://iobroker.live/badges/ems-esp-installed.svg)
![Number of Installations (stable)](https://iobroker.live/badges/ems-esp-stable.svg)
[![Dependency Status](https://img.shields.io/david/tp1de/iobroker.ems-esp.svg)](https://david-dm.org/tp1de/iobroker.ems-esp)

[![NPM](https://nodei.co/npm/iobroker.ems-esp.png?downloads=true)](https://nodei.co/npm/iobroker.ems-esp/)

**Tests:** ![Test and Release](https://github.com/tp1de/ioBroker.ems-esp/workflows/Test%20and%20Release/badge.svg)

## Bosch / Buderus heating systems with km200 / IP-inside and/or ems-esp interface 

The adapter supports an interface towards the heating systems from Bosch Group using EMS or EMS+ bus. 
(Buderus / Junkers / Netfit etc). 

## It can interface towards the heating system with use of Web-API calls toward:

* km200, km200 hrv, km100, km50, HMC300 or IP-inside (from Bosch Group) 

* ems-esp gateway (https://github.com/emsesp/EMS-ESP32) with latest dev version (see below) and the ESP32 chip. 
  The old ESP8266 gateways with API V2 are NOT SUPPORTED ANYMORE !!
  The adapter is tested for the ems-esp gateway with latest firmware version (> V3.6.0) of ESP32

* New Bosch-Group Cloud-Gateways (MX300 / EasyControl ...) are not supported since they do not support LAN API !

The ioBroker ems-esp adapter can read and write data to both gateways to control all heating components. 
It can be used either for the original Bosch-Group gateways or the ems-esp or both in parallel.
All changed states from own scripts or the object browser does have to set acknowledged = false !!!


## NEW EMS+ entities (switchPrograms and holidayModes) are implemented for EMS-ESP gateway and if found states are created. 
	The ems-esp gateway firmware does not support switchPrograms and holidayModes for EMS+ thermostats (RC310 / RC300 or similar)
	Enabling this new function will issue raw telegrams toward the ems-esp gateway and then try to read the response
	Testing is done for switchPrograms A and B for hc1 to hc4, dhw (warm water) and circulation pump (cp) and holidayModes hm1-hm5
	When the respective states are found, they are stored in the instance config and the instance is restarted (search is only once).
	
	Then after these found states the raw response is decoded and states are created identically to KM200 gateway API data
	When the km200 gateway is enabled then this function is disabled to avoid double entries with same name
	The states created consist of JSON structures, enum values or arrays and are writable - Be carefull with the right content
	I recommend to test by using the Bosch/Buderus apps to identify the right content - especially for holidayModes.
	Polling is set to every 5 minutes.

## NEW Energy recordings and statistics need an active database instance. 
	Recordings require a InfluxDB adapter version >= 4.0.2 which allows deleting of db-records
	Retition period is now read and recordings are only stored within retention period -- Beta status
	InfluxDB v2 needs the retention period to be set to > 2 years for storing all historic values. 
	In V2 this is a global parameter for all states ! 
	
## NEW: Heat Demand hysteresis improved. 
    Heat Demand per thermostat is active when actual temp is lower than (target temp - delta).
	Heat Demand is inactive when actual temp is higher then target temp.
	Make sure that delta is high enough to avoid too many boiler starts.

## NEW: Heat Demand paramters can be changed during active instance
	Heat Demand parameters delta / weight for each thermostat can be changed within objects during active instance
	Heat Demand parameters weighton / weightoff for each heating circuit can be changed within objects during active instance


German  documentation: https://github.com/tp1de/ioBroker.ems-esp/blob/main/doc/ems-esp-ds.pdf

English documentation: https://github.com/tp1de/ioBroker.ems-esp/blob/main/doc/ems-esp-es.pdf


## Changelog
<!--
	Placeholder for the next version (at the beginning of the line):
	### **WORK IN PROGRESS**
-->
### **WORK IN PROGRESS**
* ems-esp gateway: Raw telegram search for EMS+ thermostats: switchPrograms and holidayModes (RC310/RC300)
* create writable objects / states for switchPrograms and holidayModes
* this function is only active when no km200 gateway is selected - ems-esp gateway only
* improve error messages for km200 (wrong ip / passwords)
* small changes within PDF adapter documentation

### 3.0.0-alpha.0 (2024-02-05)
* Search for ems-esp states for EMS+ thermostats: switchPrograms and holidayModes (RC310/RC300)
* Implement raw telegram search for EMS+ entities and create writable objects / states
* The search is only active when no km200 gateway is selected

### 2.8.0 (2024-02-04)
* influxdb adapter version >= 4.0.2 required 
* store km200 recordings only within defined retention period for influxdb
* delay start of statistics by 5 minutes

### 2.7.5 (2024-02-02)
* allow only positive deltam in config for heat demand function

### 2.7.4 (2024-02-01)
* avoid sql errors on instance start

### 2.7.3 (2024-01-31)
* error correction for heat demand function

## License
MIT License

Copyright (c) 2024 Thomas Petrick <tp1degit@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE."
# iobroker.ems-esp
