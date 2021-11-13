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

* `prefab` - The prefab is a pre-configured game object complete with all its components, property values, and child game objects as a reusable asset. An asset's prefab must contain all the necessary parts that get instatiated in-game.
* `asset shape` - The asset's shape indicates what default asset an asset will replace in-game. Game developers expect assets to fit within a default asset's bounding box. 
* `scale` - The size of the objects within the prefab relative to Unity's game world scale.
* `orientation` - The direction that an asset faces forward. Game developers will expect that assets are facing the default direction and will rotate in-game assets as necessary.
* `location` - The default position of an asset in-game when loaded.

## Specification 

Ideally, Static 3D Object Assets should be loadable by as many games as possible. However, there are very real limitations to what can and cannot be loaded by games on specific engines, platforms, render pipelines, etc. With that in mind, the proposed metadata should give the developer enough information to determine whether or not the Rawrshak asset can be loaded by their game. This metadata should include all the necessary information for each package targeting different platforms. 

We propose specific base or default requirements so that assets may easily be loaded by less powerful machines.

### Minimum Rawrshak Asset Requirements:
As of 11/12/2021, these are the minimum requirements for Static 3D Objects:

Property | Information
------ | ------
Engine | Unity Engine
Platform | Windows Standalone 64
Render Pipeline | Built-in Render Pipeline
Fidelity | Low

The asset must fit within one of the Asset Shape categories. Orientation and scale of the prefab must adhere to the specifications below.

### Supported Engines
Name | Plans
------ | ------
Unity Engine | Supported
Unreal Engine | Future 
Godot Engine | Future

#### Unity Engine
Unity Engine is the most flexible of the game engines and it is the first engine we're deciding to support. However, Unity Engine has several types of render pipelines. 3d objects packed in asset bundles don't necessarily work properly across different render pipelines. We will be targeting one specific render pipeline first and then expanding in the future. 

##### Supported Platforms
Asset packages are platform specific. Game developers need to build and upload packages for the same asset targeting different platforms. Data is bundled, compressed, and formatted differently for different platforms for different reasons such as hardware capabilities, software capabilities, different software licensing requirements, optimization strategies.

Platform | Plans
------ | ------
Android | Supported
iOS | Supported
Windows X64 | Supported
WebGL | Supported

##### Supported Render Pipelines

*Unity has different render pipelines* and asset materials may or may not be usable between different render pipelines. This is because each render pipeline uses different shader outputs and might not have the same features. Each render pipeline is suitable for different games and platforms.

For now, we will ignore the High Definition Render Pipeline and Universal Render Pipeline and will only focus on the Built-in Render Pipeline. Support for other Unity pipelines and other game engine render pipelines will come soon.

Name | Plans
------ | ------
Built-in Render Pipeline | Supported
Universal Render Pipeline | Future
High Definition Render Pipeline | Future

### Levels of Fidelity
We propose three levels of fidelity: Low-fidelity, Medium-fidelity, and High-fidelity. Low-fidelity is ideal for platform that have limited storage, memory, and CPU power. Mobile devices (Android and IOS) are the probable use-case for this. Low-fidelity assets allow for quick download and in-game loading. This is the default setting. Asset developers must support Low-Fidelity. Medium and High fidelity assets are for desktop platforms and should only be optional. Games expecting to be run on desktop or higher powered machines can load these assets if the developer and gamer chooses to do so.

// Todo: Define the specific requirements and limits for Low, Medium, and High fidelity assets.

### Asset Shapes
There are several asset shapes (and default objects) that an asset must fit into. We provide bounding boxes to make sure an asset doesn't go beyond what the framework expects an asset's scale is to be. Based on an asset's shape, the game developer can and will place them in their game. The game developer is free to scale or rotate the prefab after loading, but game developers expect the asset's prefab to be within the shape's bounding box.

We will provide tools that will help notify the user if an asset exceeds the bounding box for a given shape.

