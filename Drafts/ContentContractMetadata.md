---
title: Content Contract Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/KEka3ZJNSn
created: 2021-08-23
updated: 2021-08-24
version: 0.1
---

# Content Contract Metadata

## Simple Summary

This is the schema for the content contract metadata. This metadata contains information about the content contract and the assets on that contract. 

## Abstract

A Content Contract is a collection of assets tied together by a common grouping such as assets by the same creator, assets beloning to the same game, or other specific grouping. The contract metadata will contain information relating to the asset collection such as the name of the collect, description, a logo, the creator and the current owner. 

This metadata be used by all user-facing applications to display contract information. The ERC-1155 only contains information about the individual assets on the contract but not the overall information about the collection. We aim to fill in this gap.

## Terminology

* `Asset` - the data stored representing in-game tradable items. 
* `Token` - these are the on-chain representation of ownership of asset instances.
* `Content Contract` - this is the onchain representation of a collection of assets.

// Comment: Should I just kill the 'asset vs token' comparison?

## Specification 

The content contract metadata schema will be loosely based on the [ERC-1155 schema](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md#erc-1155-metadata-uri-json-schema).

```
{
    "title": "Content Contract Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Name of the Content Contract"
        },
        "description": {
            "type": "string",
            "description": "Description of the Content to which this collection of assets represents."
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* of a logo or symbol for the content this contract represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        },
        "creator": {
            "type": "string",
            "description": "Person, Group of People, or Company that created this contract"
        },
        "owner": {
            "type": "string",
            "description": "Person, Group of People, or Company that owns this contract if different from the creator"
        },
        "properties": {
            "type": "object",
            "description": "Arbitrary properties. Values may be strings, numbers, object or arrays. This is data specific to the creator's game or project."
        }
    }
}
```

The content contract metadata will be uploaded to permanent storage such as Arweave so data regarding the contract is persistent. This cannot be updated over time and will be permanent once uploaded. 

### Todo: Localization
Similarly to the ERC1155 metadata json schema, metadata localization should be standardized to increase presentation uniformity across all languages. This should also be loosly based on the ERC1155 metadata localization schema. 

## Rationale

`name`, `description`, and `image` are directly copied from the ERC-1155 metadata as they are basic information to set up the contract's information page and marketplace store. Gamers would want to know where specific assets came from and not just info about the asset itself.

`creator` and `owner` are human readable names to identify the creator of the contract. However, this shouldn't be made as the main source of contract legitimacy. Any front-facing UI should always show the creator address for the contract to verify contract legitimacy. 

`properties` is an additional field where developers and content creators can add their own additional information specific to their project or creations.

## Samples

An example of the Content Contract metadata json file follows:

```
{
	"name": "Rawrshak Default Assets",
	"description": "Lorem ipsum...",
	"image": "https://arweave.net/9KvpWZFGq1rD0slJYbe54-cQ34RTDHmNHiuqJk__AB8",
	"creator": "Rawrshak",
	"owner": "Rawrshak",
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