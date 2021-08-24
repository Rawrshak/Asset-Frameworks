---
title: Public Token Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-23
updated: 2021-08-23
version: 0.1
---

# Public Token Metadata

## Simple Summary

This is the schema for the public metadata for assets in the content contract. This metadata contains public information about the asset and will be used by front-end applications.

## Abstract

The public asset metadata is standardized so that all front-end applications know how to parse and show information about each asset. This will also be downloaded, processed, and displayed in Rawrshak-enabled games.

## Terminology 

`Asset` - the data stored representing in-game tradable items. 
`Tokens` - these are the on-chain representation of ownership of asset instances.
`Public Metadata` - metadata that is retrievable by any user
`Private metadata` - metadata that is only retrievable by the token holder for that asset (and developers wallets)

## Specification 

The public metadata schema will be based on the [ERC-1155 schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema).

```
{
    "title": "Token Public Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Name of the asset"
        },
        "description": {
            "type": "string",
            "description": "Description or lore of the asset which this token represents"
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* of an asset this token represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        },
        "type": {
            "type": "string",
            "description": "The type of asset that the token is representing. This informs the hidden data reader on how to parse the token's hidden metadata. Values may be text, image, audio, static_object, or other future types that is added."
        },
        "subtype": {
            "type": "string",
            "description": "The subtype of asset that the token is representing. The value depends on what the primary type of the asset is. This informs the hidden data reader if there are any special or custom data in the metadata that it can use. It will also inform the game on how to ideally use the asset."
        },
        "properties": {
            "type": "object",
            "description": "Arbitrary properties. Values may be strings, numbers, object or arrays. This is data specific to the creator's game or project."
        }
    }
}
```

## Rationale

The public metadata schema is based on the ERC-1155 schema in order to be usable by other marketplaces in the cryptospace. This allows gamers to buy and sell their assets on any marketplace that supports ERC-1155 assets. 

We extended the schema by adding `type` and `subtype` to help front ends and marketplaces order and filter assets as they deem necessary. It also helps the in-game libraries determine how to parse the private metadata for in-game usage. 

### Todo: Localization
Similarly to the ERC1155 metadata json schema, metadata localization should be standardized to increase presentation uniformity across all languages. This should also be loosly based on the ERC1155 metadata localization schema. 

## Samples

Give sample of the standard in use.

Notes: 
* Asset `images` may default to our default asset type images we provide if the user decides not to place their own. This could also be null (or optional). If it's null, the marketplace or developer may need to link to image placeholders. 
* `custom` assets may be game specific and may not be supported by other rawrshak-enabled games.
* Currently, the following are the first asset standards to be supported. Over time, as more types of assets get standardized, more types and subtypes will be added.
* `subtypes` may have asset-specific requirements and private metadata specific. For example, a `logo` subtype may require the texture/image to be a 256x256 image and have to be under a specific size. This may be deemed by the community as standardized logo dimensions.

### Text Type
#### Title Subtype
```
{
	"name": "Big Achievement",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "text",
	"subtype": "title"
}
```

#### Lore 
```
{
	"name": "Character's Background",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "text",
	"subtype": "lore"
}
```

### Audio Type
#### SoundEffect Subtype
```
{
	"name": "Character's Death Scream",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "audio",
	"subtype": "soundeffect"
}
```

#### Voice Line Subtype
```
{
	"name": "Character's Quote",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "audio",
	"subtype": "voiceline"
}
```

#### Background Music Subtype
```
{
	"name": "Character's theme",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "audio",
	"subtype": "bgm"
}
```

#### Custom Subtype
```
{
	"name": "Develoepr-specific audio",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "audio",
	"subtype": "custom"
}
```

### Image Type
#### Standard Subtype
```
{
	"name": "Game Logo",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "image",
	"subtype": "standard"
}
```

#### Logo Subtype
```
{
	"name": "Game Logo small",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "image",
	"subtype": "logo"
}
```

#### Decal Subtype
```
{
	"name": "Character Decal",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "image",
	"subtype": "decal"
}
```

#### Banner Subtype
```
{
	"name": "Character Banner",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "image",
	"subtype": "banner"
}
```

### Static Object Type
#### Trophy Subtype
```
{
	"name": "Winner winner chicken dinner",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "staticobject",
	"subtype": "trophy"
}
```

#### Custom Subtype
```
{
	"name": "Character Statue Statue",
	"description": "…",
	"image": "<default text icon uri on arweave>",
	"type": "staticobject",
	"subtype": "custom"
}
```