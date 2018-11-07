# ![logo](/docs/branding.bmp) Telemetry Analytics API

![Build Status](https://mat-ocs.visualstudio.com/Telemetry%20Analytics%20Platform/_apis/build/status/MAT.TAP.TelemetryAnalytics.API/MAT.TAP.TelemetryAnalytics.API%20-%20Pull%20Request%20Gateway?branchName=develop)

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Installation**](/docs/Installation.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Identity Server**](/docs/IdentityServer.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Querying Metadata**](/docs/Metadata.md)<br>
- [**Consuming Data**](/docs/ConsumingData.md)<br>
- [**Views**](/docs/Views.md)<br>


Consuming Data
=====================

URIs for consuming data is grouped under two namespaces: `/data` and `/table/{frequency}/data`. The difference between the two paths is that resources under `/data` works with both InfluxDb and SqlRace while `/table/{frequency}/data` resources are currently only supports SqlRace as the backing storage.

The main difference in the functionality of the two endpoints is that you can query multipe parameters over multiple sessions at a time with `/table/{frequency}/data`. You can specifiy parameters in the URL or using views (See [Views](/docs/Views.md)). On the other hand, if the backing storage is InfluxDb, you can only query using one parameter at a time. Support for multiple parameters is in the roadmap. `/table/{frequency}/data` URIs also give access to some extra resources such as grouped data and time ranges which are currently not available for InfluxDb storage queries.

## Consuming Parameter Data

You can consume parameter data using `/data` endpoint. For resources under `/data` path, two url masks are available for querying data. One to query with the frequency of data specified and one without. 

### Base url masks

Query with frequency:

```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter}/{frequency}/data
```

And querying without specifying frequency:

```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter}/data
```

### Url parameters

| Parameter name | Description                                                                                         | Default value | Example     |
|----------------|-----------------------------------------------------------------------------------------------------|---------------|-------------|
| connection     | Connection friendly name.   |               | SQLRACE01                                  |
| sessionKey     | Session Key.               |               | 016fa61e-33e2-7e85-1bc9-4ab56c668136       |
| parameter      | Name of the parameter.  |               | vCar   |
| frequency      | Frequency of the results in Hz.  |               | 10            |

### Optional parameters

| Parameter name | Description                                                                                         | Default value | Example     |
|----------------|-----------------------------------------------------------------------------------------------------|---------------|-------------|
| from           | Filters data in session by time. All data returned would have time stamp after specified time.      |    `null`     | 10:34       |
| to             | Filters data in session by time. All data returned would have time stamp before specified time.     |    `null`     | 10:50       |
| lap            | Filters data for specified lap in session.                                                          |    `null`     | 3           |
| filter         | Generic filter expression describing filters.                                                       |    `null`     | value;ge;300 |
| page           | Index of page returned in result (0 is first page)                                                  |      0        | 3           |
| pageSize       | Size of one page.                                                                                   |     200       | 50          |


#### Query Parameter Data

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter}/{frequency}/data
```

Example
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/10/data?from=11:15&to=11:20&filter=value;gt;100,value;le;150
```

Result
```json
    {
    "time": "14:50:51.4000000",
    "value": 279.50857142857143
  },
  {
    "time": "14:50:51.5000000",
    "value": 280.149
  },
  {
    "time": "14:50:51.6000000",
    "value": 280.981
  },
  {
    "time": "14:50:51.7000000",
    "value": 281.859
  },
  {
    "time": "14:50:51.8000000",
    "value": 282.61400000000003
  },
  {
    "time": "14:50:51.9000000",
    "value": 283.427
  },
  {
    "time": "14:50:52",
    "value": 284.20899999999995
  }
```

#### Query Parameter Data Count

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameterName}/{frequency}/data/count
```

Example
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/10/data/count?from=11:15&to=11:20&filter=value;gt;100,value;le;150
```

Result
```
5646
```

## Parameters Table

You can consume parameter table data using `/table/{frequency}/data` endpoint.

### Base url mask

```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey_1,...,sessionKey_n}/parameters/{parameter_1,...,parameter_n}/table/{frequency}/data
```

#### Query Parameter Table Data

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey_1,...,sessionKey_n}/parameters/{parameter_1,...,parameter_n}/table/{frequency}/data
```

Example
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c,83ceba51-83d1-43ec-bb0a-10cdv685ca47c/parameters/vCar,BDownshiftRequested/10/data?from=11:15&to=11:20&filter=vCar;gt;100,vCar;le;150
```

#### Query Parameter Table Data Count

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey_1,...,sessionKey_n}/parameters/{parameterName_1,...,parameterName_n}/table/{frequency}/data/count
```

Example
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c,83ceba51-83d1-43ec-bb0a-10cdv685ca47c/parameters/vCar,BDownshiftRequested/10/data/count?from=11:15&to=11:20&filter=vCar;gt;100,vCar;le;150
```

#### Query Time ranges

The ```/table/{frequency}/data/timeranges``` endpoint returns time ranges of result samples. Note that you can use all the parameters and filters used in the rest of endpoints.

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey_1,...,sessionKey_n}/parameters/{parameter1,parameter2,...,parameter_n}/table/{frequency}/data/timeranges
```

Example
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/table/10/data/timeranges?from=11:15&to=11:20&filter=vCar;gt;100,vCar;le;150
```

