---
layout: single-blog-post
title: I switched from Wordpress to Jekyll
date: 2015-02-06 18:53:58.000000000 -05:00
excerpt: So, it's been a really long time (<em>like a really, really long time</em>) since I've posted, or updated this site at all. Basically it's been because I haven't found the time to blog regularly anymore. The main reason, which is actually a good thing, is that I have been working full-time as the Lead Developer of
published: true
image:
  feature: jekyll.jpg
  teaser: jekyll-square.jpg
---
So, it's been a really long time (_like a really, really long time_) since I've posted, or updated this site at all. Basically it's been because I haven't found the time to blog regularly anymore. The main reason, which is actually a good thing, is that I have been working full-time as the Lead Developer of [bookacoach.com](https://www.bookacoach.com)! It has been a really great experience, and I'm learning so much from actually having my hands on a real production app. I won't get into too many details right now (I'm going to be doing some posts about things I've learned later) but I've gotten to do some pretty cool things so far, like upgrade an app from Rails 3 to Rails 4.2, and a lot of performance tweaks/scaling.

Anyways, I got a little bit of free time so I figured I should start blogging again about the cool stuff I'm learning while working. I also decided it was time for an upgrade to my personal site. I definitely wanted something a little more lightweight/portable as compared to WordPress. Jekyll is perfect for that because I can just write all of my posts in MarkDown and host my site on Github. I'm actually writing this post right now in Sublime Text, which is pretty cool to do as well. Jekyll is designed to make a static site, where as WordPress really isn't. Up until now, I was using a WordPress plugin to export my site to html files and then using a bash script to replace the urls before pushing it my GitHub pages repo. That was a pretty cumbersome process that resulted in some pretty ugly looking HTML and required a whole WordPress installation with a database just for a simple blog. So, like I said, Jekyll is a perfect fit, and I probably should of just been doing it this way from the beginning.

I also found a free theme I really liked called Beetle by [Mokaine](http://mokaine.com/beetle-free-html/). Jekyll's templating system is really simple, so it didn't take me too much time to convert that theme to a Jekyll theme. For now, I'm going to build the site locally and then push the compiled site to my GitHub pages repo. I'm also going to push all off my Jekyll code, including the converted theme in [this repo](https://github.com/mebezac/beetle-jekyll) so anyone who wants to can see how I did it, or maybe get a jump start on using Beetle for their own blog.

Well, time to build this and the rest of my posts and see if it works :)
