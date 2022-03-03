---
title: Static 3D Object Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-11-09
updated: 2022-03-02
version: 1.0
---

# Static 3D Object Asset Metadata

## Simple Summary

The Static 3D Object Asset Metadata framework formalizes static in-game 3D asset tokens such as trophies, statues, furniture, etc. Static 3D Object Assets are assets that do not animate in any manner and must contain a model and materials packaged together.

## Abstract

The Static 3D Object Asset framework defines a metadata standard for static 3d assets and static 3d use-cases. This contains the information for non-animated 3d asset tokens. The proposed metadata format enables game developers to easily parse information for static 3d assets for in-game use. Players can then unlock, mint, and acquire 3d assets from one game or world and use them in any game or world that supports the Rawrshak frameworks.

These asset tokens can be used in many different manners such as rewarding players with trophies alongside achievements, static character models, furniture, buildings, static animals, automobiles, or whatever type of in-game asset. Players and content creators may also create their own static asset NFTs that they can bring into the games. Players may also hire 3D modellers and content creators to create assets for them.

## Terminology 

Any terms that the reader must know.

* `prefab` - The prefab is a pre-configured game object complete with all its components, property values, and child game objects as a reusable asset. An asset's prefab must contain all the necessary parts that get instatiated in-game.
* `asset shape` - The asset's shape indicates what default asset an asset will replace in-game. Game developers expect assets to fit within a default asset's bounding box. 
* `scale` - The size of the objects within the prefab relative to Unity's game world scale.
* `orientation` - The direction that an asset faces forward in 3d world space. Game developers will expect that assets are facing the default direction and will rotate in-game assets as necessary.
* `location` - The default position of an asset 3d world space when loaded.
* `asset package` - An archive file that contains platform and game engine specific non-code assets (such as models, textures, prefabs, audio clips, etc) that the game engine can load at run time. In Unity, this is an AssetBundle file.

## Specification 

Ideally, Static 3D Object Assets should be loadable by as many games as possible. However, there are very real limitations to what can and cannot be loaded by different games. Games are packaged differently for different game engines, game engine versions, render pipelines, and platforms. Currently, there are no common asset packaging formats between game engines and platforms. In order for assets to be used across different games, a developer needs to create asset packages for each game engine and each platform. Unfortunately, there's currently no way around it because games are optimized for each platform. 

With that in mind, the proposed metadata should give the developer enough information to determine whether or not an asset can be loaded by their game. This metadata should include all the necessary information for each package targeting different platforms and give developers the ability to filter out assets that cannot be loaded.

We don't have minimum requirements, but we have recommended requirements so that assets may be loaded by _most_ games in the ecosystem.

As the goal of Rawrshak is to decentralize the project, eventually, the community will create proposals and updates. These updates and new feature requests will be put up for voting through the DAO. But for now, please reach out to us if there are any suggestions, changes, or request that a community member needs or wants. 

### Recommended Rawrshak Asset Requirements:
As of 2022-03-02, these are the recommended requirements for Static 3D Objects:

An asset must fit within one of the Asset Shape categories. Orientation and scale of the prefab must adhere to the specifications below.

### Game Engines Engines
Name | Plans
------ | ------
Unity Engine | Supported
Unreal Engine | Planned Support 
Godot Engine | Planned Support

Please contact us if there's a specific game engine or proprietary game engine that you would like support for. We're always open for collaborations and expanding support.

#### Unity Engine
Unity Engine is the most flexible of the game engines therefore it is the first engine we decided to support. However, Unity Engine has several types of render pipelines that a developer needs to take into consideration.

##### Supported Render Pipelines

*Unity has different render pipelines* and asset materials may or may not be usable between different render pipelines. This is because each render pipeline uses different shader outputs and might not have the same features. Each render pipeline is suitable for different games and platforms.

For now, we will ignore the High Definition Render Pipeline and Universal Render Pipeline and will only focus on the Built-in Render Pipeline. Support for other Unity pipelines and other game engine render pipelines will come soon.

Name | Plans
------ | ------
Built-in Render Pipeline | Supported
Universal Render Pipeline | Planned Support
High Definition Render Pipeline | Planned Support

##### Supported Platforms
Asset packages are platform specific. Game developers need to build and upload packages for the same asset targeting different platforms. Data is bundled, compressed, and formatted for different platforms for reasons such as hardware capabilities, software capabilities, different software licensing requirements, and optimization strategies (compression, data formats, filetype support, etc). The following are the platforms that we expect to support. Please contact us if there are specific platforms that a developer wants support for. 

Platform | Plans
------ | ------
Android | Supported
iOS | Supported
Windows X64 | Supported
WebGL | Supported

