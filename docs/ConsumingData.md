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


Consuming Data
=====================

URIs for consuming data are grouped under two namespaces: `/data` and `/table/{frequency}/data`. The difference between the two paths is that resources under `/data` works with both InfluxDb and SqlRace while `/table/{frequency}/data` resources currently only support SqlRace as the backing storage.


## Differences between the two namespaces

 - Resources under `/data` namespace gets first-class support from InfluxDb. This means that, if you are using InfluxDb as the backing storage, you gain access to a range of aggregations such as Count, Mean, Median, Sum, First, Last, Min, Max and Stddev during down-sampling. Filtering is done in nanoseconds.
 - Resources under `/data` namespace gives more flexibility in response formatting. You can request data in either Json format or more optimized Csv format, you  can request time in microseconds or nanoseconds. This flexibility may be more useful if your client is web-based.
 - Resources under `/data` namespace allow allow one or multi parameters to be queried at the same time.
 - Resources under `/table/{frequency}/data` namespace enables you to query multiple parameters across multiple sessions at a time.
 - Resources under `/table/{frequency}/data` URIs also give access to some extra resources such as grouped data and time ranges which are currently not available for InfluxDb storage queries.

## Consuming Parameter Data

You can consume parameter data using `/data` endpoint. For resources under `/data` path, two url masks are available for querying data. One to query with the frequency of data specified and one without. Parameter data resource supports content types `application/json` and `text/csv` (default). The The latest one is more optimized for downloading large amounts of parameter data.

### Base url masks

Querying without specifying frequency endpoint:

```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter_1,parameter_2,...,parameter_n}/data
```

### Url parameters

| Parameter name | Description                                                                                         |    Example                              |
|----------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------|
| connection     | Connection friendly name.                                                                           |    SQLRACE01                            |
| sessionKey     | Session Key.                                                                                        |    016fa61e-33e2-7e85-1bc9-4ab56c668136 |
| parameter      | Name(s) of the parameter(s). *Multiple parameters can be requested by separating them with a comma*  |    vCar:Chassis, gLat:Chassis           |
| frequency      | Frequency of the results in Hz.                                                                     |    10                                   |


Query with frequency:

```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter_1;aggregation_1,...,parameter_n;aggregation_n};/{frequency}/data
```

### Url parameters

| Parameter name | Description                                                                                                                  |    Default value    |    Example                              |
|----------------|------------------------------------------------------------------------------------------------------------------------------|---------------------|-----------------------------------------|
| connection     | Connection friendly name.                                                                                                    |                     |    SQLRACE01                            |
| sessionKey     | Session Key.                                                                                                                 |                     |    016fa61e-33e2-7e85-1bc9-4ab56c668136 |
| parameter      | Name(s) of the parameter(s). *Multiple parameters can be requested by separating them with a comma*                           |                     |    vCar:Chassis, gLat:Chassis           |
| aggregation    | Optional aggregation function separated by a semicolon `;`. *Do not add semicolon if you are not specifying an aggregation.* |    `mean`           |    ;max                                 |
| frequency      | Frequency of the results in Hz.                                                                                              |                     |    10                                   |



### Optional parameters for both base urls

| Parameter name | Description                                                                                                                         |    Default value    |    Example                               |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------|---------------------|------------------------------------------|
| from           | Filters data in session by time in microseconds or nanoseconds. All data returned would have a time stamp after the specified time. |    `null`           |    1546347600                            |
| to             | Filters data in session by time in microseconds or nanoseconds. All data returned would have a time stamp before the specified time.|    `null`           |    1546348200                            |
| lap            | Filters data for specified lap in session.                                                                                          |    `null`           |    3                                     |
| filter         | Generic filter expression describing filters.                                                                                       |    `null`           |    vCar:Chassis;ge;300                   |
| page           | Index of page returned in result (0 is first page)                                                                                  |      0              |    3                                     |
| pageSize       | Size of one page.                                                                                                                   |     200             |    50                                    |
| timeUnit       | Units of time. Use `Ns` for nanoseconds and `Ms` for microseconds.                                                                  |      `Ns`           |    `Ms`                                  |
| groupBy        | Groups queries by fields (Only supported for InfluxDb storage). Use `*` to group by all available fields.                           |     `null`          |    `groupBy=lapnumber&groupBy=sessionId` |


