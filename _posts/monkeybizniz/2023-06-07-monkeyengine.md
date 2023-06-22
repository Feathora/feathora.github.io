---
title: MonkeyEngine
layout: post
summary: In a time before Unity was a viable option, we built our own custom 2D multi-platform Game Engine that we've used for most of our projects since. 
author: Vincent
date: '2023-05-22 14:35:23 +0530'
year: '2013 - 2016'
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
- Support for localisation to different languages.

# Tech

We didn't add all these features in one go, but rather on a case-by-case basis, whenever we needed the functionality. The entire engine is built in C++, using a variety of 3rd party libraries for maximum compatibility between different platforms.

### Multiplatform

The first version of the engine only supported iOS, so the structure of the engine could be kept rather simple. There was an Engine class, a Renderer class, a Texture class, an AudioPlayer class, etc. And it used Apple's own framework for some of the core functionality, such as the built-in PNG decoder. When it was time to start supporting Android, I first tried to mix in the Android code with all the iOS code using lots of '#if IOS' and '#if ANDROID' directives. This kept the overall file structure of the engine simple, but the implementation code became unreadable very quickly. I needed a better approach.

For the second version of the engine, the different platforms were separated into their own files and projects, with a platform-independent project for shared functionality. Whenever a class needs a platform-specific implementation, a virtual base class is used for shared functionality and their public-facing API. Every platform then implements their own version on top of the base class, overriding the virtual functions. To support a new platform, all you need to do is provide a new implementation on top of the base class.

Games that use the engine don't have to worry about using the right implementation class. Because every platform uses the same names for the classes, the compiler will select the right one for the right platform. The downside to this is that you end up with several files and classes that use the same name, only they're in different folders. This might be a trade-off between ease of use in a game and during development of the engine. I have a feeling there might be a better approach, but that's something I'd like to explore in a new version.

### Image loading

For image loading, the engine uses the libpng library on all platforms. The first version of the engine used Apple's own PNG loader, but to keep things consistent between platforms, it now uses libpng everywhere.

### Font rendering

Render a piece of text, how hard could it be? Very hard, it turns out. Here, I discovered things like string encoding to support the entire Unicode standard instead of just ASCII, font glyphs, kerning, and all things related to fonts.

I tackled the string encoding by using the Iconv library. For ease of use, and to save memory, the engine uses the standard std::string to pass around pieces of text. Only before rendering a string is it converted to an std::u32string using Iconv. This way, I can easily iterate over all the characters, count letters and words, and use the characters easily to find the right glyph to render.

For dynamic fonts, the engine uses the Freetype library, which supports all major font files, contains all the rules for how each individual character glyph should be rendered, such as where they should be positioned relative to each other, and can output them to images. The images can then be rendered like normal by the rendering system.

### Scenes

Game scenes for the engine can be defined in a simple XML structure. The scene file has support for the basic visual types, such images, buttons, bone sprites and text labels. They can be placed using absolute and relative positions. A custom pivot point can be defined. And there's support for enter and exit animations for when there's a transition to a new scene, including preserving objects between scenes. 

### UI Scaling

The entire game can be constructed in terms of a single reference resolution, for example 1280 by 720. Using a scaling and translation matrix, the entire scene is then scaled to the actual size and aspect ratio of the target device. You can specify settings per visual element to determine how it scales and translates. For different aspect ratios, you can define so-called anchor points to for example keep an element in the bottom-right corner, instead of moving to the middle. This functionality is comparable to responsive design webdesign, where a web page needs to look good on hundreds of different devices and resolutions.

With the iPhone 4, Apple introduced the world to Retina displays. Same screen size, but twice the pixel density, which meant all assets needed to be twice as big in resolution to get the best out of the display. To make it easy for developers, Apple uses the suffix @2x for their filenames. iOS will then automatically select the right asset for the right display. The MonkeyEngine supports this suffix as well, and has extended it to other platforms. When loading a file, it will determine the screen scale in relation to the target reference, and automatically select the high or low resolution image. When the iPad Pro came out, we've even added support for the @3x suffix and higher.

