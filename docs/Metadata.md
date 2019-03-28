# ![logo](/docs/branding.bmp) Telemetry Analytics API

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Installation**](/docs/Installation.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Identity Server**](/docs/IdentityServer.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Querying Metadata**](/docs/Metadata.md)<br>
- [**Consuming Data**](/docs/ConsumingData.md)<br>
- [**Views**](/docs/Views.md)<br>


## Querying Metadata

This section explains in detail what endpoints are available to retreive session metadata and how to execute them.
It is possible to collect session metadata from both InfluxDb and SqlRace data storages. Collected data is provided in Json format.

Sessions
========

The ```/sessions``` endpoint gives access to a list of **historical** sessions available for a given connection.<br />

### Query all available sessions

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions
```

Example  
```
GET api/v1/connections/TestConnection/sessions
```

Result  
```json
[
  {
    "key": "0151d834-7a23-46c6-a3fc-eb536adcf93b",
    "identifier": "Identifier7",
    "timeOfRecording": "2018-12-09T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:33:08.3917995Z",
    "end": "2019-03-28T14:33:08.3917995Z",
    "lapsCount": 10,
    "state": "Open",
    "topicName": "TopicName7",
    "sessionDetails": []
  },
  {
    "key": "01bdac9f-18d9-4e8c-956b-c397206bf5a4",
    "identifier": "Identifier5",
    "timeOfRecording": "2018-11-29T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:36:42.1170779Z",
    "end": "2019-03-28T14:36:42.1170779Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName5",
    "sessionDetails": []
  },
  {
    "key": "09d6802c-a9b8-4a77-bc40-3dbffd88ac7b",
    "identifier": "Identifier4",
    "timeOfRecording": "2018-11-23T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:36:42.1170779Z",
    "end": "2019-03-28T14:36:42.1170779Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName4",
    "sessionDetails": []
  },
  {
    "key": "0fa13add-4c1f-4dda-a7be-32bbfc1b8fc6",
    "identifier": "Identifier3",
    "timeOfRecording": "2018-12-05T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:42:14.1820662Z",
    "end": "2019-03-28T14:42:14.1820662Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName3",
    "sessionDetails": []
  },
  {
    "key": "150cc8cc-ef42-4730-826f-3705af91360c",
    "identifier": "Identifier2",
    "timeOfRecording": "2018-12-04T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:42:14.1820662Z",
    "end": "2019-03-28T14:56:26.1820662Z",
    "lapsCount": 10,
    "state": "Open",
    "topicName": "TopicName2",
    "sessionDetails": []
  }
]
```

Optional parameters  
-------------------  

The ```/sessions``` endpoint provides a set of optional parameters from which is possible to filter, sample size it or order the set of requested data.
  
| Parameter name | Description                                                 | Default value | Example                                                                   |  
|----------------|-------------------------------------------------------------|---------------|---------------------------------------------------------------------------|
| page           | Index of the page returned in result (0 is first page)      |               | 3                                                                         |  
| pageSize       | Size of one page.                                           | 200           | 50                                                                        |
| details        | Filters sessions by session details like driver, car etc.   |               | Driver:KHA,Car:P1GTR                                                      |  
| filter         | It allows to filter the returned set of results.            |               | state;eq;Closed                                                           |  
| order          | It allows to order by the returned set of results.          |               | timeofrecording:desc                                                      |


Filters
------ 

It is possible to collect session data applying specific filters to it.<br />
There are multiple types of filters being those:

#### Filter types

| Shortcut | Full name             |
|----------|-----------------------|
| eq       | Equal                 |
| gt       | Greater than          |
| ge       | Greater than or equal |
| lt       | Less than             |
| le       | Less than or equal    |
| ne       | Not equal             |
  

#### Parameter filter construction

Structure of one parameter filter is:

Parameter filter url mask
```
{parameterName};{filterOperationShortcut};{value}
```
  
#### <ins>Filtering by closed state</ins>

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions
```

Example  
```
GET api/v1/connections/TestConnection/sessions?filter=state;eq;Closed
```