##### Unity Engine Tools
We provide Unity Engine tools that helps the game developer orient, scale, and position a static object for deployment. The tool also packages the asset data for different platforms. Please check out Rawrshak's [Unity Tools](https://github.com/Rawrshak/Unity-Tools/releases) repo for the latest Unity Packages.

### Asset Shapes
There are several asset shapes (and default objects) that an asset must fit into. We provide bounding boxes to make sure an asset doesn't go beyond what the framework expects an asset's scale is to be. Based on an asset's shape, the game developer will place them in their game. The game developer is free to scale or rotate the prefab after loading, but game developers expect the asset's prefab to be within the shape's bounding box.

We will provide tools that will help notify the asset creator if their asset exceeds the bounding box for a given shape.

The following shapes include:
Shape | Width | Height | Depth
------ | ------ | ------ | ------
Cube | 2 | 2 | 2
Horizontal X | 2 | 1 | 1
Horizontal Y | 1 | 2 | 1
Horizontal Z | 1 | 1 | 2

### Asset Orientation and Asset Location
The Asset's prefab must be oriented in the correct direction and located at the correct position. The asset's "front" view must be towards the +Z direction. The default camera is placed at (0,1,3) and the prefab's front must be facing the camera. 

The Asset's location must be defaulted to the origin. The asset must be above the (0,0,0) point. The testing bounding boxes and default assets are expecting the asset to be at the origin before the prefab is packaged.

Game developers will expect the prefab to be oriented and located correctly when loaded.

### Subtypes

For the Static 3d Object, `subtype` gives the game developer a hint for the use-case of the static 3d object. Some subtypes may have certain requirements, but others may not. It is up to the game developer to know how to use the different subtypes in their games. 

If there are specific subtypes and use-cases that game developers want to be come a standard, please reach out to us.

#### Trophies Subtype
Trophies are static 3d objects that are rewarded to users for achievements in-game. They are required to have a Horizontal Y shape. 

#### Decoration Subtype
Decorations are static 3d objects that are acquired by users. There is no specific shape requirement for these assets. They can be loaded anywhere and placed anywhere. 

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
        "prefabName": {
            "type": "string",
            "description": "name of the prefab that's stored in the package"
        },
        "engine": {
            "type": "string",
            "description": "engine which this package supports. Currently, values only include unity"
        },
        "platform": {
            "type": "string",
            "description": "platform refers to the operating system that the game is on. Values include android, ios, windows, and webgl."
        },
        "renderPipeline": {
            "type": "string",
            "description": "The specific rendering pipeline that is set for a game engine. Currently, values only include brp (built-in render pipeline)."
        },
        "shape": {
            "type": "string",
            "description": "shape refers to which type of default asset this object is expecting to replace. Currently, values include cube, horizontalx, horizontaly, and horizontalz."
        },
        "uri": {
            "type": "string",
            "description": "uri to the asset package stored in a decentralized database."
        }
    }
}
```
## Rationale

The `engine`, `renderPipeline`, and `platform` are properties that allow the game developer to filter the asset that they can load during runtime. These will be parsed by the subgraph and allow gamers to filter out assets they can use in a game.

The `shape` allows game developers to figure the general shape of a Rawrshak asset. This allows developers to figure out which default asset this static object can replace during runtime. It also gives soft guarantees that an asset will be within the shape's boundaries.

## Samples

```
{
    "name": "Rawrshak Trophy",
    "description": "Rawrshak in-game trophy for achieving absolutely nothing.",
    "image": "https://arweave.net/5jpY4ouIl9EXqyep6z0p0S_6nt87DEfsnsxefAa9cOE",
    "tags": [
        "rawrshak",
        "static 3d object"
    ],
    "type": "static3dobject",
    "subtype": "trophy",
    "nsfw": "false",
    "assetProperties": 
    [
        {
            "prefabName": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "windows",
            "shape": "horizontaly",
            "uri": "https://arweave.net/5jpY4ouIl9EXqyep6z0p0S_6nt87DEfsnsxefAa9cOE"
        },
        {
            "prefabName": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "ios",
            "shape": "horizontaly",
            "uri": "https://arweave.net/5jpY4ouIl9EXqyep6z0p0S_6nt87DEfsnsxefAa9cOE"
        },
        {
            "prefabName": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "android",
            "shape": "horizontaly",
            "uri": "https://arweave.net/5jpY4ouIl9EXqyep6z0p0S_6nt87DEfsnsxefAa9cOE"
        },
        {
            "prefabName": "rawr-trophy",
            "engine": "unity",
            "renderPipeline": "brp",
            "platform": "webgl",
            "shape": "horizontaly",
            "uri": "https://arweave.net/5jpY4ouIl9EXqyep6z0p0S_6nt87DEfsnsxefAa9cOE"
        }
    ],
    "properties":
    {
        "creatorComments": "A trophy that is the most appealing!"
    }
}
```