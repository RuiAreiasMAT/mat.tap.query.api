# ![logo](/docs/branding.bmp) Telemetry Analytics API

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Sessions**](/docs/Sessions.md)<br>
- [**Query Parameters**](/docs/QueryParameters.md)<br>
- [**Views**](/docs/Views.md)<br>

# Authorization

## Getting a token

grant_type is always same (password). Token expires in number of seconds returned in JSON (1 day here).
For user name and password use the values on this confluence page:
https://confluence.mclaren.com/display/MATMDSG/Username+and+Password

<img src="Authorization1.png" alt="drawing" width="80%"/>

## Authorization of request

Copy token from previous token response into every request header.

<img src="Authorization2.png" alt="drawing" width="80%"/>


