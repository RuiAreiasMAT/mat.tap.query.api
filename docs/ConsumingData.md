# ![logo](/docs/branding.bmp) Telemetry Analytics API

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Querying Metadata**](/docs/Metadata.md)<br>
- [**Consuming Data**](/docs/ConsumingData.md)<br>
- [**Views**](/docs/Views.md)<br>


Consuming Data
=====================

There are multiple ways how to consume data and multiple ways how to
filter them. Parameters could be specified in URL or by a view. (See
[Views](/docs/Views.md)).

Optional parameters
-------------------

| Parameter name | Description                                                                                         | Default value | Example     |
|----------------|-----------------------------------------------------------------------------------------------------|---------------|-------------|
| from           | This filter data in session by time. All data returned would have time stamp after specified time.  |               | 10:34       |
| to             | This filter data in session by time. All data returned would have time stamp before specified time. |               | 10:50       |
| lap            | This filter data for specified lap in session.                                                      |               | 3           |
| filter         | Define parameters filter here.                                                                      |               | vCar;ge;300 |
| page           | Index of page returned in result (0 is first page)                                                  |               | 3           |
| pageSize       | Size of one page.                                                                                   | 200           | 50          |

Number of samples
-----------------


Easiest, fastest and probably best way to start is a query that returns
a number of samples that fit all filters. Note: instead of parameters
you can use [Views](https://confluence.mclaren.com/display/MATMDSG/Views).

Mask

```
GET api/connections/{connection friendly name}/sessions/{sessionKey}/parameters/{parameter1,parameter2,...,parameter_n}/{frequency}/data/count
```

Example

```
GET api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/10/data/count?from=11:15&amp;to=11:20&amp;filter=vCar;gt;100,vCar;le;150
```

Result

```
5646
```

Time ranges
-----------

This type of view would return time ranges of result samples.

Mask

```
GET api/connections/{connection friendly name}/sessions/{sessionKey}/parameters/{parameter1,parameter2,...,parameter_n}/{frequency}/data/timeRanges
```

Example

```
GET api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/10/data/timeRanges?from=11:15&amp;to=11:20&amp;filter=vCar;gt;100,vCar;le;150
```

Code Block 6 Result

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

Grouped data
------------

This view will return data grouped to time ranges.

Mask

```
GET api/connections/{connection friendly name}/sessions/{sessionKey}/parameters/{parameter1,parameter2,...,parameter_n}/{frequency}/data/grouped
```

Example

```
GET api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/10/data/grouped?from=11:15&amp;to=11:20&amp;filter=vCar;gt;100,vCar;le;150
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

Flat data
---------

This view will return all samples flat across all time ranges. This view
of data is paged. Default page size is 200 samples.

Mask

```
GET api/connections/{connection friendly name}/sessions/{sessionKey}/parameters/{parameter1,parameter2,...,parameter_n}/{frequency}/data
```

Note that some signals have a colon ( : ) in, so we use semicolons ( ; )
for the filtering.

Example

```
GET api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c/parameters/vCar/10/data?from=11:15&amp;to=11:20&amp;filter=vCar;gt;100,vCar;le;150
```

Result

```json
    {
        "Time": "11:19:45.0450000",
        "Values": {
            "vCar": 149.88
        }
    },
    {
        "Time": "11:19:45.0350000",
        "Values": {
            "vCar": 149.28
        }
    },
    {
        "Time": "11:19:45.0250000",
        "Values": {
            "vCar": 149.45
        }
    },
    {
        "Time": "11:19:45.0150000",
        "Values": {
            "vCar": 149.3
        }
    },
    {
        "Time": "11:19:45.0050000",
        "Values": {
            "vCar": 148.35
        }
    }
```

Filter
------

The filter is optional parameter where multiple types of filters could
be specified.

### Filter types

| Shortcut | Full name             |
|----------|-----------------------|
| eq       | Equal                 |
| gt       | Greater than          |
| ge       | Greater than or equal |
| lt       | Less than             |
| le       | Less than or equal    |
| ne       | Not equal             |

### Parameter filter

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

Multiple sessions data
----------------------

All data queries could be done over multiple sessions.

Mask

```
api/connections/{connection friendly name}/sessions/{sessionKey1,sessionKey2,...,sessionKeyN}
/parameters/{parameter1,parameter2,...,parameter_n}/{frequency}/data
```

Example

```
api/connections/M800960/sessions/92ce7a51-83d1-43ec-bb0a-9cda685ca47c,asdsad199-16155a-43ec-bb0a-12sadsa23a/parameters/vCar/10/data?from=11:15&amp;to=11:20&amp;filter=vCar:gt:100,vCar:le:150
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

Other views
-----------

All data views and parameters specifing is available with multi session
query.

Example
```
GET api/connections/SQLRACE01/sessions/26c9ed85-e7ab-0e3f-0fc0-8a69f7743883,0095fa36-a3cb-1dc0-59c0-df620ed271db,21c9e13c-bed0-b738-b067-33e1b67433ea,6a463017-933c-9e2a-99d7-8e0d9f1d6049,92cbbae9-cd2c-aff8-f071-856bc116405a/view/test4/10/data/count?filter=vCar:Chassis;gt;300
```

Result
```json
{
    "26c9ed85-e7ab-0e3f-0fc0-8a69f7743883": 0,
    "0095fa36-a3cb-1dc0-59c0-df620ed271db": 210,
    "21c9e13c-bed0-b738-b067-33e1b67433ea": 0,
    "6a463017-933c-9e2a-99d7-8e0d9f1d6049": 0,
    "92cbbae9-cd2c-aff8-f071-856bc116405a": 0
}
```
