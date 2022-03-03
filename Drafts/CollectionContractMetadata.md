---
title: Collection Contract Metadata
status: For Review
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/KEka3ZJNSn
created: 2021-08-23
updated: 2022-03-02
version: 1.0
---

# Collection Contract Metadata

## Simple Summary

This is the schema for the collection contract metadata. This metadata contains information about the collection contract and the assets on that contract. 

## Abstract

A Collection Contract is a contract that contains the set of assets tied together by a common grouping such as assets by the same creator, assets belonging to the same game, or other. The contract metadata will contain information relating to the asset collection such as the name of the collection, description, a logo image, and the creator. 

This metadata be used by all user-facing applications to display the collection contract information. The ERC-1155 contract only contains information about the individual assets on the contract but not the overall information about the collection. The aim is to fill in this gap.

## Terminology

* `Asset` - the data stored representing in-game tradable items. 
* `Token` - these are the on-chain representation of ownership of asset instances.
* `Collection Contract` - A smart contract that contains the on-chain representation of a collection of assets.
* `Content Creators` - The creator of an asset
* `Game Developer` - developers who create games that adopt these asset frameworks.

## Specification 

The collection contract metadata schema will be loosely based on the [ERC-1155 schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema).

```
{
    "title": "Collection Contract Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Identifies the Collection to which this contract represents"
        },
        "description": {
            "type": "string",
            "description": "Description of the Collection to which this set of assets represent"
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* of a logo or symbol for the collection this contract represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        },
        "game": {
            "type": "string",
            "description": "[Optional] A string that links a specific game to this collection contract. This may be used by the front-end to organize game-specific contracts."
        },
        "creator": {
            "type": "string",
            "description": "[Optional] A string that identifies a person, group, or company that created this contract"
        },
        "tags": {
            "type": "array",
            "description": "An array of strings used to filter or categorize the collection contract metadata"
        },
        "properties": {
            "type": "object",
            "description": "Arbitrary properties. Values may be strings, numbers, object or arrays. Additional data specific to the creator's game or project."
        }
    }
}
```

The collection contract metadata will be uploaded to permanent storage, such as Arweave, so data regarding the contract is persistent. This cannot be updated over time and will be permanent once uploaded. 

### Localization
Similarly to the ERC1155 metadata json schema, metadata localization should be standardized to increase presentation uniformity across all languages. This should also be loosely based on the ERC1155 metadata localization schema. 

## Rationale

`name`, `description`, and `image` are directly adopted from the ERC-1155 metadata as they are basic information to set up the contract's information page and marketplace store. Gamers would want to know where specific assets came from and not just info about the asset itself.

`creator` is an optional string set by the creator and can identify the creator of the contract. However, this shouldn't be made as the main source of contract legitimacy.

`game` is an optional string property for game developers to link the Collection Contract to their game. This will be used by the front end to organize game-specific collection contracts. It notifies the front end about whether or not this contract is a collection of independent assets or is attached to a specific Game.

`tags` is an array of strings that user-facing applications will use to organize the contracts. Tags must be lower-case.

`properties` is an additional field where content creators can add their own unique properties. Because `properties` are only known to the creator, user-facing applications may only be able to parse simple properties and ignore complex properties. If a developer wishes to parse more complex properties, they will need to implement their own metadata parser parser for these properties.

## Samples

An example of the Collection Contract metadata json file follows:

```
{
    "name": "Rawrshak Default Assets",
    "description": "Lorem ipsum...",
    "image": "https://arweave.net/9KvpWZFGq1rD0slJYbe54-cQ34RTDHmNHiuqJk__AB8",
    "game": "",
    "creator": "Rawrshak",
    "tags": [
        "rawrshak",
        "test",
        "role-playing game"
    ],
    "properties": {
        "simple_property": "example value",
        "rich_property": {
            "name": "Name",
            "value": "123",
            "display_value": "123 Example Value",
            "class": "emphasis",
            "css": {
                "color": "#ffffff",
                "font-weight": "bold",
                "text-decoration": "underline"
            }
        },
        "array_property": {
            "name": "Name",
            "value": [1,2,3,4],
            "class": "emphasis"
        }
    }
}
```