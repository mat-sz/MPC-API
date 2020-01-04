# Authentication

Authentication can be enabled for the entire back end or certain endpoints. In either case, all endpoints can require authentication except for the authentication and capabilities endpoints.

When not authenticated the server is expected to return a `401 Unauthorized` status code on endpoints that require authentication and a `403 Forbidden` status code on endpoints that the current user isn't allowed to access.

The process itself is as follows:

1. If authentication is available, the client will display a "Sign in" button. The client will first check the [Capabilities](../Capabilities) endpoints for the availiability of Authentication and then will check the [Status](#status) for more detailed information. (If a stored token is available, the client will check whether the token is valid.)
2. Upon clicking on the "Sign in" button, the client will make a [Start](#start-the-authentication-process) request, opening a new window with the parameters from the JSON payload.
3. When the window is closed, the client will [check](#check-state-of-the-authentication-process) whether the authentication process was successful. Some clients may perform the check multiple times. The server is required to store (and serve) the results of the authentication process for at least 300 seconds.

The token must be an alphanumeric string (allowed characters: `A-Z, a-z, 1-9, +, -, /, =, .`), maximum length of this string is 256 characters. The token will be sent with all requests in the `Authorization` header, like this:

```
Authorization: Bearer TOKEN
```

## Status

```
GET /auth
```

### Response

**When no valid token is present:**

```json
{
    "success": true,
    "data": {
        "user": null,
        "authenticationRequired": false,
    }
}
```

**When a valid token is present and the user is authenticated:**

```json
{
    "success": true,
    "data": {
        "user": {
            "name": "User name"
        },
        "tokenExpiration": "2020-01-01T10:00:00.000Z",
        "authenticationRequired": false
    }
}
```

`authenticationRequired` is an indicator field that, when set to `true`, will force the client application to start the authentication process upon connecting to the server.

## Start the authentication process

```
POST /auth
```

### Parameters

You can optionally specify a `redirectUri` that will be used instead of `window.close()`. All servers must support that.

### Response

```json
{
    "success": true,
    "data": {
        "key": "NZqgSgk158dNoLr45hIhXCLFWJ67SgBFzGxfCMACMajgcv65WkD6syPs2nEVp3fy"
    }
}
```

The client is then expected to forward (or open a window/tab) the user to `/auth/continue?key=NZqgSgk158dNoLr45hIhXCLFWJ67SgBFzGxfCMACMajgcv65WkD6syPs2nEVp3fy`. Authentication flow within the window is not standardized by the documentation, the only requirement is for the window to be closed using the `window.close()` JavaScript call.

## Check state of the authentication process

```
GET /auth/state
```

### Parameters

`key` is a required parameter.

### Response

```json
{
    "success": true,
    "data": {
        "state": "successful",
        "token": "x1gKwKSaKNYqLqL9oLjm8hPbZdktp4wn1ijE7ZO3r19gB3jqzFXpBm3MV68yjHIu",
        "tokenExpiration": "2020-01-01T10:00:00.000Z"
    }
}
```

`state` can be one of the following:

* `successful`
* `incomplete`
* `failed`

`token` is required when `state` is equal to `successful`. `tokenExpiration` is optional.

## Renew a token

```
POST /auth/renew
```

This endpoint is used for renewing tokens nearing expiration. The server is supposed to create a new token with a new expiration date or respond with a 401 Unauthorized status code to force the client to redo the authentication process.

### Response

```json
{
    "success": true,
    "data": {
        "token": "Owqa83Yvlzv1R8eh89iHCAoSAkQAVg7ALDv0QYDeFEkGoaMhWCopk4Q0yoTeVDT2",
        "tokenExpiration": "2020-01-07T10:00:00.000Z"
    }
}
```