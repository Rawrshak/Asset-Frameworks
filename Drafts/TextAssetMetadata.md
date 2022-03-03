---
title: Text Asset Metadata
status: For Review
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-24
updated: 2022-03-02
version: 1.0
---

# Text Asset Metadata

## Simple Summary

The Text Asset Metadata framework creates a guide for text-based asset tokens, with use-cases such as Player Titles and Game Lore.

## Abstract

The Text Asset framework defines a metadata standard for text-based assets and text asset use-cases. The text metadata standard is stored in the `properties` property in the Public Asset Metadata. 

Text asset tokens can be used to convey in-game achievements, in-game lore, secret instructions, and other in-game content. Gamers acquire these asset tokens through gameplay or purchase. Game developers can use these text assets across games. It enables future, yet-to-be-created usage such as keys or crafting material for future content. 

## Terminology 

* `Achievements` - Milestones or rewards in a player's in-game progress. 
* `Title` - A text-based asset reward used as player accolades.
* `Lore` - Additional in-game information about characters, places, and other in-game content.

## Specification 

We define the three Text subtypes, each hinting different usage. The `subtypes` are hints towards game developers on how to import assets into their game. The developers are free to interpret and use incoming gamer assets for their own game's purposes. Game developers expect the asset data to adhere to a `type` and `subtype`'s data requirements (if any). Assets may be filtered out by games if they violate these requirements.

### Title Subtype
```
{
    "title": "textProperties",
    "type": "object",
    "properties": {
        "title": {
            "type": "string",
            "description": "Name of the Reward. Titles can be used by players to show off their achievements. Titles are used by players to boast their achievements and cement unique their status. The title is limited to 40 characters."
        },
        "description": {
            "type": "string",
            "description": "Description of the asset which this token represents. It adds a deeper description of the asset to differentiate the Title. The description requires a max character limit of 500."
        }
    }
}
```

### Lore Subtype
```
{
    "title": "textProperties",
    "type": "object",
    "properties": {
        "title": {
            "type": "string",
            "description": "Name of the Lore. The lore name is limited to 40 characters."
        },
        "description": {
            "type": "string",
            "description": "Description of the asset which this token represents. This can be used to add more background information about a character, a place, an event, or other in-game content. This is used to build a richer world through collectibles and unlockable lore. The description requires a max character limit of 5000."
        }
    }
}
```

### Custom Subtype
```
{
    "title": "textProperties",
    "type": "object",
    "properties": {
        "title": {
            "type": "string",
            "description": "Name of the Custom text asset. No character limit."
        },
        "description": {
            "type": "string",
            "description": "Description of the asset which this token represents. Content creators can create any text based content and does not have a max character limit. Custom assets may not be supported by many games and applications in the ecosystem."
        }
    }
}
```

## Rationale

For Text-based assets, there are only two properties: `title` and `description`. Not to be confused for the `name` and `description` properties in the main metadata object, `title` and `description` inside the `textProperties` object are the properties that should appear in-game when this asset is loaded by the user. If any of these are null or undefined, a game developer should default to using the `name` and `description` from the main metadata object.

The character limits for the `title` and `description` are there so that game developers may allocate the appropriate amount of space in-game when these assets are loaded. Keep in mind that text-based assets violate the asset type or subtype requirements, in this case character limits, will not be loaded by the in-game libraries and thus players won't be able to use them. 

The `custom` subtype is used for developers who wish to use text assets with no character limits. These may not be supported by ecosystem developers, aside from the developer's own game, due to unpredictable character sizes. It is up to the individual game developers on whether to support custom assets. Developers can also impose profanity filters so be aware that the assets may not be loaded for reasons other than character limits.

Text asset tokens data is plain-text and should not contain code or specially formatted. 

## Samples

### Title Subtype
```
{
    "name": "Rawrshak Alpha User Title",
    "description": "Title bestowed to the earliest community members.",
    "image": "https://arweave.net/1f_wbiCIxcAi_WU3tLjcW0mEs5mZtGW_iKx7_fHWFnw",
    "tags": [
        "rawrshak",
        "title",
        "early rawrshak supporter"
    ],
    "type": "text",
    "subtype": "title",
    "nsfw": "false",
    "properties":
    {
        "textProperties":
        {
            "title": "Rawrshak Alpha User",
            "description": "Rawrshak's Alpha participant."
        },
        "creatorComments": "These are the earliest users of the Rawrshak project!"
    }
}
```

### Lore Subtype
```
{
    "name": "Rawrshak's Lore",
    "description": "Some special Rawrshak Lore",
    "image": "https://arweave.net/1f_wbiCIxcAi_WU3tLjcW0mEs5mZtGW_iKx7_fHWFnw",
    "tags": [
        "rawrshak",
        "lore",
        "early rawrshak supporter"
    ],
    "type": "text",
    "subtype": "title",
    "nsfw": "false",
    "properties":
    {
        "textProperties":
        {
            "title": "Rawrshak's Lore",
            "description": "Something something, deep Rawrshak Lore.",
        },
        "creatorComments": "This asset lore should only go to the early users!"
    }
}
```