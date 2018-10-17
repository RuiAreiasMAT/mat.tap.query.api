# ![logo](/docs/branding.bmp) Telemetry Analytics API

### Table of Contents
- [**Introduction**](/README.md)<br>
- [**Getting started**](/docs/GettingStarted.md)<br>
- [**Authorization**](/docs/Authorization.md)<br>
- [**Querying Metadata**](/docs/Metadata.md)<br>
- [**Consuming Data**](/docs/ConsumingData.md)<br>
- [**Views**](/docs/Views.md)<br>

# Authorization

This API uses a Token authentication. You use your user/password to ask the server for an access token, then you can include this access token in the header in all requests to the server.

## Getting a token

To ask for a password you have to use the following endpoint filling up the parameters ```username```, ```password``` and ```grant_type``` in the body of the request. Field ```grant_type``` is always the same value **"password"**:

```
GET /token
```

Following image shows an example of this type of request:

<img src="Authorization1.png" alt="drawing" width="80%"/>

Field ```access_token``` of the result gives you the bearer token that you want to use. Token expires in the number of seconds returned in JSON (1 day here).

## Authorization of request

After getting a token from the server you have to use it in each request of the API putting it as a parameter in the Header of each call. The parameter name to use is ```Authorization``` and the value is keyword **"bearer"** followed by a space and the **Token** string.

<img src="Authorization2.png" alt="drawing" width="80%"/>