Result
```json
    {
        "StartTime": "11:19:41.1650000",
        "EndTime": "11:19:45.0550000",
        "Span": "00:00:03.8900000"
    },
    {
        "StartTime": "11:19:33.8350000",
        "EndTime": "11:19:35.1250000",
        "Span": "00:00:01.2900000"
    },
    {
        "StartTime": "11:19:28.6250000",
        "EndTime": "11:19:29.7050000",
        "Span": "00:00:01.0800000"
    },
    {
        "StartTime": "11:19:08.6650000",
        "EndTime": "11:19:09.8150000",
        "Span": "00:00:01.1500000"
    },
    {
        "StartTime": "11:18:51.1550000",
        "EndTime": "11:18:54.6950000",
        "Span": "00:00:03.5400000"
    }
```    

#### Grouped data

The ```/table/{frequency}/data/grouped``` endpoint will return data grouped to time ranges. Note that you can use all the parameters and filters used in the rest of endpoints.

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey1,...,sessionKey_n}/parameters/{parameter1,parameter2,...,parameter_n}/table/{frequency}/data/grouped
```

Example
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/table/10/data/grouped?from=11:15&to=11:20&filter=vCar;gt;100,vCar;le;150
```

Result
```json
{
        "StartTime": "11:19:41.1650000",
        "EndTime": "11:19:45.0550000",
        "Span": "00:00:03.8900000",
        "Values": [
            {
                "Time": "11:15:17.1750000",
                "Values": {
                    "vCar": 149.97
                }
            },
            {
                "Time": "11:15:17.1650000",
                "Values": {
                    "vCar": 149.18
                }
            },
            {
                "Time": "11:15:17.1550000",
                "Values": {
                    "vCar": 149.17
                }
            },
            {
                "Time": "11:15:17.1450000",
                "Values": {
                    "vCar": 149.67
                }
            },
            {
                "Time": "11:15:17.1350000",
                "Values": {
                    "vCar": 149.66
                }
            }
		]
}
```

### More on filtering

Filter is an optional parameter that we can use to filter when consuming data. There are multiple types of filters could be specified.

#### Filter types

| Shortcut | Full name             |
|----------|-----------------------|
| eq       | Equal                 |
| gt       | Greater than          |
| ge       | Greater than or equal |
| lt       | Less than             |
| le       | Less than or equal    |
| ne       | Not equal             |

#### Parameter filter

Structure of one parameter filter instance is:

Parameter filter url mask
```
{parameterName};{filterOperationShortcut};{value}
```

Parameter filter example
```
vCar;gt;300
```

Example of filtering values that have vCar between 100 and 150 kph. 

Filter setting example
```
vCar:Chassis;gt;100,vCar:Chassis;le;150
```

Note that some paremeters have a colon ( : ) in, so we use semicolons ( ; ) for the filtering.


### More on querying multiple sessions table data

All data queries could be done over multiple sessions for SqlRace data (InfluxDb data support for this feature is in the roadmap).

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey1,sessionKey2,...,sessionKeyN}/parameters/{parameter1,parameter2,...,parameter_n}/table/{frequency}/data
```

Example
```
GET api/v1/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c,asdsad199-16155a-43ec-bb0a-12sadsa23a/parameters/vCar/table/10/data?from=11:15&to=11:20&filter=vCar:Chassis;gt;100,vCar:Chassis;le;150
```

Result
```json
[
    {
        "Key": "e5d51619-13df-4cb8-b263-940be28aae8f",
        "Value": [
            {
                "StartTime": "12:18:54.6550000",
                "EndTime": "12:18:57.7550000",
                "Span": "00:00:03.1000000"
            },
            {
                "StartTime": "12:21:09.7550000",
                "EndTime": "12:21:15.2550000",
                "Span": "00:00:05.5000000"
            },
            {
                "StartTime": "12:23:20.4550000",
                "EndTime": "12:23:25.7550000",
                "Span": "00:00:05.3000000"
            },
            {
                "StartTime": "12:25:41.1550000",
                "EndTime": "12:25:47.4550000",
                "Span": "00:00:06.3000000"
            },
            {
                "StartTime": "12:27:55.4550000",
                "EndTime": "12:28:01.5550000",
                "Span": "00:00:06.1000000"
            },
            {
                "StartTime": "12:30:04.9550000",
                "EndTime": "12:30:08.5550000",
                "Span": "00:00:03.6000000"
            }
        ]
    },
    {
        "Key": "fd309247-66db-41cf-87b9-4760902f3981",
        "Value": [
            {
                "StartTime": "11:32:21.2970000",
                "EndTime": "11:32:25.0970000",
                "Span": "00:00:03.8000000"
            },
            {
                "StartTime": "11:33:43.6970000",
                "EndTime": "11:33:47.7970000",
                "Span": "00:00:04.1000000"
            },
            {
                "StartTime": "11:37:45.8970000",
                "EndTime": "11:37:49.2970000",
                "Span": "00:00:03.4000000"
            },
            {
                "StartTime": "11:39:42.8970000",
                "EndTime": "11:39:46.4970000",
                "Span": "00:00:03.6000000"
            },
            {
                "StartTime": "11:42:09.2970000",
                "EndTime": "11:42:13.4970000",
                "Span": "00:00:04.2000000"
            },
            {
                "StartTime": "11:43:27.3970000",
                "EndTime": "11:43:31.4970000",
                "Span": "00:00:04.1000000"
            },
            {
                "StartTime": "11:45:36.6970000",
                "EndTime": "11:45:41.0970000",
                "Span": "00:00:04.4000000"
            },
            {
                "StartTime": "11:48:21.7970000",
                "EndTime": "11:48:26.8970000",
                "Span": "00:00:05.1000000"
            },
            {
                "StartTime": "11:50:41.2970000",
                "EndTime": "11:50:44.3970000",
                "Span": "00:00:03.1000000"
            }
        ]
    }
]
```
