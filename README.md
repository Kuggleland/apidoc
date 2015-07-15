# Kuggle API documentation
## About
This is a documentation repository which documents the various API endpoints. 

This API supports IPv4 and IPv6, as well as SPDY/2.

## Base URL
* https://api.kuggleland.com

## Versions
Currently there is only 1 version of the API

## Example Call

* https://api.kuggleland.com/1/ (This is the service discovery URL which shows you what endpoints is available)

## Reading Error messages and codes
The API will always return a dictionary / hash type. 

There is a key called *'meta'* which returns two attributes - *'code'* and *'msg'*.

* *'code'* is a http style code (which will match the HTTP code returned)
* *'msg'* is an informational message which is also localized. (Pro-tip: use the Accept-language header)

## Endpoints

* **register** (https://api.kuggleland.com/1/register | Allowed methods: POST) . Accepts a 'phonenumber' or 'phonenumber' and 'pin' or 'fbtoken'. On success it will return a 200 OK response. If a PIN is verified it will return a 'token' which is to be used for identifying yourself with other web service.
** **profile** (https://api.kuggleland.com/1/profile | Allowed methods: GET and POST). Requires authentication. GET will show you the users current profile if it exists. POST lets you create or update a new profile. The accepted fields are 'firstname', 'gender' (0 = female, 1 = male, 2 = other), or 'dob' (eg. 1990-1-1).
** **kredits** (https://api.kuggleland.com/1/kredits | Allowed methods: GET). Requires authentication and a profile. This will show you the current kredits balance for the authenticated user.