### Audio

The engine has support for stereo audio fragments. On Windows and iOS, the engine uses OpenAL to play the audio fragments, while on Android, where OpenAL sadly wasn't available at the time, it uses OpenSLES. Functionality is kept the same on all platforms however.

### Movie player

The engine is able to play complete movies stored in mp4 format. It does this by using the FFmpeg library, the same library VLC uses to decode and play video's. A movie is essentialy nothing more than a frame-by-frame animation. When playing a movie, the engine spawns a thread that will read and decode the video file, rendering each frame to a texture buffer. These texture are then simply displayed as normal textures by the rendering engine, so they can integrate perfectly with the rest of the scene.

### DragonBones

The engine has support for bone animations made in [DragonBones](https://dragonbones.github.io/en/index.html). DragonBones can export to JSON files, which are then read by the engine, and integrated into the normal rendering engine as regular textures with weighted bone transformations.

### Data

There's support for several types of data storage in the engine. It uses libxml2 for XML files, jsoncpp for JSON files, and it has SQLite database support in the form of libsqlite3. In the engine, XML is used used for configuration files, JSON is used by the texture manager, while save data is stored in an SQLite database.

There's also support for downloading data from an API using the cURL library.

### Visual Editor

At one point, the MonkeyEngine did have a visual editor called the MonkeyEditor, in which we could create and edit a scene. The editor ran on macOS, and was itself built in the engine, just like in Unity. This meant that the scene preview would look exactly the same in the editor as it would on the device. The scene configuration was stored in an XML file, which could then be loaded in the game at runtime. Sadly, as the engine evolved, it turned out to be too time-consuming to keep the editor up to date as well, so support for it was eventually dropped. The XML files were easy enough to edit by hand, we still only supported 2D and building and running the game on the device only took a few seconds, which was good enough for our needs.

### Tools

I wanted to make it as easy as possible to use the engine, that's why I've also made a few tools available.

Most important is the AssetValidator tool. It runs during the build phase, and automatically scales every high-res image to a low-res version for use on low-end devices, so we don't have to export them all manually. It emits warnings if there is no high-res version available. It also checks if there are any localized images, and emits an error if it's missing from one of the localizations. The entire image database is then written to a JSON file, which is read by the engine at runtime for easy and quick asset lookup.

On mobile, the App Icon needs to be included in lots of different resolutions. Instead of having to export it to every resolution every time, I made a tool that takes the high-resolution image, and exports it to all required resolutions for every platform, using the required naming conventions and folder structures.

There's also a tool to automatically convert audio files into the right format for every platform, which used to be .caf for iOS and .wav for Android.

To set up a new development machine, there's a batch file that downloads all the relevant 3rd-party libraries from git and compiles them for the target platforms. 

### Interesting Engine

Engine development is something that still fascinates me to this day, and there's a lot of things I'd like to improve, I'd like to add support for 3D, physics, Raytracing, consoles, and much more. Not because we needed, but just as a fun project for myself, and to keep learning and improving my skills. That's why I've started working on the [Interesting Engine](/projects/personal/interesting-engine). These days, I wouldn't recommend writing your own engine anymore, not for production purposes (unless you're a big company with enough resources), but I do think every developer should try it for themselves at least once. You will learn so much about the way engines work and what they do for you, making you a better developer in any engine.

### Games

Most of the games we've made since building the engine, have been made in the engine, adding features to the engine whenever the need arose. A few examples include:
- [LUTS](/projects/monkeybizniz/luts) was the first game we made in the MonkeyEngine and included all the basics, including support for DragonBones animations. 
- For [Prik](/projects/monkeybizniz/prik) we added support for Android and Localisation.
- In [Kucheza](/projects/monkeybizniz/kucheza) we added support for running on Windows via the Win32 API.

