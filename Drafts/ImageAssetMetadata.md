---
title: Image Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-29
updated: 2022-03-02
version: 1.0
---

# Image Asset Metadata

## Simple Summary

The Image Asset Metadata framework creates a guide for Image-based asset tokens, with use-cases such as Profile images, Decals, and Banners.

## Abstract

The Image Asset Framework defines a metadata standard for image/2d-based assets and image asset use-cases. The image metadata standard is stored in the `assetProperties` property in the Public Asset Metadata. 

The metadata standard allows game developers to easily parse metadata for image asset tokens for display in-game. This allows players to take their image assets and use them wherever it is applicable.

Image asset tokens can be used in many different manners such as rewarding players with emblems, logos, sprays, decals, etc. Players may also create their own image assets that they can bring into their games to show their unique content. Game developers may also use Image assets as a way to reward players with in-game artwork.

## Terminology 

* `Resolution` - this defines the amount of detail an image has. In general, it refers to the pixel count: N pixels width by M pixels height
* `Aspect Ratio` - an attribute that describes the proportional relationship between the width of an image and its height.
* `content type` - the metadata used to indicate the original media type of the resource (prior to any content encoding applied for sending or storage)

## Specification 

We define the Image-based asset frameworks for different in-game use cases. The `subtypes` are hints towards game developers on how to import and load assets into their game. Developers are free to interpret the framework and use the incoming gamer assets for their game's purposes. There are no specific resolution requirements, but creators should keep in mind that game developers may cap the maximum resolution they are willing to load for efficiency. Consider making images at a width between 256 and 1024 pixels.

### Profile Subtype

The profile subtype is used for user profile images. An existing Art-NFT and PFP can be wrapped into this image subtype. The profile subtype requires that the image texture has an aspect ratio of 1:1. Ideally, the default resolution is 256x256, but a creator may provide other resolutions if desired. 

Game developers can use image assets of this subtype to display a gamer's profile image, guild emblems, project or game logos, etc.

### Decal Subtype

The decal subtype requires that the image texture have an aspect ratio of between 2:1 and 1:2. This type of asset allows a user to display an image in-game such as decals, sprays, guild emblems, etc. 

### HorizontalBanner Subtype

The horizontal banner subtype requires that the image texture has an aspect ratio greater than 1.5:1. This type of asset can be used for in-game things like billboards, profile banners, advertisments, etc.

### VerticalBanner Subtype

The vertical banner subtype requires that the image texture has an aspect ratio greater than of 1:1.5. This type of asset can be used for in-game things like billboards, profile banners, advertisments, etc.

### Custom Subtype

The custom subtype doesn't have a requirement for aspect ratio and doesn't have a default texture requirement dimension. The creator is free to experiment with different formats ideal for their game. The custom subtype, however, may not be loadable in other games aside from the creator's game. The dimensions, ideally, would still be a power of 2.

### Metadata Schema 
```
{
    "title": "Asset Properties",
    "type": "object",
    "properties": {
        "textures": {
            "type": "array",
            "description": "An array of texture objects"
        }
    }
}

{
    "title": "Texture",
    "type": "object",
    "properties": {
        "uri": {
            "type": "string",
            "description": "link to the texture object"
        },
        "height": {
            "type": "int",
            "description": "[Optional] Height of the texture in Pixels"
        },
        "width": {
            "type": "int",
            "description": "[Optional] Width of the texture in Pixels"
        },
        "contentType": {
            "type": "string",
            "description": "[Optional] Content type. Can be image/png, image/jpg, image/svg"
        }
    }
}
```

## Rationale

`Unity Textures` require that dimension sizes should be powers of 2 on each side, (that is, 32, 64, 128, 256, 512, 1024, 2048 pixels (px), and so on). It is possible to use non-power of two dimensions but it requires more memory and is slower for the GPU to sample. We will notify the creator when they are using a resolution that isn't a power of 2. This may or may not be the case for `Unreal Engine` textures.

`uri` for each texture is what is loaded by the game engine. The `height` and `width`, which are optional, must correspond to the downloaded image texture (if they exist), otherwise, it will not be loaded. Based on the subtype, it is up to the game developer on how the texture is used. The game developer also determines whether to load the image as a texture and assign it to a material (3D Project) or load it as a Sprite (2D project). 

`textures` is an array of Texture objects, giving a content creator the ability to add different resolutions and media types for their image asset. The `height`, `width`, and `contentType` gives the game developer the ability to choose which texture they would like to load in their game, based on platform, graphics performance, or other reasons.

More `subtypes` (use cases) may also be proposed by game developers and content creators in the future.

## Samples

### Profile Subtype
```
{
    "name": "Rawrshak Logo Decal",
    "description": "Rawrshak Logo decal usable as a spray, emblem, or decal",
    "image": "https://arweave.net/2BmDysofcccaTUbjKJHgr9_0ImlFadE00yAS_3E9M00",
    "tags": [
        "rawrshak",
        "logo",
        "summer collection"
    ],
    "type": "image",
    "subtype": "profile",
    "nsfw": "false",
    "assetProperties": 
    [
        {
            "uri": "arweave.net/2BmDysofcccaTUbjKJHgr9_0ImlFadE00yAS_3E9M00",
            "height": 256,
            "width": 256,
            "contentType": "image/png"
        },
        {
            "uri": "arweave.net/95Da52sofcccaTUbjKJHgr9_0ImlFadE00yAS99e4Ca",
            "height": 512,
            "width": 512,
            "contentType": "image/png"
        },
        {
            "uri": "arweave.net/1asda774acccaTUbjKJHgr9_0asd7hh8q4dz95a4g5e"
        }
    ],
    "properties":
    {
        "creatorComments": "Rawrshak Represent!"
    }
}
```

### HorizontalBanner Subtype
```
{
    "name": "Rawrshak Banner",
    "description": "Rawrshak Banner usable in banners, headers, etc.",
    "image": "https://arweave.net/2BmDysofcccaTUbjKJHgr9_0ImlFadE00yAS_3E9M00",
    "tags": [
        "rawrshak",
        "logo",
        "summer collection"
    ],
    "type": "image",
    "subtype": "horizontal-banner",
    "assetProperties": 
    [
        {
            "uri": "arweave.net/asdas789zx93cccaTUbjKJHgr9asd423g1axa_sa500as",
            "height": 128,
            "width": 256,
            "contentType": "image/png"
        }
    ],
    "properties":
    {
        "creatorComments": "Rawrshak Banner Represent!"
    }
}
```