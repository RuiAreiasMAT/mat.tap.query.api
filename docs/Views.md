# Views

A user can specify the view of parameters and then use it to query parameters data without specifying all parameters every time. API exposes
CRUD API for views.

### Get all views

```
GET example
GET api/views
```
```
Result
```
```json

 [
    {
        "Id": 2,
        "Name": "TestView",
        "Description": "Some test parameters."
    }
]
```
#### Query view parameters

```
Request
GET api/views/2/parameters
```
```
Result
```
```json
[
    "NGear:Chassis",
    "vCar:Chassis"
]
```
### Add new view

```
Request
POST api/views
```

```
Body
```
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

```
Request
PUT api/views/2/parameters
```
```
Body
```
```json
[
    "NGear:Chassis",
    "gLat:Chassis"
]
```

```
Result
```
```json
[
    "NGear:Chassis",
  	"gLat:Chassis",
    "vCar:Chassis"
]
```
### Delete view parameters

```
Request
DELETE api/views/2/parameters
```
```
Body
[
    "NGear:Chassis"
]
```
```
Result
```
```json
[
  	"gLat:Chassis",
    "vCar:Chassis"
]
```
## Query parameters using view

```
Example
api/connections/SQLRACE01/sessions/26c9ed85-e7ab-0e3f-0fc0-8a69f7743883/vi
ew/test4/10/data
```
