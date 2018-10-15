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

### Get all views

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
### Query view parameters

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


## Query view parameters

API exposes a CRUD API for views. You can use this CRUD in order to create, modify or delete "views".

### Add new view

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
### Edit view

```
Request
PUT api/views/
```
```
Body
```
```json
{
	"Id" : 2,
    "Name": "TestView",
    "Description": "Some test parameters."
}
```
### Add view parameters

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
### Delete view parameters

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
## Consuming Data using a view

Example

```
GET api/connections/SQLRACE01/sessions/26c9ed85-e7ab-0e3f-0fc0-8a69f7743883/view/test4/10/data
```

(Result example here)
