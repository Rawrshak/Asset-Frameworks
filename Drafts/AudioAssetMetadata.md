---
title: Audio Asset Metadata
status: For Review
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-30
updated: 2022-03-02
version: 1.0
---

# Audio Asset Metadata

## Simple Summary

The Audio Asset Metadata framework creates a guide for Audio-based asset tokens, with use-cases such as sound effects, character lines, and background music.

## Abstract

The Audio Asset framework defines a metadata standard for audio-based assets and audio asset use-cases. The audio metadata standard is stored in the `properties` property in the Public Asset Metadata.


The metadata standard allows game developers to easily parse metadata for audio asset tokens for audio use in-game. This allows players to unlock audio assets from one game or world and use them wherever it is applicable.

Audio asset tokens can be used in many different manners such as rewarding players with short sound effects, character lines, background music, shouts, quotes, etc. Players and content creators may also create their own audio asset tokens that they can bring into their games to show their unique assets. 

## Terminology 

* `content type` - the metadata used to indicate the original media type of the resource (prior to any content encoding applied for sending or storage)

## Specification 

We define the Audio-based asset frameworks for different in-game use cases. The `subtypes` are hints towards game developers on how to load and where to use the audio assets brought to their game by players. Developers are free to interpret the framework and use the incoming audio assets as they see if, but players expect the certain uses for their audio assets. 

Some `subtypes` may have similar requirements but players generally expect to use them in different ways based on the specified use-case. 

Unity has great flexibility on which audio assets should be kept in memory at all times. The game developer should determine how best to load and store these assets in their own game to optimize memory usage. 

### Supported Formats

These formats are supported on the Windows, Android, iOS, and WebGL platforms.

Format | Extension
------ | ------
MPEG3 Layer 3 | .mp3
Microsoft Wave | .wav

### Sound Effect Subtype

The sound effect subtype requires that the audio file has a maximum duration of 2 second. We recommend Sound Effects assets should be kept in memory as they can be attached to any in-game asset.

### Shouts Subtype

The Shout subtype requires that the audio file has a maximum duration of 2 second. Shout assets should be kept in memory as they can be attached to the player (other players or characters) in game.

### Character Line Subtype

The character lines subtype requires that the audio file has a maximum duration of 10 seconds. Character lines assets are optional to keep in memory. The game developer may allow players to say these lines at any time or may only have specific locations where they can be loaded and played. These may be loaded on demand as necessary. 

It is up to the game developer on where to load these assets and how to keep them in memory.

### Background Music Subtype

The background music subtype requires that the audio file has a maximum duration of 300 seconds. Background Music assets are loaded per gamer and not shared during gameplay. They may also be loaded gradually or on demand. They shouldn't be kept in memory.

### Custom Subtype

The custom subtype doesn't have a maximum duration for the audio. The creator is free to experiment with different durations and audio file types (media file types) for their game.The custom subtype, however, may not be loadable in other games aside from the creator's game. It is up to each game developer whether or not to support custom assets from other games.

### Schema 
```
{
    "title": "audioProperties",
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
        "uri": {
            "type": "string",
            "description": "uri to the downloadable audio file"
        },
        "contentType": {
            "type": "string",
            "description": "[Optional] content type. Can be audio/mpeg, audio/wav"
        },
        "duration": {
            "type": "int",
            "description": "[Optional] duration of the audio clip in milliseconds"
        }
    }
}
```

## Rationale

For the audio asset metadata, the developer can add as many audio asset properties to the audio asset as they need. This provides the game developer a choice of which audio file to use. Each `audio subtype` has its own specific requirements so that game developers know how to properly load assets and when/how to use them in their game. 

Each audio subtype has different max duration requirements in order to guarantee optimal memory usage for each subtype. The sound effects and shout subtypes should be kept in memory when loaded in game so the gamers may use them whenever possible. The character lines and background music subtype should be loaded on demand and the developer may choose when to load them in-game. 

`duration` and `contentType` are optional and is used to give developers hints on how to load an asset. Some creators may opt to leave it out or it may be incorrect due to malicious or negligence. 

Once the metadata is loaded, the developer can choose which of the `audioProperties` to use if there is more than one.

This audio metadata standard is simple and contains only the most basic required information for an audio asset.


## Samples

### Sound Effect Subtype
```
{
    "name": "Rawrshak Unlock Sound Effect",
    "description": "Sound effect whenever a user unlocks a Rawrshak NFT",
    "image": "https://arweave.net/eAnYl_kEpDtYQA8240b0naau5PP4czOFP1qGg4zazQQ",
    "tags": [
        "rawrshak",
        "sound effect",
        "x-collab"
    ],
    "type": "audio",
    "subtype": "sound-effect",
    "nsfw": "false",
    "properties":
    {
        "audioProperties": 
        [
            {
                "uri": "arweave.net/TsBCV3HyMssIZQk0S7MqZht_zR3e9pBXDWUo0VYAXW4"
            },
            {
                "uri": "arweave.net/TsBCV3HyMssIZQk0S7MqZht_zR3e9pBXDWUo0VYAXW4",
                "contentType": "audio/mp3",
                "duration": "500",
            }
        ],
        "creatorComments": "Unique Rawrshak Ping!"
    }
}
```

### Background Music Subtype
```
{
    "name": "Rawrshak Opening Theme",
    "description": "Rawrshak Opening Song - Jump into the Madness",
    "image": "https://arweave.net/eAnYl_kEpDtYQA8240b0naau5PP4czOFP1qGg4zazQQ",
    "tags": [
        "rawrshak",
        "bgm",
        "summer collection"
    ],
    "type": "audio",
    "subtype": "background-music",
    "nsfw": "false",
    "properties":
    {
        "audioProperties": 
        [
            {
                "uri": "arweave.net/TsBCV3HyMssIZQk0S7MqZht_zR3e9pBXDWUo0VYAXW4",
                "contentType": "audio/wav",
                "duration": "90000",
            },
            {
                "uri": "arweave.net/dI_5bBKqfDwLUHhQXu_ubnnBi5f3DcpHQ_oHmIky1QU"
            }
        ],
        "creatorComments": "Some opening song that introduces Rawrshak"
    }
}
```