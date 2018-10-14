# Authorization

## Getting a token

grant_type is always same (password). Token expires in number of seconds returned in JSON (1 day here).
For user name and password use the values on this confluence page:
https://confluence.mclaren.com/display/MATMDSG/Username+and+Password

![Authorization 1](Authorization1.png | width=80)

## Authorization of request

Copy token from previous token response into every request header.

![Authorization 2](Authorization2.png | width=80)


