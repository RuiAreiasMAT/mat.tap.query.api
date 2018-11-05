# ![logo](/docs/branding.bmp) Telemetry Analytics API

![Build Status](https://mat-ocs.visualstudio.com/Telemetry%20Analytics%20Platform/_apis/build/status/MAT.TAP.TelemetryAnalytics.API/MAT.TAP.TelemetryAnalytics.API%20-%20Pull%20Request%20Gateway?branchName=develop)

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Querying Metadata**](/docs/Metadata.md)<br>
- [**Consuming Data**](/docs/ConsumingData.md)<br>
- [**Views**](/docs/Views.md)<br>


Querying Parameter Data
========

There are several resources available to query telemetry data. In this section we are going to explain the different endpoints available and what information we can obtain from them.

Base route for all parameter data queries is `api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameter}`.

Required parameters  
-------------------  
  
| Parameter name | Description
|----------------|-------------------------------------------------------------| 
| connectionName | Identifier of the SqlRace or InfluxDb connection. |  
| sessionKey | Unique identifier of the session. |  
| parameter | Name of the parameter for which you like to query data for. |

Optional parameters  
-------------------  
  
| Parameter name | Description | Default value | Example |  
|----------------|-------------|---------------|---------|   
| from | Earliest data point timestamp. | `null` | identifier;eq;test|measurementName;eq;testMeasurement |
| to | Latest data point timestamp. | `null` | identifier;eq;test|measurementName;eq;testMeasurement |
| lap | Lap number. | `null` | 5 |
| page | Index of page returned in result | 0 | 2 |
| pageSize | Size of one page. | 200 | 50 |
| filter | Expression specifying filters to use. | `null` | identifier;eq;test|measurementName;eq;testMeasurement |

Data
=====

There are two resources to obtain parameter data: one with frequency of data specified and one without.

With frequency:

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameter}/{frequency:long}/data
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis/50/data
```
For InfluxDb data you can only query one parameter at a time at the moment. However, with SqlRace you may query more than one parameter at a time by using the `/table/{frequency:long}` route:

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameters}/table/{frequency:long}/data
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis,vCar:Chassis/table/50/data
```

Without frequency:

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameter}/data
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis/data
```

Data Count
==========

API resources for data count allows you to get the total parameter count for a specific parameter under various filters. Similar to the resources for data, you have the option to specify the frequency.

With frequency:

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameter}/{frequency:long}/data/count
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis/50/data/count
```

For InfluxDb data you can only query one parameter at a time at the moment. However, with SqlRace you may query more than one parameter at a time by using the `/table/{frequency:long}` route:

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameters}/table/{frequency:long}/data/count
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis,vCar:Chassis/table/50/data/count
```

Without frequency:

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameter}/data/count
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis/data/count
```

For SqlRace storage, you have a few extra resources to group parameter data as well as query time ranges. Furthermore, you can query more than one parameter at a time.

Grouped Data
============

`data/grouped` queries grouped parameters.

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameters}/table/{frequency:long}/data/grouped
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis,vCar:Chassis/table/50/data/grouped
```

Time Range Data
============

`data/timeranges` queries time range of the data for parameters specified.

Mask
```
GET api/connections/{connectionName}/sessions/{sessionKey}/parameters/{parameters}/table/{frequency:long}/data/timeranges
```

Example  
```
GET api/connections/Season2017/sessions/28d4e3a5-c0aa-4a87-8513-bfdfdf78e3a2/parameters/BDownshiftRequested:Chassis,vCar:Chassis/table/50/data/timeranges
```