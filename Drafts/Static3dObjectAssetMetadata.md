---
title: Static 3D Object Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-11-09
updated: 2021-11-09
version: 0.1
---

# Static 3D Object Asset Metadata

## Simple Summary

The Static 3D Object Asset Metadata standard formalizes static in-game 3D asset NFTs such as trophies, statues, furniture, etc. Static 3D Object Assets are assets that do not animate in any manner and must contain a model and material packaged together.

## Abstract

The Static 3D Object Asset Metadata standard is `assetProperties` object in the Public Asset Metadata. This contains the information for non-animated 3d asset NFTs. This creates a common metadata format so that game developers can easily parse metadata for static 3d asset NFTs for usage in-game. This allows players to unlocked 3d assets from one game or world, collect and display them in world-creator games or wherever applicable.

3D model NFTs can be used in many different manners such as rewarding players with trophies alongside achievements, static character models, furniture, buildings, static animals, automobiles, or whatever in-game content asset that can exist. Players and content creators may also create their own static asset NFTs that they can bring into the games to show their unique content.

Players may also hire 3D modellers and content creators to create assets for them.

## Terminology 

Any terms that the reader must know.

* Todo:

## Specification 

Static 3D Object Assets should be small enough to be easily loadable by games. It is up to the developer on the levels of fidelity that they provide for the assets they create.

We propose specific base requirements so that assets may easily be loaded by less powerful machines.

*Unity has different render pipelines* and asset materials may or may not be usable between different render pipelines. This needs investigation still. For now, we will ignore the High Definition Render Pipeline and Universal Render Pipeline and will only focus on the Build-in Render Pipeline. This is subject to change.

`prefab-name` only contains one name. This means that when an asset is built, only one prefab will be built per asset. Please include all models, of more than 1, under one prefab. 

Unity supports many different platforms and that each package built for one platform isn't usable for other platforms. Because of this, a developer will need to build their package for each specific platform that you that the developer is targeting. 

At the very least, we recommend building for the following platforms: "Android, Win65, iOS, WebGL

## Rationale

Define the reasons for the specification and why this is the solution we've come up with.

## Samples

{
    "unity": 
    {
        "prefab-name": "hero.prefab"
        "target-platforms": [
            {
                "target-platform": "Win32",
                "uri": "https://arweave.net/<transaction-id>"
            },
            {
                "target-platform": "Android",
                "uri": "https://arweave.net/<transaction-id>"
            },
            {
                "target-platform": "Win64",
                "uri": "https://arweave.net/<transaction-id>"
            }
        ]
    },
    "properties": 
    {
        "otherDeveloperData": "Developer or game specific additional data"
    }
}
