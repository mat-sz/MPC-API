# Capabilities

These endpoints provide knowledge of server's supported functionality to the client. **Everything described in this section MUST BE implemented by all servers unless specified otherwise.**

## About

```GET /about```

### Response

```json
{
    "data": {
        "name": "MPC-API Server",
        "description": "A description.",
        "version": 0,
        "supportedFeatures": [
            "authentication",
            "connected_devices",
            "playback",
            "playlists",
            "search",
            "settings",
            "websockets"
        ]
    }
}
```

## Icon

*(not required)*

```GET /icon.png```

If present must be a PNG file, served with the `image/png` MIME type and the PNG image data as the response payload. The aspect ratio must be 1:1, resolution should be at least 128x128px.