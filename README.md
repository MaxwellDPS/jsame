# jsame

**jsame** is a JS library to decode [EAS](http://en.wikipedia.org/wiki/Emergency_Alert_System)/[SAME](http://en.wikipedia.org/wiki/Specific_Area_Message_Encoding) (Emergency Alert System/Specific Area Message Encoding) alert messages. These messages are primarily used by the National Weather Service for weather-related warnings. 
**jsame** will decode a demodulated message, filter by SAME ([US](http://www.nws.noaa.gov/nwr/coverage/county_coverage.html)/[CA](http://www.ec.gc.ca/meteo-weather/default.asp?lang=En&n=E5A4F19C-1)) and/or event code, provide readable object back.

**PLESE DONT TRUST THIS CODE WITH YOUR LIFE... COMMON SENSE GUYS**

### Requirements

* [NPM](https://www.npmjs.com/)
* [NodeJS](https://nodejs.org/en/)
* [Moment.js](https://momentjs.com/) +2.18.0


### Installation

> npm install @maxwelldps/jsame

### Usage

**jsame** can decode EAS messages diectly from multitmon, or in SAME format `ZCZC-ORG-EEE-PSSCCC-PSSCCC+TTTT-JJJHHMM-LLLLLLLL-`

>jsame.decode(Message, List of EEE to exclude, List of FIPS to watch )

Example to watch for Lancatser County NE, and not exclude events.
>jsame.decode("ZCZC-WXR-RWT-031109+0030-0771800-KOAX/NWS", [], ['031109'])


Exlude Required Weekly tests
>jsame.decode("ZCZC-WXR-RWT-031109+0030-0771800-KOAX/NWS", ['RWT'])

Exlude Required Weekly tests, and Match state of NE
>jsame.decode("ZCZC-WXR-RWT-031109+0030-0771800-KOAX/NWS", ['RWT'], ['031109'])


Example input
>ZCZC-WXR-RWT-055027-055039-055047-055117-055131-055137-055139-055015-055071+0030-0771800-KMKX/NWS-


>EAS:  ZCZC-WXR-RWT-055027-055039-055047-055117-055131-055137-055139-055015-055071+0030-0771800-KMKX/NWS     -

```js
var SAME = require('./dsame.js'); 

Message = "ZCZC-WXR-RWT-055027-055039-055047-055117-055131-055137-055139-055015-055071+0030-0771800-KMKX/NWS-";

decodedMeassage = SAME.decode(Message);
console.log(decodedMeassage["MESSAGE"]);
```


#### Event Codes

*This list includes current and proposed event codes.*

Code| Description                  |Code| Description
:--:|:-----------------------------|:--:|:-----------------------------
ADR | Administrative Message       |AVA | Avalanche Watch
AVW | Avalanche Warning            |BHW | Biological Hazard Warning
BWW | Boil Water Warning           |BZW | Blizzard Warning
CAE | Child Abduction Emergency    |CDW | Civil Danger Warning
CEM | Civil Emergency Message      |CFA | Coastal Flood Watch
CFW | Coastal Flood Warning        |CHW | Chemical Hazard Warning
CWW | Contaminated Water Warning   |DBA | Dam Watch
DBW | Dam Break Warning            |DEW | Contagious Disease Warning
DMO | Demo Warning                 |DSW | Dust Storm Warning
EAN | Emergency Action Notification|EAT | Emergengy Action Termination
EQW | Earthquake Warning           |EVA | Evacuation Watch
EVI | Evacuation Immediate         |EWW | Extreme Wind Warning
FCW | Food Contamination Warning   |FFA | Flash Flood Watch
FFS | Flash Flood Statement        |FFW | Flash Flood Warning
FLA | Flood Watch                  |FLS | Flood Statement
FLW | Flood Warning                |FRW | Fire Warning
FSW | Flash Freeze Warning         |FZW | Freeze Warning
HLS | Hurricane Local Statement    |HMW | Hazardous Materials Warning
HUA | Hurricane Watch              |HUW | Hurricane Warning
HWA | High Wind Watch              |HWW | High Wind Warning
IBW | Iceberg Warning              |IFW | Industrial Fire Warning
LAE | Local Area Emergency         |LEW | Law Enforcement Warning
LSW | Land Slide Warning           |NAT | National Audible Test
NIC | National Information Center  |NMN | Network Message Notification
NPT | National Periodic Test       |NST | National Silent Test
NUW | Nuclear Plant Warning        |POS | Power Outage Statement
RHW | Radiological Hazard Warning  |RMT | Required Monthly Test
RWT | Required Weekly Test         |SMW | Special Marine Warning
SPS | Special Weather Statement    |SPW | Shelter in Place Warning
SSA | Storm Surge Watch            |SSW | Storm Surge Warning
SVA | Severe Thunderstorm Watch    |SVR | Severe Thunderstorm Warning
SVS | Severe Weather Statement     |TOA | Tornado Watch
TOE | 911 Outage Emergency         |TOR | Tornado Warning
TRA | Tropical Storm Watch         |TRW | Tropical Storm Warning
TSA | Tsunami Watch                |TSW | Tsunami Warning
VOW | Volcano Warning              |WFA | Wild Fire Watch
WFW | Wild Fire Warning            |WSA | Winter Storm Watch
WSW | Winter Storm Warning         |BLU | Blue Alert
SQW | Snow Squall Warning          |    | 

An alert must match one of each specified alert type in order to be processed. If an alert type is omitted, any alert will match that type. In most cases, using only SAME codes to filter alerts will be the best option.


#### Output 

Output is an object with the following

Variable        | Description                       | Example
:---------------|:----------------------------------|:------------------
 MESSAGE        | Readable message                  | *(See sample text output below)*
 ORG            | Organization code                 | WXR
 EEE            | Event code                        | RWT
 PSSCCC         | Geographical area (SAME) codes    | 020103-020209-020091-020121-029047-029165-029095-029037
 TTTT           | Purge time code                   | 0030
 JJJHHMM        | Date code                         | 1051700
 LLLLLLLL       | Originator code                   | KEAX/NWS
 COUNTRY        | Country code                      | US
 LLLL-ORG       | Originating station - Org code    | KEAX-WXR
 {organization} | Organization name                 | National Weather Service
 {location}     | Originator location               | Pleasant Hill, Missouri
 {event}        | Event type                        | Required Weekly Test
 {type}         | Event type indicator              | T
 {start}        | Start time                        | 12:00 PM
 {end}          | End time                          | 12:30 PM
 {length}       | Length of event                   | 30 minutes
 {seconds}      | Event length in seconds           | 1800 
 {date}         | Local date                        | Tuesday, March 17th 2020, 1:00:00 pm


### Sample output 1
>"EAS: ZCZC-WXR-EWW-055027-055039-055047-055117-055131-055137-055139-055015-055071+0030-0771800-KOAX/NWS-"
>(same_decode(TEST_STRING2, ['RWT', 'DMO', 'RMT'], ['055000']));

```json
{ 
  MESSAGE: 'The National Weather Service in Omaha, NE has issued a Extreme Wind Warning valid until 1:30 PM for the following counties in Wisconsin: Calumet, Dodge, Fond du Lac, Green Lake, Manitowoc, Sheboygan, Washington, Waushara, Winnebago. (KOAX/NWS)',
  ORG: 'WXR',
  EEE: 'EWW',
  PSSCCC_LIST:
   [ '055015',
     '055027',
     '055039',
     '055047',
     '055071',
     '055117',
     '055131',
     '055137',
     '055139' ],
  TTTT: '0030',
  JJJHHMM: '0771800',
  STATION: 'KOAX',
  LLLLLLLL: 'KOAX/NWS',
  COUNTRY: 'US',
  'LLLL-ORG': 'KOAX-WXR',
  organization: 'National Weather Service',
  location: 'Omaha, NE ',
  event: 'Extreme Wind Warning',
  type: 'W',
  start: '1:00 PM',
  end: '1:30 PM',
  length: 30,
  seconds: 1800,
  date: 'Tuesday, March 17th 2020, 1:00:00 pm' 
}
```

### Sample output 2
>"ZCZC-CIV-LAE-000000+0030-0771800-KXYZ/FM-"
>(same_decode(TEST_STRING2, ['RWT', 'DMO', 'RMT']));

```json
{ 
  MESSAGE: 'The Civil Authorities in undefined have issued a Local Area Emergency valid until 1:30 PM for the following counties in Nebraska: ALL. (KXYZ/NWS)',
  ORG: 'CIV',
  EEE: 'LAE',
  PSSCCC_LIST: [ '031000' ],
  TTTT: '0030',
  JJJHHMM: '0771800',
  STATION: 'KXYZ',
  LLLLLLLL: 'KXYZ/NWS',
  COUNTRY: 'US',
  'LLLL-ORG': 'KXYZ-CIV',
  organization: 'Civil Authorities',
  location: 'undefined ',
  event: 'Local Area Emergency',
  type: 'E',
  start: '1:00 PM',
  end: '1:30 PM',
  length: 30,
  seconds: 1800,
  date: 'Tuesday, March 17th 2020, 1:00:00 pm'
}

```

### Sample Text Output

>The National Weather Service in Pleasant Hill, Missouri has issued a Required Weekly Test valid until 12:30 PM for the following counties in Kansas: Leavenworth, Wyandotte, Johnson, Miami, and for the following counties in Missouri: Clay, Platte, Jackson, Cass. (KEAX/NWS)

This [experimental pagermon site](https://wx.maxwelldps.com/) is updated using jsame, pagermon, multimon-ng and a rtl-sdr dongle.

### Known Issues

### Credits
Thanks to [dsame Copyright (c) 2016 Joseph W. Metcalf](https://github.com/cuppa-joe/dsame) from which the code was adapted

---
