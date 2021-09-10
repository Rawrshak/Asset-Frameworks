---
title: Public Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-23
updated: 2021-09-09
version: 0.2
---

# Public Asset Metadata

## Simple Summary

This is the schema for the public metadata for assets in the content contract. This metadata contains public information about the asset and will be used by front-end applications.

## Abstract

The public asset metadata is a metadata framework that all front-end applications know how to parse and show information about each asset. This will also be downloaded, processed, and displayed in Rawrshak-enabled games.

Todo: The public metadata will now contain subcategory data that will be viewable by anyone. The reason in-game asset data should be public is because others won't be able to download and view the asset if it's only downloadable by the owner. I need to update this to include that data for each type and subtype of asset.

## Terminology 

* `Asset` - the data stored representing in-game tradable items. 
* `Tokens` - these are the on-chain representation of ownership of asset instances.
* `Public Metadata` - metadata that is retrievable by any user
* `Private metadata` - metadata that is only retrievable by the token holder for that asset (and developers wallets)

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
        "tags": {
            "type": "array",
            "description": "An array of strings that will be used as Tags for the content contract metadata."
        }
        "type": {
            "type": "string",
            "description": "The type of asset that the token is representing. This informs the hidden data reader on how to parse the token's hidden metadata. Values may be text, image, audio, static_object, or other future types that is added."
        },
        "subtype": {
            "type": "string",
            "description": "The subtype of asset that the token is representing. The value depends on what the primary type of the asset is. This informs the hidden data reader if there are any special or custom data in the metadata that it can use. It will also inform the game on how to ideally use the asset."
        },
        "assetProperties": {
            "type": "object",
            "description": "Asset properties that are specific to the type and subtype of the asset that is necessary to present the asset in-game."
        },
        "developer-properties": {
            "type": "object",
            "description": "Arbitrary properties. Values may be strings, numbers, object or arrays. This is data specific to the creator's game or project."
        }
    }
}
```

## Rationale

The public metadata schema is based on the ERC-1155 schema in order to be usable by other marketplaces in the cryptospace. This allows gamers to buy and sell their assets on any marketplace that supports ERC-1155 assets. 

We extended the schema by adding `type` and `subtype` to help front ends and marketplaces order and filter assets as they deem necessary. It also helps the in-game libraries determine how to parse the private metadata for in-game usage. 

`assetProperties` is an object that contains the data specific to the `type` and `subtype` asset. Each type/subtype of asset will have a different frameworks for what the `assetProperties` object contains. We will create separate documents for each schema pertaining to the type/subtypes. 

`developer-properties` is an object that contains data that the developer added specific to their game. This is similar to the `properties` object in the ERC-1155 schema. Developers may use this object to add additional information as necessary.

### Todo: Localization
Similarly to the ERC1155 metadata json schema, metadata localization should be standardized to increase presentation uniformity across all languages. This should also be loosly based on the ERC1155 metadata localization schema. 

## Samples

Notes: 
* Asset `images` may default to our default asset type images we provide if the user decides not to place their own. This could also be null (or optional). If it's null, the marketplace or developer may need to link to image placeholders. 
* `custom` subtype assets may be game specific and may not be supported by other rawrshak-enabled games.
* Currently, the following are the first asset frameworks to be supported. Over time, as more types of assets are proposed or requested, more types and subtypes will be added.
* `subtypes` may have asset-specific requirements. For example, a `logo` subtype may require the texture/image to be a 256x256 image and have to be under a specific size. Eventually, the community will want to set a standard dimensions for specific things like logos or gamer emblems. `title` subtype may have a `title` character limit of 40 characters and description of 500 characters whereas `lore` subtype may have a `title` character limit of 40 with description limit of 5000. These character limits allow developers to allocate the specific space so all `title` and `lore` assets can fit when displayed.

### Text Type
#### Title Subtype
```
{
    "name": "Big Achievement",
    "description": "…",
    "image": "<default text icon uri on arweave>",
    "tags": [
        "Rawrshak",
        "Title",
        "Major Achievement"
    ],
    "type": "text",
    "subtype": "title",
    "assetProperties": 
    {
        "title": "Big Achievement Title",
        "description": "Success in Big Challenge. Player defeated Big Boss."
    },
    "developer-properties":
    {
        "experience-gain": 50,
        "level-requirement": 10,
        "unlock-bonus": "AA4424"
    }
}
```
