---
title: Audio Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-30
updated: 2022-01-31
version: 0.4
---

# Audio Asset Metadata

## Simple Summary

The Audio Asset Metadata framework creates a common guideline for Audio-based NFTs such as sound effects, character lines, and background music between games.

## Abstract

The Audio Asset Metadata framework is `assetProperties` object in the Public Asset Metadata. This contains the information for Audio-based NFTs. This creates a common metadata format so that game developers can easily parse metadata for audio NFTs for usage in-game. This allows players to unlocked audio assets from one game or world and use them wherever it is applicable.

Audio NFTs can be used in many different manners such as rewarding players with short sound effects, character lines, background music, shouts, quotes, etc. Players and content creators may also create their own audio NFTs that they can bring into their games to show their unique content. 

## Terminology 

* `content type` - the metadata used to indicate the original media type of the resource (prior to any content encoding applied for sending or storage)

## Specification 

We define the Audio-based NFT frameworks for different audio duration. However, these `subtypes` are merely suggestions when it comes to usage. The developers are free to use the frameworks for different purposes as intended as long as they adhere to the specific requirements of each subtype. 

Note: these audio duration requirements are not final and can be changed upon community game developer / content creator review. These are merely starting points for discussion proposed by Rawrshak.

Note: Unity has great flexibility on which audio assets should be kept in memory at all times. The game developer should honor the requirements for the framework in order to optimize memory usage. 

### Supported Formats
Format | Extension
------ | ------
MPEG3 Layer 3 | .mp3
Microsoft Wave | .wav

### Sound Effect Subtype

The sound effect subtype requires that the audio file has a maximum duration of 1 second. Sound Effects NFTs should be kept in memory as they can be attached to any in-game asset.

### Shouts Subtype

The Shout subtype requires that the audio file has a maximum duration of 2 second. Shout NFTs should be kept in memory as they can be used by the player in game at any moment.

### Character Line Subtype

The character lines subtype requires that the audio file has a maximum duration of 10 seconds. Character lines NFTs are optional to keep in memory. The game developer may allow players to say these lines at any time or may only have specific locations where they can be loaded and played. These may be loaded on demand as necessary.

### Background Music Subtype

The background music subtype requires that the audio file has a maximum duration of 300 seconds. Background Music assets are loaded per gamer and not shared during gameplay. They may also be loaded gradually or on demand. They shouldn't be kept in memory.

### Custom Subtype

The custom subtype doesn't have a maximum duration for the audio. The creator is free to choose what they want to use in their games and NFT. The custom subtype, however, may not be loadable in other games aside from the creator's game. It is up to the game developer whether or not to support custom assets from other games.

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
        "name": {
            "type": "string",
            "description": "name of the audio file - optional"
        },
        "uri": {
            "type": "string",
            "description": "uri to the downloadable audio file"
        },
        "contentType": {
            "type": "string",
            "description": "content type. Can be audio/mpeg, audio/wav"
        },
        "duration": {
            "type": "int",
            "description": "duration of the audio clip in milliseconds"
        }
    }
}
```

## Rationale

For the audio asset metadata, the developer can add as many audio asset properties to the NFT as they need so it provides the game developer a choice of which audio file to use. Each `audio` subtype has its own specific requirements so that game developers know how to properly load assets and when/how to use them in their game. The specific requirements for each audio subtype is `duration` and load type. The sound effects and shout subtypes should be kept in memory when loaded in game so the gamers may use them whenever possible. The character lines and background music subtype should be loaded on demand and the developer may choose when to load them in-game. Limiting `duration` for each subtype specifies each subtype's use case and limits the asset file size.

Audio data such as `duration` is specific information for the game developer for loading the audio clip, which may or may not be used.

Once the metadata is loaded, the developer can choose which of the `assetProperties` to use if there is more than one.

This metadata framework draft is simple and contains only the most basic required information for an audio asset. It may be updated later on, more information such as channelCount and sampleRate may be added. Feedback from game developers and sound engineers may be necessary to determine additional information that is necessary.


## Samples

### Sound Effect Subtype
```
{
    "name": "Rawrshak Unlock Sound Effect",
    "description": "Sound effect whenever a user unlocks a Rawrshak NFT",
    "image": "<default text icon uri on arweave>",
    "tags": [
        "Rawrshak",
        "Sound Effect",
        "X-Collab"
    ],
    "type": "audio",
    "subtype": "sound-effect",
    "nsfw": "false",
    "assetProperties": 
    [
        {
            "name": "unlockEffect.wav",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "duration": "500",
        },
        {
            "name": "unlockEffectWav",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "duration": "500",
        },
        {
            "name": "unlockEffectMp3",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/mp3",
            "duration": "500",
        }
    ],
    "devProperties":
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
    "tags": [
        "Rawrshak",
        "BGM",
        "Summer Collection"
    ],
    "type": "audio",
    "subtype": "background-music",
    "nsfw": "false",
    "assetProperties": 
    [
        {
            "name": "openingTheme.wav",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "duration": "90000",
        },
        {
            "name": "openingThemeWav",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/wav",
            "duration": "90000",
        },
        {
            "name": "openingThemeMp3",
            "uri": "arweave.net/<transaction-id>",
            "contentType": "audio/mp3",
            "duration": "90000",
        }
    ],
    "devProperties":
    {
        "creatorComments": "Some opening song that introduces Rawrshak"
    }
}
```