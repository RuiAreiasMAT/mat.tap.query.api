# ![logo](/docs/branding.bmp) Telemetry Analytics API

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Querying Metadata**](/docs/Metadata.md)<br>
- [**Consuming Data**](/docs/ConsumingData.md)<br>
- [**Views**](/docs/Views.md)<br>


# Views

A user can specify the view of parameters and then use it to query parameters data without specifying all parameters every time. 

## Get all views

Example
```
GET api/views
```
Result
```json

 [
    {
        "Id": 2,
        "Name": "TestView",
        "Description": "Some test parameters."
    }
]
```
## Query view parameters

Request
```
GET api/views/2/parameters
```

Result
```json
[
    "NGear:Chassis",
    "vCar:Chassis"
]
```


# CRUD API for Views

API exposes a CRUD API for views. You can use this CRUD in order to create, modify or delete "views".

## Add new view

Request
```
POST api/views
```

Body
```json
{
    "Name": "TestView",
    "Description": "Some test parameters."
}
```
## Edit view

Request
```
PUT api/views/
```
Body
```json
{
    "Id" : 2,
    "Name": "TestView",
    "Description": "Some test parameters."
}
```

## Delete view

Request
```
DELETE api/views/2
```

## Add view parameters

Request
```
PUT api/views/2/parameters
```

Body
```json
[
    "NGear:Chassis",
    "gLat:Chassis"
]
```

Result
```json
[
    "NGear:Chassis",
    "gLat:Chassis",
    "vCar:Chassis"
]
```
## Delete view parameters

Request
```
DELETE api/views/2/parameters
```
Body
```
[
    "NGear:Chassis"
]
```
Result
```json
[
    "gLat:Chassis",
    "vCar:Chassis"
]
```
# Consuming Data using a View

(High level explanation)

Example
```
GET api/connections/SQLRACE01/sessions/26c9ed85-e7ab-0e3f-0fc0-8a69f7743883/view/test4/10/data
```

Result
```
(Result example here)
```

## Multiple sessions query

All data views and parameters specifing is available with multi session
query.

Example
```
GET api/connections/SQLRACE01/sessions
/26c9ed85-e7ab-0e3f-0fc0-8a69f7743883,0095fa36-a3cb-1dc0-59c0-df620ed271db,21c9e13c-bed0-b738-b067-33e1b67433ea,6a463017-933c-9e2a-99d7-8e0d9f1d6049,92cbbae9-cd2c-aff8-f071-856bc116405a
/view/test4/10/data/count?filter=vCar:Chassis;gt;300
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