Result  
```json
[
  {
    "key": "16881d4c-7bcc-48f1-934f-a32645fc829f",
    "identifier": "Identifier6",
    "timeOfRecording": "2018-12-09T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T12:36:10.6907316Z",
    "end": "2019-03-28T12:36:10.6907316Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName6",
    "sessionDetails": []
  },
  {
    "key": "23d61829-cd8d-4522-8951-f9c0f3867548",
    "identifier": "Identifier5",
    "timeOfRecording": "2018-12-10T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T12:36:10.6907316Z",
    "end": "2019-03-28T12:36:10.6907316Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName5",
    "sessionDetails": []
  },
  {
    "key": "7a3eb574-2038-4fa8-bebe-9b13eef64ab7",
    "identifier": "Identifier4",
    "timeOfRecording": "2018-11-29T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:33:08.3917995Z",
    "end": "2019-03-28T14:33:08.3917995Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName4",
    "sessionDetails": []
  },
  {
    "key": "7a5e7a00-1860-449f-913b-e03688223622",
    "identifier": "Identifier10",
    "timeOfRecording": "2018-12-02T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T12:36:10.6907316Z",
    "end": "2019-03-28T12:36:10.6907316Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName10",
    "sessionDetails": []
  },
  {
    "key": "8b420497-c312-45c5-93cd-4a2758d28e66",
    "identifier": "Identifier10",
    "timeOfRecording": "2018-12-10T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:33:08.3917995Z",
    "end": "2019-03-28T14:33:08.3917995Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName10",
    "sessionDetails": []
  }
]
```

#### <ins>Filtering by time of recording bigger than</ins>

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions
```

Example  
```
GET api/v1/connections/TestConnection/sessions?filter=timeOfRecording;gt;2018-12-03T00:00:00Z
```

Result  
```json
[
  {
    "key": "0151d834-7a23-46c6-a3fc-eb536adcf93b",
    "identifier": "Identifier7",
    "timeOfRecording": "2018-12-09T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:33:08.3917995Z",
    "end": "2019-03-28T14:33:08.3917995Z",
    "lapsCount": 10,
    "state": "Open",
    "topicName": "TopicName7",
    "sessionDetails": []
  },
  {
    "key": "16881d4c-7bcc-48f1-934f-a32645fc829f",
    "identifier": "Identifier6",
    "timeOfRecording": "2018-12-09T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T12:36:10.6907316Z",
    "end": "2019-03-28T12:36:10.6907316Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName6",
    "sessionDetails": []
  },
  {
    "key": "23d61829-cd8d-4522-8951-f9c0f3867548",
    "identifier": "Identifier5",
    "timeOfRecording": "2018-12-10T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T12:36:10.6907316Z",
    "end": "2019-03-28T12:36:10.6907316Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName5",
    "sessionDetails": []
  },
  {
    "key": "52930321-f71b-4380-a886-45a8fe077e29",
    "identifier": "Identifier6",
    "timeOfRecording": "2018-12-05T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:36:42.1170779Z",
    "end": "2019-03-28T14:36:42.1170779Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName6",
    "sessionDetails": []
  },
  {
    "key": "8b420497-c312-45c5-93cd-4a2758d28e66",
    "identifier": "Identifier10",
    "timeOfRecording": "2018-12-10T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:33:08.3917995Z",
    "end": "2019-03-28T14:33:08.3917995Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName10",
    "sessionDetails": []
  }
]
```

Ordering
------

To a collection of data is also provided the option of ordering the requested data set. The properties can be ordered by asc or desc.

#### Parameter order construction

Structure of one parameter filter is:

Parameter filter url mask
```
{parameterName};{order}
```

#### <ins>Ordering by end time descending</ins>

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions
```

Example  
```
GET api/v1/connections/TestConnection/sessions?orderBy=end:desc
```

