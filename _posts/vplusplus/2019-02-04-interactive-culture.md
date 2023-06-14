---
layout: post
title:  "Interactive Culture"
summary: "A collection of cultural, location-based apps about poetry."
author: Vincent
date: '2019-05-22 14:35:23 +0530'
category: vplusplus
thumbnail: /assets/img/posts/logo_sic.png
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /projects/vplusplus/interactive-culture/
usemathjax: true
---


In collaboration with [Stichting Interactive Culture](http://www.interactive-culture.nl/) I'm building a collection of location-based cultural apps.

# VERS

It started out with one app called VERS, available on [iPhone](https://apps.apple.com/us/app/vers/id1436304425) and [Android](https://play.google.com/store/apps/details?id=com.vplusplus.DichtOpUtrecht) devices. Using the app, you can walk through the city of Utrecht, hunting for poems. You can see the locations of the poems in the app, but you will only unlock them when you arrive at that location. These locations are the places where the poet intended for the poem to be read, or the place the poem is about. You can then either read the poem, or you can listen to an audio version of it. You can also save it in a 'Bundle' for you to read or listen to later.

<img src="/assets/img/posts/interactiveculture_screenshot.png" class="img-fluid">

Since this first prototype, that only had a handful of poems in Utrecht, we've added literally hundreds of new poems, all over the country. All of these have a title, location, text, but also an audio fragment and possibly images. If we were to bundle all of the content within the app itself, it would very quickly grow very large, which is undesirable for a phone app. In addition, we didn't want to have to release a new version of the app every time we wanted to add new content.

To solve both of these problems, I created a web back-end with a database and a REST API for us to use. Using this web portal, we can easily add new poems, setting their location, title, content, and uploading the audio fragments and images. There's also support for different 'collections' that use separate map icons, to for example distinguish between poems in Utrecht and Groningen. Whenever the app is opened on the phone, it automatically checks for new content on the server and downloads it the updates. To save data, audio and images are only downloaded when they are actually needed, and cached on the device for future listening.

<img src="/assets/img/posts/interactiveweb.png" class="img-fluid">

# More apps

Not only has the VERS app gained a lot of content, it's also spawned several related projects. There's been a version of the app for a festival in Grignan, France called Toutes Lettres, there's a version of the app in Basel, Switzerland called Hör Mal, and we're working on a variation of the app for the city of Amersfoort, in addition to our internal Interactive Culture test app, that we use to prototype and test new functionality. 

To accomodate these different apps, I've created a Core library project that contains all the functionality that is shared between the different apps. For example, it contains the Map page, the Bundle functionality, the Audio player component, the downloading and updating of content, and the location service that unlocks content based on the user's location. Whenever functionality is added to the Core library, all the apps can profit from it. I've set up an automated Build pipeline in Azure that triggers whenever I push to Git, and the resulting library is then packaged into a NuGet package and uploaded to our private NuGet repository. This way, every app that uses the Core library can update to the new version at their own pace, instead of breaking every time I make a change. The individual apps can then take this shared functionality and customize it to create their own look and feel.

To set up a new app, all we have to do now is create a new project add the Core NuGet package to it and start importing functionality, and the app can be up and running in a matter of minutes.

Every app uses the exact same web back-end to get their content, so we can manage everything in one place. Individual apps are authorized using a secure OAuth access token, so every app only has access to their own data. This also prevents other people from using the API to access and download all our content.

# Tech

The apps were originally built in Xamarin and are currently being upgraded to use .NET MAUI (along with a visual overhaul to make it look pretty), to allow us to use the same codebase for both the Android and iOS apps. The web back-end and REST API are built using the Blazor framework, uses a SQL database for storage and is hosted in Azure for ease of use. 

We use App Center for internal distribution of test versions, where every time changes are pushed to a specific branch in Git, it triggers the build pipeline for both an iOS and Android version, and automatically distributes it within our team. Every team member is then notified with an email and can download the app. Crash reports are also collected through App Center, making it easy to debug any issues that might arise.



