---
layout: single-blog-post
title: "Week 3 Done and 4 Basically Done at Tealeaf Academy"
modified:
categories:
excerpt: So, I just wrapped up my Blackjack web app. You can play it here <a href="http://zacjack.herokuapp.com" target="_blank">http://zacjack.herokuapp.com</a>. I'm pretty proud of the little UI I came up with, including the scoreboard. I would definitely not consider myself a CSS expert, but I think I did a fairly good job.
tags: []
image:
  feature: tealeaf.png
  teaser: tealeaf-academy-logo.png
  thumb:
date: 2014-01-24T16:32:37-05:00
---

So, I just wrapped up my Blackjack web app. You can play it here: [http://zacjack.herokuapp.com](http://zacjack.herokuapp.com). I'm pretty proud of the little UI I came up with, including the scoreboard. I would definitely not consider myself a CSS expert, but I think I did a fairly good job.

The project itself is written in Ruby and utilizes the [Sinatra](http://www.sinatrarb.com/) framework. The styling was made a whole lot easier for me by using the wonderful Flatly theme from [bootswatch](http://bootswatch.com/). Bootswatch just takes the awesomeness that is Twitter Bootstrap and makes it look better.

Week 3 was the meat of the project, basically building the entire game from scratch. It seems a little tedious at first that you have to deal with every get and post request separately. It didn't take long to get in that frame of mind though, and it really does allow you to easily build some pretty powerful applications.

In building the web app, I also ran into the issue of browser compatibility. I do all of my coding on a Ubuntu virtual machine and testing using the Chromium browser. I used some of the new HTML5 form input types including the number type. I like it because it only allows entering numbers and you can dynamically set the minimum and maximum values allowed. So, I used that to make sure that people couldn't bet negative numbers or more than the amount of money they had. It worked beautifully. The problem that I didn't realize until a friend of mine played on Firefox was that the number input type is not yet supported in that browser. There's a good resource [here](http://www.w3schools.com/html/html5_form_input_types.asp) showing the new input types and which browsers they work in.

That wasn't too hard of an issue to address, I just had to add some additional logic to my main.rb file that checked to make sure the bet was a valid number and in the acceptable range.

After getting that straightened out, I added the ajax capabilities to the game so the "ugly" urls like /game/player/bet didn't show up in the address bar. Currently I am also attempting to "ajax-ify" the betting process. Right now it only works because it refreshes the page.
