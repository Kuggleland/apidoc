# Kuggle API documentation
![kugglejs](https://cloud.githubusercontent.com/assets/83470/8816445/7bcd2826-306a-11e5-96e2-59728b2845ba.png)

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
* **purposes** (https://api.kuggleland.com/1/purpose | Allowed methods: GET and POST). Requires authentication and a profile. Doing a GET will show the current users last purposes. Required parameters: 'lat' and 'lng' OR 'placeid', 'purpose', and 'type' (Either 'goal', 'help', or 'give')
* **purpose liking** (https://api.kuggleland.com/1/purpose/PURPOSEID/like | Allowed methods: POST). Requires authentication and a profile. No parameters needed.
* **purpose disliking** (https://api.kuggleland.com/1/purpose/PURPOSEID/dislike | Allowed methods: POST). Requires authentication and a profile. No parameters needed.
* **nearby people** (https://api.kuggleland.com/1/nearby/people | Allowed methods: GET). Requires: 'lat' and 'lng' and optionally a 'distance' in meters which defaults to 5000 if left out.
* **nearby places** (https://api.kuggleland.com/1/nearby/places | Allowed methods: GET). Requires: 'lat' and 'lng' and optionally a 'distance' in meters which defaults to 5000 if left out.
* **inbox** (https://api.kuggleland.com/1/messages | Allowed methods: GET and POST). Requires Authentication and a user profile. For GET, no parameters needed. Will return list of users which sent the current user a message. For POST, the parameters required is 'to' (recipients user identifier), 'messagetype' (currently only 'text' is supported), and 'text'.
* **messages from a sender** (https://api.kuggleland.com/1/messages/senderid | Allowed methods: GET). Requires Authentication and a user profile. No parameters is needed. This will return ALL the unread messages from a particular sender. 

## Example - User registration
The below is a typical user journey from unauthenticated to authenticated.

* Make a POST call to the register endpoint (either a 'fbtoken' or 'phonenumber'
* If 'phonenumber', then make an additional  POST call to the register endpoint with a 'pin'
* Once you get a token, make a GET call to the profile endpoint to get the persons profile.
* If the person is already known it will show the profile where you can display it in the user interface, otherwise you will need to make an additional call (this time POST) to profile with the 'firstname', 'gender', and 'dob' (YYYY-MM-DD format). If you registered from facebook, the register endpoint should show this and automatically do the profile setup and linking so this step is skipped.
* Once the profile is set up, the access token provided now has full access to the other endpoints on the system. 