The following shapes include:
Shape | Width | Height | Depth
------ | ------ | ------ | ------
Small Object | 1 | 1 | 1
Medium Object | 2 | 2 | 2
Large Object | 4 | 4 | 4
Horizontal X Object| 2 | 1 | 1
Horizontal Y Object| 1 | 2 | 1
Horizontal Z Object| 1 | 1 | 2

### Asset Orientation and Asset Location
The Asset's prefab must be oriented in the correct direction and located at the correct position. The asset's "front" view must be towards the +Z direction. The default camera is placed at (0,1,3) and the prefab's front must be facing the camera. 

The Asset's location must be defaulted to the origin. The asset must be above the (0,0,0) point. The testing bounding boxes and default assets are expecting the asset to be at the origin before the prefab is packaged.

### Subtypes

For the Static 3d Object, subtypes are a way to describe what the asset is used for and a way to filter assets. Some subtypes may have certain requirements, but others may not. It is up to the game developer to know how to use the different subtypes in their games.

#### Trophies Subtype
Trophies are static 3d objects that are rewarded to users for achievements in-game. They are required to have a Horizontal Y shape. 

#### Decoration Subtype
Decorations are static 3d objects that are acquired by users. There is no specific shape requirement for these assets. They can be loaded anywhere and placed anywhere. There's no specific usage designated for this type of static 3d object.

### Metadata Schema 
```
{
    "title": "Asset Properties",
    "type": "object",
    "properties": {
        "textures": {
            "type": "array",
            "description": "An array of Prefab objects"
        }
    }
}

{
    "title": "Prefab",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "name of the prefab that's stored in the package"
        },
        "engine": {
            "type": "string",
            "description": "engine which this package supports. Values include unity, unreal, and godot."
        },
        "platform": {
            "type": "string",
            "description": "platform refers to the operating system that the game is on. Values include android, ios, windows, and webgl."
        },
        "renderPipeline": {
            "type": "string",
            "description": "renderPipeline refers to the specific rendering pipeline that is set for a game engine. values on this include brp (built-in render pipeline), urp (universal render pipeline), or hdrp (high-definition render pipeline)."
        },
        "fidelity": {
            "type": "string",
            "description": "fidelity refers to the asset's quality, ease of download, and ease of loading. Values include low, medium, and high."
        },
        "shape": {
            "type": "string",
            "description": "shape refers to which type of default asset this object is expecting to replace.Values include small, medium, large, horizontalx, horizontaly, and horizontalz."
        },
        "uri": {
            "type": "string",
            "description": "uri to the asset package stored in a decentralized database."
        }
    }
}
```
## Rationale

Define the reasons for the specification and why this is the solution we've come up with.

## Samples

{
    "name": "Rawrshak Trophy",
    "description": "Rawrshak in-game trophy for achieving absolutely nothing.",
    "image": "<default text icon uri on arweave>",
    "tags": [
        "Rawrshak",
        "Static 3D Object"
    ],
    "type": "static3dobject",
    "subtype": "trophy",
    "assetProperties": 
    [
        {
            "name": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "windows",
            "fidelity": "low",
            "shape": "horizontaly",
            "uri": "arweave.net/<transaction-id>"
        },
        {
            "name": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "ios",
            "fidelity": "low",
            "shape": "horizontaly",
            "uri": "arweave.net/<transaction-id>"
        },
        {
            "name": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "android",
            "fidelity": "low",
            "shape": "horizontaly",
            "uri": "arweave.net/<transaction-id>"
        },
        {
            "name": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "webgl",
            "fidelity": "low",
            "shape": "horizontaly",
            "uri": "arweave.net/<transaction-id>"
        },
        {
            "name": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "windows",
            "fidelity": "medium",
            "shape": "horizontaly",
            "uri": "arweave.net/<transaction-id>"
        },
        {
            "name": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "high",
            "fidelity": "medium",
            "shape": "horizontaly",
            "uri": "arweave.net/<transaction-id>"
        }
    ],
    "devProperties":
    {
        "creatorComments": "A trophy that is the most appealing!"
    }
}
