---
title: Audio Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-30
updated: 2021-08-30
version: 0.1
---

# Audio Asset Metadata

## Simple Summary

The Audio Asset Metadata framework creates a common guideline for Audio-based NFTs such as sound effects, character lines, and background music between games.

## Abstract

The Audio Asset Metadata framework is `assetProperties` object in the Public Asset Metadata. This contains the information for Audio-based NFTs. This creates a common metadata format so that game developers can easily parse metadata for audio NFTs for usage in-game. This allows players to unlocked audio assets from one game or world and use them wherever it is applicable.

Audio NFTs can be used in many different manners such as rewarding players with short sound effects, character lines, background music, shouts, quotes, etc. Players and content creators may also create their own audio NFTs that they can bring into their games to show their unique content. 

## Terminology 

* `audio compression` - the reduction of certain types of audio data in order to shrink the file size.
* `audio channels` - an audio channel refers to the independent audio signal which is collected or playback when a sound is recorded or played back in different spacial position. Mono refers to an audio file with only 1 channel. Stereo is the reproduction of sound using two or more independent audio channels in a way that creates the impression of sound coming from various directions.
* `sample-rate` - sound waves are converted into data by taking a series of snapshot measurements, called samples. This information is then converted into binary data. The sample rate is the number of samples taken in an audio file per second. 44.1 kHz is the standard. 44.1 kHz refers to 44,100 samples per second in the audio file.

## Specification 

We define the Audio-based NFT frameworks for different audio duration. However, these `subtypes` are merely suggestions when it comes to usage. The developers are free to use the frameworks for different purposes as intended as long as they adhere to the specific requirements of each subtype. 

Note: these audio duration requirements are not final and can be changed upon community game developer / content creator review. These are merely starting points for discussion proposed by Rawrshak.

Note: Unity has great flexibility on which audio assets should be kept in memory at all times. The game developer should honor the requirements for the framework in order to optimize memory usage. 

Note: Each game must know what engine and platform they are running on. They must know which audio package to download and load for their game.

### Supported Formats
Format | Extension
------ | ------
MPEG3 Layer 3 | .mp3
Ogg Vorbis | .ogg
Microsoft Wave | .wav
Audio Interchangable File Format | .aiff / .aif

### Sound Effect Subtype

The sound effect subtype requires that the audio file has a maximum duration of 1 second. Sound Effects NFTs should be kept in memory as they can be attached to any in-game asset.

### Shouts Subtype

The Shout subtype requires that the audio file has a maximum duration of 2 second. Shout NFTs should be kept in memory as they can be used by the player in game at any moment.

### Character Line Subtype

The character lines subtype requires that the audio file has a maximum duration of 10 seconds. Character lines NFTs are optional to keep in memory. The game developer may allow players to say these lines at any time or may only have specific locations where they can be loaded and played. These may be loaded on demand as necessary.

### Background Music Subtype

The background music subtype requires that the audio file has a maximum duration of 300 seconds. Background Music assets are loaded per gamer and not shared during gameplay. They may also be loaded gradually or on demand. They shouldn't be kept in memory.

### Schema 
```
{
    "title": "Asset Properties",
    "type": "object",
    "properties": {
        "audio": {
            "type": "object",
            "description": "An array of texture objects"
        }
    }
}

{
    "title": "Audio",
    "type": "object",
    "properties": {
        "engine": {
            "type": "string",
            "description": "engine which this package supports. Values include none, unity, and unreal. none refers to the raw, uncompressed, unpackaged file for use by dapps."
        },
        "compression": {
            "type": "string",
            "description": "specifies the compression algorithm used when the audio file was packaged. For Unity, these values can be PCM, ADPCM, or Compressed. Compressed is the default common value."
        },
        "uri": {
            "type": "string",
            "description": "uri to the downloadable file or audio package"
        },
        "contentType": {
            "type": "string",
            "description": "content type. Can be audio/mpeg, audio/wav, audio/ogg, audio/x-aiff"
        },
        "durationMs": {
            "type": "int",
            "description": "duration of the audio clip in milliseconds"
        },
        "channelCount": {
            "type": "int",
            "description": "channel count of the audio clip"
        },
        "sampleRateHz": {
            "type": "int",
            "description-hz": "sample rate of the audio clip in hz"
        },
    }
}
```

## Rationale

For the audio asset metadata, the developer can add as many audio packages to the NFT as they need. Each `audio` subtype has its own specific requirements so that game developers know how to properly load assets and when/how to use them in their game. The specific requirements for each audio subtype is `duration` and load type. The sound effects and shout subtypes should be kept in memory when loaded in game so the gamers may use them whenever possible. The character lines and background music subtype should be loaded on demand and the developer may choose when to load them in-game. Limiting `duration` for each subtype specifies each subtype's use case and limits the asset file size.

Other audio data such as `durationMs`, `channelCount`, and `sampleRateHz` is specific information for the game developer for loading the audio clip.

The `engine` and `compression` allows the developer to filter assets and determine which file best to load. If the game developer cannot load any of the files, the NFT will not be loaded. The developer may also opt to add an unpackaged version (`engine` = `none`, `compression` = `raw`) that can be used by front-ends to preview an asset. These unpackaged version isn't packaged for any specific game engine.

For this draft, we propose the following default audio type per game engine that an NFT should support. Each Audio NFT should have the following audio package and Rawrshak-enabled games should support it.

#### Unity
```
{
    "engine": "unity",
    "compression": "compressed"
    "uri": "arweave.net/<transaction-id>",
    "contentType": "audio/wav",
    "durationMs": 1000,
    "channelCount": 1,
    "sampleRateHz": 44100
}
```

For the Unity engine, the default compression should be 'compressed', default file extension should be '.wav', and the default sample rate should be 44.1 kHz (standard). 

## Samples

### Sound Effect Subtype
```
{
    "name": "Rawrshak Unlock Sound Effect",
    "description": "Sound effect whenever a user unlocks a Rawrshak NFT",
    "image": "<default text icon uri on arweave>",
    "type": "audio",
    "subtype": "sound-effect",
    "assetProperties": 
    [
        {
            "engine": "none",
            "compression": "raw",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "durationMs": "500",
            "channelCount": 1,
            "sampleRateHz": 44100
        },
        {
            "engine": "unity",
            "compression": "compressed",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "durationMs": "500",
            "channelCount": 1,
            "sampleRateHz": 44100
        },
        {
            "engine": "unity",
            "compression": "compressed",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/mp3",
            "durationMs": "500",
            "channelCount": 2,
            "sampleRateHz": 48000
        }
    ],
    "developer-properties":
    {
        "creatorComments": "Unique Rawrshak Ping!"
    }
}
```

### Background Music Subtype
```
{
    "name": "Rawrshak Opening Theme",
    "description": "Rawrshak Opening Song - Jump into the Madness",
    "image": "<default text icon uri on arweave>",
    "type": "audio",
    "subtype": "background-music",
    "assetProperties": 
    [
        {
            "engine": "none",
            "compression": "raw",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "durationMs": "90000",
            "channelCount": 2,
            "sampleRateHz": 44100
        },
        {
            "engine": "unity",
            "compression": "compressed",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "durationMs": "90000",
            "channelCount": 2,
            "sampleRateHz": 44100
        },
        {
            "engine": "unity",
            "compression": "compressed",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/mp3",
            "durationMs": "90000",
            "channelCount": 2,
            "sampleRateHz": 44100
        }
    ],
    "developer-properties":
    {
        "creatorComments": "Some opening song that introduces Rawrshak"
    }
}
```