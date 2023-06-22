---
layout: post
title:  "Interesting Engine"
summary: "Re-imagining the original MonkeyEngine with 3D support, new platforms and a brand new codebase, based on everything I've learned since."
author: Vincent
date: '2023-05-22 14:35:23 +0530'
year: '2023'
category: personal
thumbnail: /assets/img/posts/code.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /projects/personal/interesting-engine/
usemathjax: true
---


It's been a few years since I've built the original MonkeyEngine. I've learnt a lot since then, and I see quite a few things I'd like to improve or change. That's why, also as a technical challenge to myself and to keep learning new things, I've started on a new version of the engine, called the Interesting Engine. 

# Research

As research, I've read the great book [Game Engine Architecture](https://www.gameenginebook.com/) by Jason Gregory, one of the Engine Developers at Naughty Dogs. In the book, he breaks down a lot of the different components that make up a game engine, and shows the different approaches game engines can take to tackle certain challenges. This Interesting Engine project is a fun way for myself to try out some of the theory from the book.

As a first step, I'm currently focussing on the rendering pipeline. Where the MonkeyEngine only supported 2D (although it did have some support for 3D meshes at some point), this time I want to be able to create a full 3D world in my engine. I recently got hold of a Nintendo Switch Development Kit, so I'm combining these two.

What I have so far, is a simple renderer that can load meshes, materials with shaders, a skybox, and a simple scene in which you can move around with the camera. This is all built in C++, using the NVN library, Nintendo's own low-level rendering library that is optimised for the Nintendo Switch hardware, and is quite similar in use to DirectX 12. 

<img src="/assets/img/posts/trialsofaswitch.jpg" class="img-fluid">

For further research, I've also been getting into [DirectX 12](https://www.3dgep.com/learning-directx-12-1/), [Raytracing](https://raytracing.github.io/), and looking into how I can [combine the two](https://developer.nvidia.com/rtx/raytracing/dxr/dx12-raytracing-tutorial-part-1). 

# Mesh Loading

I needed a way to get mesh data into my engine. One possible solution that I've used in the past, is to implement the Assimp library to load an FBX file. But for now, that felt a bit like overkill, though I'll probably add it in the future. Instead, I wrote a small exporter in Unity, that simply takes an array of all the meshes I want to export, and exports them into a binary file, containing only the data I actually want to use. In this case, the vertices, indices, normals and UV's. This binary file can then be read directly in the engine.

```c#
foreach(var mesh in meshes)
{
    var data = new List<byte>();

    data.AddRange(StructToBytes(mesh.vertexCount));
    data.AddRange(StructToBytes(mesh.GetIndexCount(0)));

    for (int i = 0; i < mesh.vertexCount; i++)
    {
        data.AddRange(StructToBytes(mesh.vertices[i]));
        data.AddRange(StructToBytes(mesh.normals[i]));
        data.AddRange(StructToBytes(mesh.uv[i]));
    }
    
    foreach(var index in mesh.GetIndices(0))
    {
        data.AddRange(StructToBytes(index));
    }

    File.WriteAllBytes(Path.Combine(targetDirectory, $"{mesh.name}.mesh"), data.ToArray());
}
```

