# MPC-API

**The documentation is not complete yet.** I am working on it while testing ideas out on a reference implementation, so things may and will change until I'm 100% sure everything works like it should.

This repository contains a description of the Media Playback Control API. The API is designed to allow the reuse of one media player front end across multiple media players (with the use of plugins or node.js-based servers to control them remotely). Requests and responses are made using JSON payloads, with base64-encoded images (and other binary files) as string contents of given fields.

The functionality of this API is subject to change without notice until it's finalized.

[**Capabilities**](Capabilities) is the only functionality that must be implemented on the server side, others are not required. Clients must support everything allowed by the API.

## Table of Contents

- [Capabilities](Capabilities)<sup>*</sup>
- [Authentication](Authentication)<sup>+</sup>
- [Playback (Queue)](Playback)<sup>+</sup>
- [WebSockets](WebSockets)<sup>+</sup>
- [Connected Devices](Connected%20Devices)
- [Playlists](Playlists)
- [Search](Search)
- [Settings](Settings)

<sup>* - required</sup>

<sup>+ - suggested</sup>

## Current Version

**0**, using the prefix `/api/v0`.

## JSON structure

All JSON responses are structured in the following way:

- A `data` key that contains the response itself.
- An `error` key that contains an error.

Either a `data` key or an `error` key must be present. If none (or both) is present the response is erroneous and should be discarded.

## Errors

If an error occurs, the server is expected to return a JSON object that contains an `error` property, with a 4xx or 5xx HTTP status code. When no errors occur the server must return a 200 OK status code.

The server should return a 501 Not Implemented status code for endpoints that aren't supported by the server but are listed in the documentation.

### Error response

```json
{
    "error": {
        "statusCode": 401,
        "message": "Authorization required."
    }
}
```

## CORS

Servers must send the following headers to allow for direct client to server communication:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: POST, GET, OPTIONS, PUT, DELETE
Access-Control-Allow-Credentials: false
Access-Control-Max-Age: 86400
Access-Control-Allow-Headers: Content-Type, Authorization, X-Requested-With
```

Preflight (OPTIONS) requests must be supported as well. [Authentication](Authentication) support is suggested to prevent unauthorized cross-site requests.

## HTTPS

HTTPS (and WSS for WebSockets) is recommended, but not required. This may change in future versions of the API.