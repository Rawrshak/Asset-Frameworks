---
title: Public Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-23
updated: 2022-03-02
version: 1.0
---

# Public Asset Metadata

## Simple Summary

This is the schema for the public metadata for assets. This metadata contains information common between the different asset types and should be used by front-end applications to display asset information.

## Abstract

Unlike the ERC-721 standard, Rawrshak interoperable gaming Semi-Fungible Tokens (SFTs) requires the metadata extension. This allows game developers to define specific name and details about the assets which the gaming NFTs represent. 

This Public Asset Metadata schema is loosely based on the [ERC-1155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema) and [ERC-721 metadata schema](https://eips.ethereum.org/EIPS/eip-721) so that these may be usable in other marketplaces and applications designed to parse ERC-1155 and ERC-721 NFT metadata.

Games and applications should be able to parse and process an asset's public metadata. 

## Terminology 

* `Asset` - the data stored representing in-game tradable items. 
* `Non-Fungible Tokens (NFTs)` - these are the on-chain representation of ownership of unique in-game assets.
* `Semi-Fungible Tokens (SFTs)` - these are the on-chain representation of ownership of in-game assets instances. Rawrshak uses SFTs for in-game asset as they are more efficient. SFTs represent instances of in-game assets that are fungible but can be converted to a unique NFT (non-fungible). Assets are convertable between completely unique and fungible. 
* `Public Metadata` - metadata that is retrievable by any user
* `Private metadata` - metadata that is only retrievable by the token holder (and creator) for that asset
* `Asset Creation` - Creating an asset means storing the asset details that an SFT represents, making it available for minting.
* `Asset Minting` - Minting an asset means instantiating the tokens for an asset that can be distributed.
* `Content Creators` - The creator of an asset
* `Game Developer` - developers who create games that adopt these asset frameworks.

## Specification 

The public metadata schema will be based on the [ERC-1155 schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema), which in turn is loosely based on the [ERC-721 metadata schema](https://eips.ethereum.org/EIPS/eip-721).

```
{
    "title": "Token Public Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Identifies the asset to which this token represents"
        },
        "description": {
            "type": "string",
            "description": "Describes the asset to which this token represents"
        },
        "decimals": {
            "type": "integer",
            "description": "Unsupported - The number of decimal places that the token amount should display - e.g. 18, means to divide the token amount by 1000000000000000000 to get its user representation."
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* of an asset this token represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        },
        "tags": {
            "type": "array",
            "description": "An array of strings used to filter or categorize an asset"
        },
        "type": {
            "type": "string",
            "description": "The major asset categorization of asset that the token is representing. This informs the metadata reader on how to parse the token's asset properties. Values may be text, image, audio, static_object, and other future types."
        },
        "subtype": {
            "type": "string",
            "description": "The minor asset categorization of asset that the token is representing. The value depends on what the type property of the asset is. This informs the metadata reader of the expected in-game usage of the asset and specific requirements for the asset."
        },
        "nsfw": {
            "type": "boolean",
            "description": "A flag that indicates that a particular asset contains explicit or adult content."
        },
        "assetProperties": {
            "type": "object",
            "description": "Type-specific properties of the asset that is necessary to present the asset in-game."
        },
        "properties": {
            "type": "object",
            "description": "Arbitrary properties. Values may be strings, numbers, object or arrays. Additional data specific to the creator's game or project."
        }
    }
}
```

### Localization
Similarly to the ERC1155 metadata json schema, metadata localization should be standardized to increase presentation uniformity across all languages. This should also be loosely based on the ERC1155 metadata localization schema. 

## Rationale

The public metadata schema is based on the ERC-1155 schema in order to be usable by other marketplaces in the crypto space. This allows gamers to buy and sell their assets on any marketplace that supports ERC-1155 assets. 

We extended the schema by adding `type` and `subtype` to help games identify and filter assets. The `subtype` gives games a hint on an asset's expected usage.

`image` property may default to Rawrshak's default asset type images provided if the user decides not to place their own. If it's null, the marketplace or developer may need to link to image placeholders. 

`nsfw` gives games the ability to flag and filter explicit content that may not be suitable for all viewers. Content Creators are expected to flag their own assets.

`tags` is an array of strings that user-facing applications will use to organize assets. Tags must be lower-case.

`assetProperties` is an object that contains the data specific to the `type` and `subtype` asset. Each type of asset will have a different frameworks for what the `assetProperties` object contains. We will create separate documents for each schema pertaining to the type/subtypes. Some types may have duplicate properties such as `description`. In these cases, the app developer should use the lower level property in the `assetProperties`. If it is empty or undefined, the reader should use the higher level property. 

`properties` is an object that contains arbitrary properties that a developer can added specific to their asset. Developers may use the `properties` object to add information specific to their game or custom asset usage.

Currently, the Rawrshak Asset Framework only supports `text`, `audio`, `image`, and `static_object` (static 3d object). In the future, the project aims to create frameworks for other in-game assets such as Chracters (skins), Animations (poses, emotes), Attachables (Hats, weapons, backpacks), Consumables (Area keys, lootboxes, DLC keys), etc. `custom` subtype assets may be creator or game specific and will probably be filtered out (unsupported) by other games.

`subtypes` may have asset-specific requirements. For example, a `profile` subtype may require the texture/image to have an aspect ratio of 1:1 and under a specific size resolution. 

`decimal` is unsupported making these assets not fractionalizable. It is kept in order to maintain compatibility with the ERC-1155 metadata schema.

## Samples

### Text Type
#### Title Subtype
```
{
    "name": "Big Achievement",
    "description": "The user has a big achievement!",
    "image": "https://arweave.net/1f_wbiCIxcAi_WU3tLjcW0mEs5mZtGW_iKx7_fHWFnw",
    "tags": [
        "rawrshak",
        "title",
        "big achievement"
    ],
    "type": "text",
    "subtype": "title",
    "nsfw": "false",
    "assetProperties": 
    {
        "title": "Big Achievement Title",
        "description": "Success in Big Challenge. Player defeated Big Boss."
    },
    "devProperties":
    {
        "experience-gain": 50,
        "level-requirement": 10,
        "unlock-bonus": "AA4424"
    }
}
```

Notes: It is okay to have some missing information in the json object. For example, the metadata may be missing `decimals` or `devProperties`.
