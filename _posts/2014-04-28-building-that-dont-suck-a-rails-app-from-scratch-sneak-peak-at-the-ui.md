---
layout: single-blog-post
title: Building That Don't Suck (a rails app) from scratch. Sneak Peak at the UI.
date: 2014-04-28 15:03:58.000000000 -04:00
categories:
tags: []
excerpt: So, now that I'm done learning how to build Rails apps, it's time to use my knowledge to build some real applications. The first project I've chosen to work on is called That Don't Suck. Basically, the idea for the website is a community of people that help each other find things that don't suck.
type: post
published: true
image:
  feature: pat.jpg
  teaser: tds-teaser.png
  thumb:
---
So, now that I'm done learning how to build Rails apps, it's time to use my knowledge to build some real applications. The first project I've chosen to work on is called That Don't Suck. Basically, the idea for the website is a community of people that help each other find things that don't suck.

For example, when designing websites for people, one of the things that is really hard to find is free stock photos that don't suck. I could login to That Don't Suck and first search to see if that topic exists yet. If it does, I can start looking through the suggestions people have added already. If not, I can add the topic and ask people to make suggestions for me.

Once people have made some suggestions, I, and other people looking at the suggestions can vote and comment on each one. The one with the most votes will be at the top so people can easily see which one the community suggests.

There's also a bunch of other features I will be adding, such as a notification system, social networking, gamification through points and badges, full text search, and more.  Right now, though I am focused on getting the UI looking the way I want it to. This is because, once I have the UI set up, I can then focus on building up the functionality without worrying about the look and feel quite as much since that is already taken care of. Also, forcing myself to create mockups of all the features beforehand helps me organize all the features I am looking to add so I can start thinking about the different pieces I am going to need and the best way to put those into the models and controllers.

For the UI, I decided to use Twitter Bootstrap since it allows you to create a pretty attractive UI without much heavy lifting on your part. I am using the [Darkly](http://bootswatch.com/darkly/) Bootstrap theme from Bootswatch because you still have all the functionality of Bootstrap without looking quite as generic and boring as the plain vanilla version.

[![]({{ site.url }}/img/TDS-2014-04-28-300x150.png)]({{ site.url }}/img/TDS-2014-04-28.png)

The UI is pretty much done now, so I will now move on to the fun part, adding real functionality to the project! I will be using TDD to drive the implementation of my code. I'll try to update at least once a week about my progress and share any tips I might stumble upon.