Result  
```json
[
  {
    "key": "f75f1c7d-9192-42c8-89f0-c1f1b613adcc",
    "identifier": "Identifier2",
    "timeOfRecording": "2018-12-02T00:00:00Z",
    "timeZone": null,
    "sessionType": "StreamingSession",
    "start": "2019-03-28T11:01:31.7641885Z",
    "end": "2019-03-28T11:15:43.7641885Z",
    "lapsCount": 10,
    "state": "Open",
    "topicName": "TopicName2",
    "sessionDetails": []
  },
  {
    "key": "c64c8e95-7540-4951-8815-c908e51b2491",
    "identifier": "Identifier2",
    "timeOfRecording": "2018-12-07T00:00:00Z",
    "timeZone": null,
    "sessionType": "StreamingSession",
    "start": "2019-03-28T10:59:57.401443Z",
    "end": "2019-03-28T11:14:09.401443Z",
    "lapsCount": 10,
    "state": "Open",
    "topicName": "TopicName2",
    "sessionDetails": []
  },
  {
    "key": "fa3f18bb-153d-4c52-a1ff-8d04c46035b5",
    "identifier": "Identifier2",
    "timeOfRecording": "2018-12-05T00:00:00Z",
    "timeZone": null,
    "sessionType": "StreamingSession",
    "start": "2019-03-28T10:51:36.6730065Z",
    "end": "2019-03-28T11:05:48.6730065Z",
    "lapsCount": 10,
    "state": "Open",
    "topicName": "TopicName2",
    "sessionDetails": []
  },
  {
    "key": "test-session-id",
    "identifier": "TestSession1",
    "timeOfRecording": "2018-12-03T00:00:00Z",
    "timeZone": null,
    "sessionType": "StreamingSession",
    "start": "2019-03-28T10:51:36.6730065Z",
    "end": "2019-03-28T11:05:48.6730065Z",
    "lapsCount": 10,
    "state": "Open",
    "topicName": "TopicName1",
    "sessionDetails": []
  },
  {
    "key": "0179b921-1d0d-4b9e-96e1-9e9f1d86ccfd",
    "identifier": "Identifier10",
    "timeOfRecording": "2018-12-12T00:00:00Z",
    "timeZone": null,
    "sessionType": "StreamingSession",
    "start": "2019-03-28T11:01:31.7641885Z",
    "end": "2019-03-28T11:01:31.7641885Z",
    "lapsCount": 10,
    "state": "Closed",
    "topicName": "TopicName10",
    "sessionDetails": []
  }
]
```
<br />

### <ins>Query a specific session by its identifier</ins>

The API allows to retreive a session by its identifier.

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/id/{identifier}
```

Example  
```
GET api/v1/connections/TestConnection/sessions/id/TestSession1
```

Result  
```json
{
  "key": "test-session-id",
  "identifier": "TestSession1",
  "timeOfRecording": "2018-12-03T00:00:00Z",
  "sessionType": "StreamingSession",
  "start": "2019-03-28T12:36:10.6907316Z",
  "end": "2019-03-28T12:50:22.6907316Z",
  "lapsCount": 0,
  "state": "Open",
  "topicName": "TopicName1",
  "sessionDetails": []
}
```
<br />

### <ins>Query all sessions by their sessions keys</ins>

The API allows to retreive a session by its identifier.

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/id/{identifier}
```

Example  
```
GET api/v1/connections/TestConnection/sessions/01388874-7a80-4d86-8020-8709c278fc9a,0151d834-7a23-46c6-a3fc-eb536adcf93b
```

Result  
```json
[
  {
    "key": "01388874-7a80-4d86-8020-8709c278fc9a",
    "identifier": "Identifier3",
    "timeOfRecording": "2018-11-24T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T15:24:23.5599596Z",
    "end": "2019-03-28T15:24:23.5599596Z",
    "lapsCount": 0,
    "state": "Closed",
    "topicName": "TopicName3",
    "sessionDetails": []
  },
  {
    "key": "0151d834-7a23-46c6-a3fc-eb536adcf93b",
    "identifier": "Identifier7",
    "timeOfRecording": "2018-12-09T00:00:00Z",
    "sessionType": "StreamingSession",
    "start": "2019-03-28T14:33:08.3917995Z",
    "end": "2019-03-28T14:33:08.3917995Z",
    "lapsCount": 0,
    "state": "Open",
    "topicName": "TopicName7",
    "sessionDetails": []
  }
]
```

Live sessions  
===================  

