# REST API

(High level Introduction)

## Getting started

1. For querying the server it is recommended (but not essential) to use Postman.
		 a. Download and install it if you don't have it.
		 b. You don't need an account

2. Get a password.
		a. For an example of this see the Authorization page (but change the username and password)
		b. For those who are not familiar with this, you use your password to ask the server for an access token. You then include this access token in the header in all requests to the server.


## Connections

Query API could be connected to multiple sources of data.

```code
api/connections
```
```json
Result
[
{
"FriendlyName": "Season2017",
"ServerName": "SqlRace-RESTAPI\\SQLEXPRESS",
"IsSqlite": false,
"DatabaseName": "SQLRACE01",
"Identifier": "Season2017",
"RootPath": "C:\\SQLRace_Filestream"
},
{
"FriendlyName": "Live Session Cache",
"ServerName": "DbEngine=SQLite;Data
Source=C:\\Users\\AdminRestApi\\AppData\\Local\\McLaren Applied
Technologies\\ATLAS 10\\SQL Race\\LiveSessionCache.ssn2;PRAGMA
journal_mode=WAL;",
"IsSqlite": true,
"DatabaseName": "C:\\Users\\AdminRestApi\\AppData\\Local\\McLaren
Applied Technologies\\ATLAS 10\\SQL Race\\LiveSessionCache.ssn2",
"Identifier": "Live Session Cache",
"RootPath": null
}
]
```
## Query connection

Every connection provides a list of sessions.


```
Mask
api/connections/{connection friendly name}/sessions
```
```
Example
api/connections/Simulator/sessions
```
Paging is supported by this query. Page and page size could be specified. Default page size is 50 sessions in one page.

```
Paging
api/connections/Simulator/sessions?page=1&pageSize=
```
```json
Result
{
"Key": {
"value": "67ad8a93-6ba7-4c5d-9947-9c0e963ccf9e"
},
"Identifier": "171023220758",
"TimeOfRecording": "2017-10-23T22:07:58.3646176",
"TimeZone": "UTC",
"SessionType": "SessionType",
"StartTime": "00:00:00",
"EndTime": "00:00:00",
"LapsCount": 0,
"Id": 1301,
"State": 2
},
{
"Key": {
"value": "0661d026-271d-4f2a-a255-b977438a10fe"
},
"Identifier": "171023220700",
"TimeOfRecording": "2017-10-23T22:07:00.0236738",
"TimeZone": "UTC",
"SessionType": "SessionType",
"StartTime": "00:00:00",
"EndTime": "00:00:00",
"LapsCount": 0,
"Id": 1296,
"State": 2
}
```