#### Query Parameter Data

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter_1,parameter_2,...,parameter_n}/data
```

Example
```
GET api/v1/connections/Connection/sessions/64192714-90fe-4865-a191-a6887e465184/parameters/vCar:Chassis,gLat:Chassis/data?from=1549534712000000000&to=1549534712500000000groupBy=*
```

Csv Result:
```
time,tagValues,vCar:Chassis,gLat:Chassis
1549534712000000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.2,0.025
1549534712100000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,0.762,0.01
1549534712200000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.214,-0.02
1549534712300000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,3.044,-0.015
1549534712400000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,2.701,0.005
1549534712500000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.355,NaN
```

Json Result:
```
{
  "parametersWithTags": 
  {
    "vCar:Chassis": 
    [
      {
        "timestamp": 1549534712000000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "Topic"
        },
        "value": 1.2
      },
      {
        "timestamp": 1549534712100000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "Topic"
        },
        "value": 0.762
      },
      {
        "timestamp": 1549534712200000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "Topic"
        },
        "value": 1.214
      },
      {
        "timestamp": 1549534712300000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "Topic"
        },
        "value": 3.044
      },
      {
        "timestamp": 1549534712400000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "Topic"
        },
        "value": 2.701
      },
      {
        "timestamp": 1549534712500000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "Topic"
        },
        "value": 1.355
      }
    ],
    "gLat:Chassis": 
    [
      {
        "timestamp": 1549534712000000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "TestTopic"
        },
        "value": 0.025
      },
      {
        "timestamp": 1549534712100000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "TestTopic"
        },
        "value": 0.01
      },
      {
        "timestamp": 1549534712200000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "TestTopic"
        },
        "value": -0.02
      },
            {
        "timestamp": 1549534712300000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "TestTopic"
        },
        "value": -0.015
      },
      {
        "timestamp": 1549534712400000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "TestTopic"
        },
        "value": 0.005
      },
      {
        "timestamp": 1549534712500000000,
        "tag": {
          "lapnumber": "1l",
          "sessionId": "64192714-90fe-4865-a191-a6887e465184",
          "topic": "TestTopic"
        },
        "value": "NaN"
      }
    ]
  }
}
```

#### Query Parameter Data Frequency

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter_1;aggregation_1,parameter_2;aggregation_2,...,parameter_n;aggregation_n}/{frequency}/data
```

Example
```
GET api/v1/connections/Connection/sessions/64192714-90fe-4865-a191-a6887e465184/parameters/vCar:Chassis;mean/10/data?from=1549534712000000000&to=1549534713300000000&filter=vCar;gt;100,vCar;le;150
```

Csv Result:
```
time,tagValues,vCar:Chassis
1549534712000000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.2
1549534712100000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,0.762
1549534712200000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.214
1549534712300000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,3.044
1549534712400000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,2.701
1549534712500000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.355
1549534712600000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,0.07
1549534712700000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.506
1549534712800000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,2.069
1549534712900000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,1.421
1549534713000000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,0.005
1549534713100000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,0.416
1549534713200000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,0.81
1549534713300000000,lapnumber=1l;sessionId=64192714-90fe-4865-a191-a6887e465184;topic=TestTopic,0.275
```

Json Result:
```
{
  "timestamps": 
  [
    1549534712000000000,
    1549534712100000000,
    1549534712200000000,
    1549534712300000000,
    1549534712400000000,
    1549534712500000000,
    1549534712600000000,
    1549534712700000000,
    1549534712800000000,
    1549534712900000000,
    1549534713000000000,
    1549534713100000000,
    1549534713200000000,
    1549534713300000000
  ],
  "parametersWithTags":
  {
    "Mean(vCar:Chassis)": [
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 1.2
      },
      {
        "tag": {      
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 0.762
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 1.214
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 3.044
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 2.701
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 1.355
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 0.07
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 1.506
      },{
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 2.069
      }
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 1.421
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 0.005
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 0.416
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 0.81
      },
      {
        "tag": {
            "lapnumber": "1l",
            "sessionId": "64192714-90fe-4865-a191-a6887e465184",
            "topic": "TestTopic"
        },
        "value": 0.275
      }
    ],
  "startTime": 1549534712000000000,
  "endTime": 1549534713300000000,
  "totalCount": 14
  }
}
```

#### Query Parameter Data Count

This functionality is either available for `/data` namespace with and without frequency. 

Mask
```
GET api/{apiVersion}/connections/{connection name}/sessions/{sessionKey}/parameters/{parameter};{aggregation}/{frequency}/data/count
```

