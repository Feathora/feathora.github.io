---
layout: post
title:  "Graduation"
summary: "For my graduation, I wrote my Master Thesis on the possibilities of weather in games. I then built a proof of concept in which you have to predict the weather and take advantage of it in order to beat your opponent."
author: Vincent
date: '2019-05-22 14:35:23 +0530'
year: '2013'
category: student
thumbnail: /assets/img/posts/code.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /projects/student/graduation/
usemathjax: true
---


During my half-year Graduation project in 2013, I wrote a Master Thesis where I researched the possibilities of weather in games. I noted that there were hardly any games that used the weather as an actual gameplay mechanic. It's mostly either used to create atmosphere, or as a puzzle. My entire thesis can be found below.

<object data="{{ site.url }}{{ site.baseurl }}/assets/img/BoomanVincentWeatherInGames.pdf" width="1000" height="1000" type="application/pdf"></object> 

# Proof of Concept

As a proof of concept, I also designed and built a vertical slice for a game, where the weather is used as a gameplay mechanic. It's a simple strategy game where you have to constantly look at the sky and clouds, to try to predict whether it's going to be sunny or raining. This has an impact on which kind of food you can grow, and how well your units perform. The entire concept can be found in the Thesis.

When I started this project, I began building the game in C++ using OpenGL, but quickly realised setting up a whole rendering engine would cost me too much time for now. So I switched to Unity. I also made the decision to implement this concept as a networked multiplayer game, so I could focus on the mechanic itself, instead of also having to write an AI that can respond to the weather and their opponent. I used [SmartFoxServer](https://smartfoxserver.com/) for the multiplayer implematation. 

To simulate the weather effects on the ground and the units, I used an orthographic camera placed above the entire scene to render the clouds, along with their humidity level, into a RenderTexture. This texture is then passed along to the Terrain shader, to show whether the ground is wet or dry. The texture is also sampled by the units and farms, to determine the weather and the corresponding effects at their location. Sampling this data from the C# side turns out to be very slow due to memory constraints, so I wrote a simple plugin in C++ that uses DirectX to sample the texture data. Nowadays, I would probably use something like a Compute Shader for this, but I don't remember those being available or at least very well-known back then.

I've uploaded the Unity project of this game to [Github](https://github.com/Feathora/Lightning) for anyone that's interested.




