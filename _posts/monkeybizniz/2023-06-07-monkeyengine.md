---
title: MonkeyEngine
layout: post
summary: In a time before Unity3D was a viable option, we built our own custom 2D multi-platform Game Engine that we've used for most of our projects since. 
author: Vincent
date: '2019-05-22 14:35:23 +0530'
category: monkeybizniz
thumbnail: "/assets/img/posts/code.jpg"
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll,
  devlopr-jekyll tutorial,best jekyll themes
permalink: "/projects/monkeybizniz/monkeyengine/"
usemathjax: true
---

Back in 2013, the world of mobile game development looked different than it does now. You could use Unity to build your game, but to actually compile it for mobile, you had to pay for a license per platform, it wasn't included in the free version yet. At Monkeybizniz, we did create the [Game of the Golden Age](https://spelvandegoudeneeuw.wordpress.com/het-spel/) in Unity, but every time Apple released a new version of iOS, it took Unity at least a few weeks to catch up and support the new version, which resulted in an unhappy client for us, because he couldn't run the game anymore and we couldn't fix it until Unity released a new update.

This led us to the decision to build our own in-house engine, called the MonkeyEngine. Because the engine was only meant to be for our own internal projects, we could keep it as simple and lightweight as we wanted, only adding the functionality that we needed. Especially back then, when mobile devices weren't as powerful as they are now, Unity didn't always run smoothly on low-end devices. Our engine had an advantage there.

Some features of the engine:
- It had to run on iOS and Android. Support for Win32 and macOS was added later as well. Xbox One is partially supported as a personal experiment.
- We were a 2D-only game studio, so we only needed a 2D image and font rendering system.
- Support for audio.
- Support for multi-touch and touch keyboard input.
- Particles.
- Accelerometer on mobile devices.
- A movie player.
- Frame-by-frame animations using spritesheets.
- Bone animations using DragonBones.
- Easily define UI animations and transitions.
- UI that easily scales to multiple resolutions, aspect ratios and device orientations.
- Support for save data.

# Tech

We didn't add all these features in one go, but rather on a case-by-case basis, whenever we needed the functionality. The entire engine is built in C++, using a variety of 3rd party libraries for maximum compatibility between different platforms.

### Multiplatform

The first version of the engine only supported iOS, so the structure of the engine could be kept rather simple. There was an Engine class, a Renderer class, a Texture class, an AudioPlayer class, etc. And it used Apple's own framework for some of the core functionality, such as the built-in PNG decoder. When it was time to start supporting Android, I first tried to mix in the Android code with all the iOS code using lots of '#if IOS' and '#if ANDROID' directives. This kept the overall file structure of the engine simple, but the implementation code became unreadable very quickly. I needed a better approach.

For the second version of the engine, 

### Image loading

### Font rendering

Render a piece of text, how hard could it be? Very hard, it turns out.

### Scenes

### UI Scaling

### Audio

### Movie player

### DragonBones

### Data

### Visual Editor

At one point, the MonkeyEngine did have a visual editor called the MonkeyEditor, in which we could create and edit a scene. The editor ran on macOS, and was itself built in the engine, just like in Unity. This meant that the scene preview would look exactly the same in the editor as it would on the device. The scene configuration was stored in an XML file, which could then be loaded in the game at runtime. Sadly, as the engine evolved, it turned out to be too time-consuming to keep the editor up to date as well, so support for it was eventually dropped. The XML files were easy enough to edit by hand, we still only supported 2D and building and running the game on the device only took a few seconds, which was good enough for our needs.

### Tools

### Interesting Engine

Engine development is something that still fascinates me to this day, and there's a lot of things I'd like to improve, I'd like to add support for 3D, physics, Raytracing, consoles, and much more. Not because we needed, but just as a fun project for myself, and to keep learning and improving my skills. That's why I've started working on the [Interesting Engine](/projects/personal/interesting-engine). These days, I wouldn't recommend writing your own engine anymore, not for production purposes (unless you're a big company with enough resources), but I do think every developer should try it for themselves at least once. You will learn so much about the way engines work and what they do for you, making you a better developer in any engine.