Example
```
GET api/v1/connections/Connection/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar:Chassis;mean,gLat:Chassis;mean/10/data/count?from=1549534714876000000&to=1549572500653000000
```

Result
```
[
    {
        "name":"Mean(vCar:Chassis)",
        "count":6236.0
    },
    {
        "name":"Mean(gLat:Chassis)",
        "count":6291.0
    }
]
```

#### Aggregate Parameter Data over Tags.

If your backing storage is InfluxDb, you can aggregate parameter data over InfluxDb tags.

Mask
```
GET api/v1/connections/{connections}/sessions/{sessionKey}/parameters/{parameter};{aggregation}/data/aggregate?groupBy={tag1}&groupBy={tag2}
```

Example
```
GET api/v1/connections/Connection/sessions/64192714-90fe-4865-a191-a6887e465184/parameters/vCar:Chassis;mean,gLat:Chassis;mean/data/aggregate?groupBy=lapnumber
```

Csv Result
```
time,tagValues,vCar:Chassis,gLat:Chassis
1549534711876000000,lapnumber=1l,112.335688215992,0.18400058029484376
1549534711876000000,lapnumber=2l,199.965857988166,-0.82674907292954236
1549534711876000000,lapnumber=3l,225.134068751761,0.087585425383542262
1549534711876000000,lapnumber=4l,191.609365859654,0.2036408483625701
1549534711876000000,lapnumber=5l,209.018854306413,0.26592772152238897
1549534711876000000,lapnumber=6l,179.251327235393,0.25245055569364194
1549534711876000000,lapnumber=7l,186.329971860711,0.52127658846863922
```

Json Result
```
{
  "timestamps": [
    1549534711876000000,
    1549534711876000000,
    1549534711876000000,
    1549534711876000000,
    1549534711876000000,
    1549534711876000000,
    1549534711876000000
  ],
  "parametersWithTags": {
    "Mean(vCar:Chassis)": [
      {
        "tag": {
          "lapnumber": "1l"
        },
        "value": 112.335688215992
      },
      {
        "tag": {
          "lapnumber": "2l"
        },
        "value": 199.96585798816574
      },
      {
        "tag": {
          "lapnumber": "3l"
        },
        "value": 225.13406875176085
      },
      {
        "tag": {
          "lapnumber": "4l"
        },
        "value": 191.60936585965354
      },
      {
        "tag": {
          "lapnumber": "5l"
        },
        "value": 209.01885430641281
      },
      {
        "tag": {
          "lapnumber": "6l"
        },
        "value": 179.25132723539306
      },
      {
        "tag": {
          "lapnumber": "7l"
        },
        "value": 186.32997186071083
      }
    ],
    ,
    "Mean(gLat:Chassis)": [
      {
        "tag": {
          "lapnumber": "1l"
        },
        "value": 0.18400058029484376
      },
      {
        "tag": {
          "lapnumber": "2l"
        },
        "value": -0.82674907292954236
      },
      {
        "tag": {
          "lapnumber": "3l"
        },
        "value": 0.087585425383542262
      },
      {
        "tag": {
          "lapnumber": "4l"
        },
        "value": 0.2036408483625701
      },
      {
        "tag": {
          "lapnumber": "5l"
        },
        "value": 0.26592772152238897
      },
      {
        "tag": {
          "lapnumber": "6l"
        },
        "value": 0.25245055569364194
      },
      {
        "tag": {
          "lapnumber": "7l"
        },
        "value": 0.52127658846863922
      }
    ]
  }
}
```

## Parameters Table

You can consume parameter table data using `/table/{frequency}/data` endpoint.

**Currently not supported by InfluxDb.**

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

The ```/table/{frequency}/data/grouped``` endpoint will return data grouped by time ranges. Note that you can use all the parameters and filters used in the rest of endpoints.

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

Filter is an optional parameter that we can use to filter when consuming data. There are multiple types of filters.

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

Structure of one parameter filter is:

Parameter filter url mask
```
{parameterName};{filterOperationShortcut};{value}
```

Parameter filter example
```
vCar:Chassis;gt;300
```

Example of filtering values that have vCar between 100 and 150 kph. 

Filter setting example
```
vCar:Chassis;gt;100,vCar:Chassis;le;150
```

Note that some paremeters have a colon ( : ) in, so we use semicolons ( ; ) for the filtering.


### More on querying multiple sessions table data

All data queries can be done over multiple sessions for SqlRace data (InfluxDb data support for this feature is in the roadmap).

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
