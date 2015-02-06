---
layout: single-blog-post
title: Finished Week 3 of Course 3 at Tealeaf Academy
date: 2014-03-26 10:29:23.000000000 -04:00
categories:
tags: []
excerpt: The first part of this week basically dealt with refactoring RSpec code by using macros and shared examples. These are really helpful because common things like checking to make sure an unauthenticated user is redirected to the sign in page if they try to do an action that requires them to be logged in can be extracted out to a single shared example. This means if for any reason down the road if you need to change the behavior of what happens when an unauthenticated user tries to perform a privileged action, you only need to change that shared example instead of having to go and change it in all of your test files.
type: post
published: true
meta:
  dsq_thread_id: '2515247657'
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
---
The first part of this week basically dealt with refactoring RSpec code by using macros and shared examples. These are really helpful because common things like checking to make sure an unauthenticated user is redirected to the sign in page if they try to do an action that requires them to be logged in can be extracted out to a single shared example. This means if for any reason down the road if you need to change the behavior of what happens when an unauthenticated user tries to perform a privileged action, you only need to change that shared example instead of having to go and change it in all of your test files.

The second half of the week, we got into doing feature specs using Capybara. This is basically testing the actions of an actual user clicking around on your UI and performing the actions of a real user. This is important because it allows you to test the integration of you code at a higher level than just the model and controller levels. This allows to test basically real world behaviors.

Doing feature specs will take a little getting used to for me, but I think I am getting the hang of it. The biggest thing for me to get used to is the fact that you do multiple things all within one <code class='highlight'>scenario</code> block, where in the model and controller specs, you separate just about every action and permutation into it's own test.

Feature specs are a whole different kind of testing though. So, I just need to get used to switching context from testing the small units to testing everything together the way a user would use it. I'm definitely starting to get my head around this TDD thing now though!