The ```/sessions/live``` endpoint gives you access to a list of **live** sessions available for a given connection. You can filter this list of sessions by several parameters.

Optional parameters  
-------------------  
  
| Parameter name | Description                                                 | Default value | Example                                                                   |  
|----------------|-------------------------------------------------------------|---------------|---------------------------------------------------------------------------|  
| filter         | This filter returned query with expression.                 |               | LapsCount &gt; 5 %26%26 TimeOfRecording &gt; DateTime.Parse("2017-12-18") |  
| page           | Index of page returned in result (0 is first page)          |               | 3                                                                         |  
| pageSize       | Size of one page.                                           | 200           | 50                                                                        |  
  
Filter  
------  
  
Please note that some characters are not allowed in URL and therefore must be specified with percentage sign and symbol number. For example: & must be replaced by %26.  

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/live
```
  
Example  
```
GET api/v1/connections/Simulator/sessions/live
```


Parameters  
==========  

The ```/sessions/{sessionKey}/parameters``` endpoint gives you access to a list of **parameters** available for a specific session. The list of **parameters** of a session are the fields related to the data that we can consume as described in [Consuming Data] (/docs/ConsumingData.md) section.

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters
```

Example  
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters
```
  
Optional parameters  
-------------------  
  
| Parameter name | Description                                                 | Example           |  
|----------------|-------------------------------------------------------------|-------------------|  
| page           | Index of page returned in result (0 is first page)          | 3                 |  
| pageSize       | Size of one page.                                           | 50                |  
| contains       | Text filter applied to the <ins>identifier</ins> parameter. | vCar              |
| startsWith     | Text filter applied to the <ins>identifier</ins> parameter  | vCar              |
| filter         | It allows to filter the set of results.                     | Frequency;ge;10   |
| order          | It allows to order by the returned set of results.          | MaximumValue:desc |

### Paging  

Example  
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters?page=2&pageSize=50
```
  
### Filtering

It is possible to provide filtering for 
  
Example 
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters?contains=vCar
```
  
Result  
```json
[
  {
    "identifier": "vCar:APP1",
    "name": "vCar",
    "maximumValue": 100,
    "minimumValue": 0,
    "description": "fTAGDisp_vCar",
    "units": "",
    "format": "%5.2f",
    "parentParameterGroup": "APP1",
    "frequency": 0,
    "aggregates": "Avg"
  },
  {
    "identifier": "vCar:APP2",
    "name": "vCar",
    "maximumValue": 360,
    "minimumValue": 0,
    "description": "Car speed",
    "units": "",
    "format": "%5.1f",
    "parentParameterGroup": "APP2",
    "frequency": 0,
    "aggregates": "Avg"
  },
  {
    "identifier": "vCar_2Buffer:APP8",
    "name": "vCar_2Buffer",
    "maximumValue": 400,
    "minimumValue": 0,
    "description": "fTAGDisp_vCar_2Buffer",
    "units": "",
    "format": "%5.2f",
    "parentParameterGroup": "APP8",
    "frequency": 0,
    "aggregates": "Avg"
  },
  {
    "identifier": "vCarDemo:APP1",
    "name": "vCarDemo",
    "maximumValue": 400,
    "minimumValue": 0,
    "description": "fTAGDisp_vCarDemo",
    "units": "",
    "format": "%5.2f",
    "parentParameterGroup": "APP1",
    "frequency": 0,
    "aggregates": "Avg"
  },
  {
    "identifier": "vCarEndofMarpleSector1:APP3",
    "name": "vCarEndofMarpleSector1",
    "maximumValue": 20,
    "minimumValue": 0,
    "description": "fTAGDisp_vCarEndofMarpleSector1",
    "units": "",
    "format": "%5.2f",
    "parentParameterGroup": "APP3",
    "frequency": 0,
    "aggregates": "Avg"
  }
]
```

Details  
=======

The ```/sessions/{sessionKey}/details``` endpoint gives you access to a list of **details** available for a specific session.

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/details
```
  
Example  
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/details
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

The ```/sessions/{sessionKey}/laps``` endpoint gives you access to information related to the **laps** of a specific session. 
  
Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/laps
```
  
Example 
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/laps
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
