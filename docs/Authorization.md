# ![logo](/docs/branding.bmp) Telemetry Analytics API

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Sessions**](/docs/Sessions.md)<br>
- [**Query Parameters**](/docs/QueryParameters.md)<br>
- [**Views**](/docs/Views.md)<br>

# Authorization

This API uses a Bearer Token Authenticantion. You use your user/password to ask the server for an access token. You then include this access token in the header in all requests to the server.

## Getting a token

To ask for a password you have to use the following endpoint filling up the parameters "username", "password" and "grant_type" in the body of the request:

```
/token
```

"grant_type" is always the same value "password". Next image shows an example of this type of request using [Postman](https://getpostman.com):

<img src="Authorization1.png" alt="drawing" width="80%"/>

Token expires in number of seconds returned in JSON (1 day here).

## Authorization of request

After that you have to copy from previous token response into every request header.

<img src="Authorization2.png" alt="drawing" width="80%"/>


