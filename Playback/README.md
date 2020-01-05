# Playback

These methods provide information about playback and allow for playback control.

## Status

```
GET /playback
```

### Response

```json
{
    "success": true,
    "data": {
        "playback": {
            "active": true,
            "progress": 100000,
            "duration": 200000
        },
        "track": {
            "id": "123",
            "name": "Track Name",
            "artistName": "Artist",
            "duration": 200000
        },
        "album": {
            "id": "345",
            "name": "Album Name"
        },
        "playlist": {
            "id": "456",
            "name": "Playlist Name"
        }
    }
}
```

## Skip to the next track

```
POST /playback/next
```

### Response

```json
{
    "success": true
}
```

## Skip to the previous track

```
POST /playback/previous
```

### Response

```json
{
    "success": true
}
```