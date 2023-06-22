---
layout: post
title:  "HKU Projects Platform"
summary: "A student once asked if I could show them what other students made in previous years. I realised I couldn't easily do that, so I made a website that could."
author: Vincent
date: '2022-05-22 14:35:23 +0530'
year: '2021'
category: hku
thumbnail: /assets/img/posts/hkuberproject.png
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /projects/hku/hkuberproject/
usemathjax: true
---

When I told my studens what the assignment would be for their very first Unity classes, someone asked me if I could show them what other students had made in previous years, so they'd have an idea what was expected of them. Sadly, I realised I couldn't easily show them, not without digging through all the submissions, downloading everything and figuring out who made what. I thought it was a shame to let all those beautiful projects that are made every year go to waste, so I made a website to preserve them. This website is still live and can be found [here](https://projects.hku.nl).

The front page immediately shows a feed of all the latest and greatest student projects, and can be filtered on which course or project they made the project for. The overview uses widgets from itch.io, because by using itch.io as platform for the projects, the students automatically have complete control over their project pages, including support for downloads, and a web player.

I wanted to make this website as easy as possible for the students to use. That's why I've hooked up the login to the LDAP server of the HKU, allowing students to login with their existing student mail and password. After that, they can link their itch.io account and create a project. It will then automatically show up in the overview.

The whole website is built in C# and some HTML/CSS using Blazor, to easily manage the front-end and back-end in one place. It uses a MySQL database for storage and runs on an Ubuntu server at the HKU. I've open-sourced the entire project on the [HKU Git](https://git.hku.nl/vincent.booman/hkuberproject) so the students could see how it's built and possibly make their own contributions as well.

# Online Showcase

During their third and fourth year, the students work on large projects for half a year. Normally, at the end of every half year, there's a Showcase event at the HKU, where they all present their projects. But then Covid-19 struck, and we couldn't go to school anymore, so we had to find a different way to do our Showcase. The solution turned out to be this projects website, to which I added 'Showcase TV'. Now we could host complete streaming events on the website.

<img src="/assets/img/posts/projects_showcase.jpeg" class="img-fluid">

I've added support for streaming using either YouTube or Twitch. An event can host multiple different 'TV Channels', so we can have multiple streams simultaneously, each with their own separate chat, so viewers can leave comments on what they see. There's also an Agenda, so you can see which project is up next, you can visit their respective itch.io pages from here, and you can quickly switch between channels. In addition to supporting one stream per channel, we can also use a separate stream per item, so every student project can have their own stream, for example. The web page then automatically switches between streaming sources, no view interaction needed.

We as admins have a separate admin page, where we can create new events, create all the channels and make the agenda, so the entire system is future-proof for whenever we want to host another streaming event.


