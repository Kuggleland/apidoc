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

## Headers
There are two headers which you can pass:
* *'Token'*: This is how you identify yourself as a valid user - for protected endpoints
* *'Accept-language'*: This is where you define the language. Refer to RFC1766 on how its used. All modern web browsers pass this header.


## Reading Error messages and codes
The API will always return a dictionary / hash type. 

There is a key called *'meta'* which returns two attributes - *'code'* and *'msg'*.

* *'code'* is a http style code (which will match the HTTP code returned)
* *'msg'* is an informational message which is also localized. (Pro-tip: use the Accept-language header)

## Endpoints

* **register** (https://api.kuggleland.com/1/register | Allowed methods: POST) . Accepts a 'phonenumber' or 'phonenumber' and 'pin' or 'fbtoken'. On success it will return a 200 OK response. If a PIN is verified it will return a 'token' which is to be used for identifying yourself with other web service. If FB token is used, in addition to 'token', there is another attribute called 'fromfb' which contains all the information that is grabbed from facebook
* **profile** (https://api.kuggleland.com/1/profile | Allowed methods: GET and POST). Requires authentication. GET will show you the users current profile if it exists (this method will also link any tokens to an existing profile too). POST lets you create or update a new profile. The accepted fields are 'firstname', 'gender' (0 = female, 1 = male, 2 = other), or 'dob' (eg. 1990-01-01).
* **kredits** (https://api.kuggleland.com/1/kredits | Allowed methods: GET). Requires authentication and a profile. This will show you the current kredits balance for the authenticated user.

## Example - User registration
The below is a typical user journey from unauthenticated to authenticated.

* Make a POST call to the register endpoint (either a 'fbtoken' or 'phonenumber'
* If 'phonenumber', then make an additional  POST call to the register endpoint with a 'pin'
* Once you get a token, make a GET call to the profile endpoint to get the persons profile.
* If the person is already known it will show the profile where you can display it in the user interface, otherwise you will need to make an additional call (this time POST) to profile with the 'firstname', 'gender', and 'dob' (YYYY-MM-DD format). If you registered from facebook, the register endpoint should show this. Why? We'd like to get users to be able update their info just in case facebook data is not correct or incomplete.
* Once the profile is set up, the access token provided now has full access to the other endpoints on the system.
