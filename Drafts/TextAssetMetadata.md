---
title: Text Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-24
updated: 2021-08-30
version: 0.1
---

# Text Asset Metadata

## Simple Summary

The Text Asset Metadata framework creates a common guideline for text-based NFTs such as Titles and Lore between games.

## Abstract

The Text Asset Metadata framework is `asset-properties` object in the Public Asset Metadata. This contains the information for Text-based NFTs such as Titles and Lore. Text NFTs are used to convey in-game achievements and unlockable information about in-game content. Gamers are able to acquire these assets through gameplay or purchase. These NFTs may be used as keys or materials for future content such as unlockable content or asset crafting. 

## Terminology 

* `asset-properties` - This is the object in the public asset metadata containing the Text NFT data.
* `Achievements` - These are trophies meant to reward a player's in-game progress. 
* `Title` - A text-based NFT reward used as accolades.
* `Lore` - Additional in-game information about characters or other in-game content.

## Specification 

We define the three Text-based NFT frameworks for different usage. However, these `subtypes` are merely guides when it comes to usage. The developers are free to use the frameworks for different purposes as intended as long as they adhere to the specific requirements of each subtype so that they can be loaded by games in the Rawrshak ecosystem. 

### Title Subtype
```
{
    "title": "Asset Properties",
    "type": "object",
    "properties": {
        "title": {
            "type": "string",
            "description": "Name of the Reward. Titles can be used by players to show off their achievements. They can attach titles directly to their wallet as Primary or Secondary Titles. Titles are used by players to brag about achievements and make unique their status. The title is limited to 40 characters."
        },
        "description": {
            "type": "string",
            "description": "Description of the asset which this token represents. It adds a deeper description of the asset to differenciate the Title. In-game title description reads this as it contains a fixed max character limit of 500."
        }
    }
}
```

### Lore Subtype
```
{
    "title": "Asset Properties",
    "type": "object",
    "properties": {
        "title": {
            "type": "string",
            "description": "Name of the Lore. The lore name is limited to 40 characters."
        },
        "description": {
            "type": "string",
            "description": "Description of the asset which this token represents. This can be used to add more background information about a character, a place, an event, or whatever in-game content. This is used to build a richer world through collectibles and unlockable lore. Lore's description is limited to 5000 characters."
        }
    }
}
```


### Custom Subtype
```
{
    "title": "Asset Properties",
    "type": "object",
    "properties": {
        "title": {
            "type": "string",
            "description": "Name of the Custom text asset. No character limit."
        },
        "description": {
            "type": "string",
            "description": "Description of the asset which this token represents. No character limit. This allows the developer to create whatever text based content they wish to add. This may not be supported by other games as it is custom to the creator."
        }
    }
}
```

## Rationale

For Text-based assets, we have two properties: `title` and `description`. Not to be confused for the `name` and `description` properties in the main metadata object, `title` and `description` inside the `asset-properties` object are the properties that should appear in-game when this asset is loaded by the user. 

The character limits for the `title` and `description` are there so that game developers may allocate the appropriate amount of space for when these assets are loaded. Any text-based nft that goes over the character limit will not be loaded by the in-game libraries and thus players won't be able to use them. 

The `custom` subtype is used for developers who wish to use text assets with no character limits. These may or may not be supported by ecosystem developers so be aware.

Developers may also opt to impose profanity filters so be aware that the assets may not be loaded for reasons other than character limits.

## Samples

### Title Subtype
```
{
    "name": "Rawrshak Original Sinner Title",
    "description": "Title bestowed to the earliest community members.",
    "image": "<default text icon uri on arweave>",
    "type": "text",
    "subtype": "title",
    "asset-properties": 
    {
        "title": "Rawrshak Original Sinner",
        "description": "The earliest of Rawrshak believers. The one's willing to listen to an odd voice somewhere in the depths of the crypto space. These unique individuals supported the creation of the next reality during the depths of a world crisis.",
    },
    "developer-properties":
    {
        "creator-comments": "These guys seriously believed in me when no one else did. I just want to return the favor."
    }
}
```

### Lore Subtype
```
{
    "name": "Rawrshak's Reason for Existence Lore",
    "description": "The reason why Rawrshak Exists",
    "image": "<default text icon uri on arweave>",
    "type": "text",
    "subtype": "title",
    "asset-properties": 
    {
        "title": "Rawrshak's Reason for Existence",
        "description": "During the depths of the Covid crisis, the mental health of the creator was slowly deteriorating. Day-in, Day-out, his desire to work became less and less. Is this what I really want to do with my life? Yes, it's a comfortable life and one without worries.. But is this it? 150 more years of this? Do I not have something else to give to this world? Yes, it is the arrogance of man that lead me to think I can change the world, but isn't aiming for the impossible something worth doing? Fuck it, I want to try. Failure is always an option, BUT I need to try.",
    },
    "developer-properties":
    {
        "creator-comments": "TLDR, without traveling, friends, and worldy distractions, I realized my life was going to be monotonous for the rest of my life if nothing changed. I might as well aim for the moon! I have nothing to lose. And I'm extremely blessed that I have nothing to lose. Side note: I do think I will live another 150 years more."
    }
}
```