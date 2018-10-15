# ![logo](/docs/branding.bmp) Telemetry Analytics API

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Query Metadata**](/docs/Metadata.md)<br>
- [**Consuming Data**](/docs/ConsumingData.md)<br>
- [**Views**](/docs/Views.md)<br>


Query Metadata
========  

There are multiple endpoints available for query sessions information.  

(High level explanation here)

  
Query sessions  
==============  

The "/sessions" endpoint give you access to a list of **historical** sessions available for a given connection. You can filter this list of sessions by several parameters.

Optional parameters  
-------------------  
  
| Parameter name | Description | Default value | Example |  
|----------------|-------------------------------------------------------------|---------------|---------------------------------------------------------------------------|  
| items | This filter sessions by session items like driver, car etc. | | Driver:KHA,Car:P1GTR |  
| filter | This filter returned query with expression. | | LapsCount &gt; 5 %26%26 TimeOfRecording &gt; DateTime.Parse("2017-12-18") |  
| page | Index of page returned in result (0 is first page) | | 3 |  
| pageSize | Size of one page. | 200 | 50 |  
  
Filter  
------  
  
Please note that some characters are not allowed in URL and therefore  
must be specified with percentage and symbol number. For example: & must  
be replaced by %26.  
  
Query  

```
GET api/connections/{connection friendly name}/sessions
```

Example  

```
GET api/connections/Simulator/sessions?items=Driver:KHA,Car:P1GTR&filter=LapsCount > 5 %26%26 TimeOfRecording >
DateTime.Parse("2017-12-18")
```

Result  
```json
[
    {
        "Key": "7b7b910b-0b3a-4aff-b64f-54204d8ea863",
        "Identifier": "171218111334",
        "TimeOfRecording": "2017-12-18T11:13:34.3866611",
        "TimeZone": "UTC",
        "SessionType": "Session",
        "StartTime": "12:46:02.0290000",
        "EndTime": "13:08:40.1190000",
        "LapsCount": 9,
        "Id": 1495,
        "State": 0
    },
    {
        "Key": "652618d1-995e-48dd-91c3-600447f59d9e",
        "Identifier": "171218115852",
        "TimeOfRecording": "2017-12-18T11:58:52.8114764",
        "TimeZone": "UTC",
        "SessionType": "Session",
        "StartTime": "13:31:06.9320000",
        "EndTime": "13:42:57.5890000",
        "LapsCount": 6,
        "Id": 1497,
        "State": 2
    }
]
```
  
Query live sessions  
===================  

The "/sessions/live" endpoint give you access to a list of **live** sessions available for a given connection. You can filter this list of sessions by several parameters.

Optional parameters  
-------------------  
  
| Parameter name | Description | Default value | Example |  
|----------------|-------------------------------------------------------------|---------------|---------------------------------------------------------------------------|  
| filter | This filter returned query with expression. | | LapsCount &gt; 5 %26%26 TimeOfRecording &gt; DateTime.Parse("2017-12-18") |  
| page | Index of page returned in result (0 is first page) | | 3 |  
| pageSize | Size of one page. | 200 | 50 |  
  
Filter  
------  
  
Please note that some characters are not allowed in URL and therefore  
must be specified with percentage and symbol number. For example: & must  
be replaced by %26.  

Query  
```
GET api/connections/{connection friendly name}/sessions/live
```
  
Example  
```
GET api/connections/Simulator/sessions/live
```

Items  
=====

(High level explanation)

Query list of items in session:  

Query  
```
GET api/connections/{connection friendly name}/sessions/{sessionKey}/items
```
  
Example  
```
GET api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/items
```
  
Result  
```json
[
    {
        "Name": "Session Description",
        "Value": "P1GTR"
    },
    {
        "Name": "Session Name",
        "Value": "GAS R1"
    },
    {
        "Name": "Session Number",
        "Value": "R1"
    },
    {
        "Name": "Driver",
        "Value": "GAS"
    },
    {
        "Name": "Car",
        "Value": "P1GTR"
    },
    {
        "Name": "Circuit",
        "Value": "Bar"
    },
    {
        "Name": "Race/Test",
        "Value": "Simulator"
    },
    {
        "Name": "Pit Lane Trigger",
        "Value": "None"
    },
    {
        "Name": "Unit Data Source",
        "Value": "vTAG RF,Ethernet Telemetry/Wirelink "
    },
    {
        "Name": "Date of recording",
        "Value": "07/08/2017"
    }
]
```

Laps  
====  
  
Query list of laps in session:  
  
Query
  
```
GET api/connections/{connection friendly name}/sessions/{sessionKey}/laps
```
  
Query example  
  
```
api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/laps
```
  
Result
  
```json
[
    {
        "StartTime": "12:08:15.5120000",
        "EndTime": "12:09:05.7300000",
        "LapTime": "00:00:50.2180000",
        "CountForFastestLap": false,
        "Name": "Out Lap",
        "Number": 0
    },
    {
        "StartTime": "12:09:05.7300000",
        "EndTime": "12:13:37.1600000",
        "LapTime": "00:04:31.4300000",
        "CountForFastestLap": true,
        "Name": "Lap 16",
        "Number": 16
    },
    {
        "StartTime": "12:13:37.1600000",
        "EndTime": "12:16:15.0300000",
        "LapTime": "00:02:37.8700000",
        "CountForFastestLap": true,
        "Name": "Lap 17",
        "Number": 17
    },
    {
        "StartTime": "12:16:15.0300000",
        "EndTime": "12:18:48.8300000",
        "LapTime": "00:02:33.8000000",
        "CountForFastestLap": true,
        "Name": "Lap 18",
        "Number": 18
    }
]
```


Parameters  
==========  
  
Query list of parameters in session:  
  
Query parameters  
  
```
api/connections/{connection friendly name}/sessions/{sessionKey}/parameters
```
  
Optional parameters  
-------------------  
  
| Parameter name | Description | Example |  
|----------------|----------------------------------------------------|---------|  
| page | Index of page returned in result (0 is first page) | 3 |  
| pageSize | Size of one page. | 50 |  
| contains | Text filter | vCar |  
  
Query parameters example  
  
```
api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters
```
  
### Paging  
  
Query parameters example  
  
```
api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters?page=2&amp;pageSize=50
```
  
### Filtering  
  
Query parameters example  
  
```
api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters?contains=vCar
```
  
Result  
  
```json
{
        "Name": "vCar",
        "MaximumValue": 400,
        "MinimumValue": 0,
        "Description": "Car Speed"
    },
    {
        "Name": "vCarHigh",
        "MaximumValue": 400,
        "MinimumValue": 0,
        "Description": "Car speed from vHighestF adjusted to centreline"
    },
    {
        "Name": "vCarMG",
        "MaximumValue": 1000,
        "MinimumValue": 0,
        "Description": "vCar based on nEngine"
    },
    {
        "Name": "vCarEOS",
        "MaximumValue": 350,
        "MinimumValue": 0,
        "Description": "Car speed (end of straight)"
    },
    {
        "Name": "vCarLimited",
        "MaximumValue": 400,
        "MinimumValue": 0,
        "Description": "Power Limited Car Speed"
    },
    {
        "Name": "vCarMaxOilSample",
        "MaximumValue": 500,
        "MinimumValue": 0,
        "Description": "Max car speed in oil sample window"
    },
    {
        "Name": "vCarHighRaw",
        "MaximumValue": 400,
        "MinimumValue": 0,
        "Description": "Unlimited car speed for slip calcs from vCarHigh"
    }
```

    