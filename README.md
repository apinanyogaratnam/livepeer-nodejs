# Livepeer Node

Livepeer API wrapper for Node projects, supporting for streams and sessions.

The **Stream** is the core building block of the livepeer.com platform. A livepeer.com stream is a unique object with configuration data and metadata about all live stream sessions associated with it.

The **Session** is a single live streaming session. It is an especially important Livepeer.com API object if need to reference recorded live stream sessions.

This library is intended to provide Livepeer API convenience methods for applications written in server-side Javascript.
Please note that this package uses Livepeer API keys and is intended to be used in server-side code only.

Not familiar with Livepeer? Check out https://livepeer.org/ for more information.

## Documentation

See the [Livepeer docs](https://livepeer.org/docs)

## Installation

```
npm install @livepeer/livepeer-node --save
```

or

```
yarn add @livepeer/livepeer-node
```

## Usage

To start, you will need to get API key for Livepeer. For more information on where to get an API key, visit https://livepeer.com/app/user/keys 


Require the `@livepeer/livepeer-node` npm module and create a Livepeer instance. Your Livepeer instance will have some properties such as Stream and Session that allow you to access the Stream and Session APIs.

```javascript
const Livepeer = require(‘@livepeer/livepeer-node’);
const livepeerObject = new Livepeer(api_key);
```

### Stream

The **stream** is the core building block of the livepeer.com platform. A livepeer.com stream is a unique object with configuration data and metadata about all live stream sessions associated with it.

#### Stream Object

| Property  | Description |
| ------------- | ------------- |
| name  | Additional identifier for the asset. Often set to a human readable string. This identifier does not need to be unique  |
| profiles  | Transcoding rendition settings. The source will be delivered with the renditions in the HLS and does not need to be redefined in the profiles parameter. If no profiles are defined, only the source will be delivered for playback  |
| id  | Unique identifier for the stream.   |
| createdAt  | Timestamp when the asset was created. Reported in Unix epoch time.   |
| streamKey  | Unique secret key used to form the RTMP ingest URL. ex: RTMP ingest URL - rtmp://rtmp.livepeer.com/live  |
| playbackId  | Unique identifier used to form the playback URL. ex: Playback URL - https://cdn.livepeer.com/hls/{playbackId}/index.m3u8   |
| ...  | Other object keys.  |

#### Create a stream

Create a stream with name and profiles.

```javascript
const stream = await livepeerObject.Stream.create(
    {
    “name”: “test_stream”, 
    “profiles”: [
        {
            “name”: “720p”,
            “bitrate”: 2000000,
            “fps”: 30,
            “width”: 1280,
            “height”: 720
        },
        {
            “name”: “480p”,
            “bitrate”: 1000000,
            “fps”: 30,
            “width”: 854
            “height”: 480
        },
        {
            “name”: “36p”,
            “bitrate”: 500000,
            “fps”: 30,
            “width”: 640,
            “height”: 360
        },
    ]
    }
);
```

#### Retrieve all streams

Get all streams with the same user (owner of the api_key)

```javascript
const streams = await livepeerObject.Stream.getAll(streamOnly = 0, isActive = false, record = false);
```
| Parameter  | Description |
| ------------- | ------------- |
| streamOnly | If 1, exclude historic stream objects |
| isActive | If true, get only active stream |
| record | If true, get only recorded on stream |

#### Retrieve a stream using id

```javascript
const stream = await livepeerObject.Stream.get(id);
```

#### Turn on/off recording

Turn on/off recording for streams, but not for session or historic stream object (representing a single live stream)

```javascript
const result = await stream.setRecord(0/1);
```

### Session

The **session** is a single live streaming session. It is an especially important Livepeer.com API object if need to reference recorded live stream sessions.
