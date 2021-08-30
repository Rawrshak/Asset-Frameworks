---
title: Image Asset Metadata
status: Draft
author: Christian Sumido (@gcbsumid)
discussions-to: https://discord.gg/Ge2j4Cd65H
created: 2021-08-29
updated: 2021-08-30
version: 0.1
---

# Image Asset Metadata

## Simple Summary

The Image Asset Metadata framework creates a common guideline for Image-based NFTs such as Logos, Sprays/Decals, Emblems, Banners, and others between games.

## Abstract

The Image Asset Metadata framework is `asset-properties` object in the Public Asset Metadata. This contains the information for Image-based NFTs. This creates a commont metadata format so that game developers can easily parse metadata for image NFTs for display in-game. This allows players to take their image nfts and use them wherever it is applicable.

Image NFTs can be used in many different manners such as rewarding players with emblems, logos, sprays, or decals. Players may also create their own image NFTs that they can bring into their games to show their unique content. Game developers may also use Image NFTs as a way to reward players with in-game artwork.

## Terminology 

* 

## Specification 

We define the Image-based NFT frameworks for different sizes. However, these `subtypes` are merely suggestions when it comes to usage. The developers are free to use the framework for different purposes as intended as long as they adhere to the specific requirements of each subtype. 

Note: these dimensions are not final and can be increased or decreased depending on the community of game developer's requirements.

### Square Subtype

The square subtype requires that the image texture has an aspect ratio of 1:1. It also requires the `textures` array to contain a 256x256 pixel image as the default. Other texture sizes may go up to 1024x1024 and as low as 48x48 pixels.

### HorizontalBanner Subtype

The horizontal banner subtype requires that the image texture has an aspect ratio of 2:1. It also requires the `textures` array to contain a 256x128 pixel image as the default. Other texture sizes may go up to 1024x512 and as low as 96x48 pixels.

### VerticalBanner Subtype

The vertical banner subtype requires that the image texture has an aspect ratio of 1:2. It also requires the `textures` array to contain a 128x256 pixel image as the default. Other texture sizes may go up to 512x1024 and as low as 48x96 pixels.

### Custom Subtype

The custom subtype doesn't have a requirement for aspect ratio and doesn't have a default texture requirement dimension. The creator is free to choose what they want to use in their games and NFT. The custom subtype, however, may not be loadable in other games aside from the creator's game. The dimensions must still be a power of 2.

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
            "description": "Height of the texture"
        },
        "width": {
            "type": "int",
            "description": "Width of the texture"
        },
        "content-type": {
            "type": "string",
            "description": "content type. Can be image/png, image/jpg, image/svg"
        }
    }
}
```

## Rationale

`Unity Textures` require that dimension sizes should be powers of 2 on each side, (that is, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048 pixels (px), and so on). It is possible to use non-power of two dimensions but it requires more memory and is slower for the GPU to sample. To simplify things, the framework is requiring power of 2 for both dimensions. 

This may or may not be the case for `Unreal Engine` textures but for simplicity, we will keep the Power of 2 requirement for now.

`uri` for each texture is what is loaded by the game engine. The `height` and `width` must correspond to the downloaded image texture, otherwise, it will not be loaded. It is up to the game developer on how the texture is used whether it be as a decal, a user image, a logo, a banner, etc. The game developer also determines whether to load the image as a texture and assign it to a material (3D Project) or load it as a Sprite (2D project). 

The game developer will use the `height`, `width`, and `content-type` to determine which texture they wish to load. 

These `subtypes` are not final and may be updated. More `subtypes` may also be proposed by game developers and content creators. 

## Samples

### Square Subtype
```
{
    "name": "Rawrshak Logo Decal",
    "description": "Rawrshak Logo decal usable as a spray, emblem, or decal",
    "image": "<default text icon uri on arweave>",
    "type": "text",
    "subtype": "square",
    "asset-properties": 
    [
        {
            "uri": "arweave.net/<tx>",
            "height": 256,
            "width": 256,
            "content-type": "image/png"
        },
        {
            "uri": "arweave.net/<tx>",
            "height": 512,
            "width": 512,
            "content-type": "image/png"
        },
        {
            "uri": "arweave.net/<tx>",
            "height": 1024,
            "width": 1024,
            "content-type": "image/png"
        }
    ],
    "developer-properties":
    {
        "creator-comments": "Rawrshak Represent!"
    }
}
```

### HorizontalBanner Subtype
```
{
    "name": "Rawrshak Banner",
    "description": "Rawrshak Banner usable in banners, headers, etc.",
    "image": "<default text icon uri on arweave>",
    "type": "text",
    "subtype": "horizontal-banner",
    "asset-properties": 
    [
        {
            "uri": "arweave.net/<tx>",
            "height": 128,
            "width": 256,
            "content-type": "image/png"
        },
        {
            "uri": "arweave.net/<tx>",
            "height": 256,
            "width": 512,
            "content-type": "image/png"
        },
        {
            "uri": "arweave.net/<tx>",
            "height": 512,
            "width": 1024,
            "content-type": "image/png"
        }
    ],
    "developer-properties":
    {
        "creator-comments": "Rawrshak Banner Represent!"
    }
}
```