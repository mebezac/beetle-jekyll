---
layout: single-blog-post
title: Done With Week 6 of Course 3 at Tealeaf Academy
date: 2014-04-15 14:40:55.000000000 -04:00
categories:
tags: []
excerpt: It's crazy to think about it, but I only have 2 weeks left until I'm done with all 3 courses at Tealeaf. I've learned an absolutely crazy amount and I'm so excited to get working on my own projects and look for a job in my new career.
type: post
published: true
meta:
  dsq_thread_id: '2616536821'
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
---
It's crazy to think about it, but I only have 2 weeks left until I'm done with all 3 courses at Tealeaf. I've learned an absolutely crazy amount and I'm so excited to get working on my own projects and look for a job in my new career.

This week was no different than previous weeks in this course, we learned a whole lot. We covered everything from setting up some admin permissions and restrictions, file uploading, the fog and CarrierWave gems, Amazon S3, and credit card processing using Stripe and a custom form.

When we were setting up some admin permissions, we needed a way to check if a user were an admin or not. To differentiate "normal users" from admins, we added a Boolean column to the users table. Something that I wasn't aware of in Rails, is that if you have a Boolean column on a table, it automatically gives you access to a handy method. Say you added a Boolean column with the name <code class='highlight'>admin</code>. You now have access to the <code class='highlight'>admin?</code> method. So you can call it like this <code class='highlight'>user.admin?</code> and it will return the Boolean value of that column. Pretty handy!

In the second half of the week I ran into an issue using the Figaro gem to set my Stripe API keys. I was getting a parsing error when trying to start up the rails server. If you run into this issue, check your text editor's settings. For me, it was simply because Sublime Text had the indentation setting for the <code class='highlight'>application.yml</code> file set to Tabs instead of spaces.

Learning how to use Stripe was a lot of fun. Having the ability to accept credit cards right on my site in my own form without having to worry about PCI compliance or anything else like that is really powerful. There aren't a lot of projects to work on that won't need payment processing of some sort. Setting up the custom form for Stripe was also a lot of fun because I got to brush back up on my JavaScript and jQuery skills.

I've got a number of ideas for projects to start on as soon as I'm done with Tealeaf to bolster up my portfolio. There's one leading in my mind, but I'll post about that more once I get started :)
