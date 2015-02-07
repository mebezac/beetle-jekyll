---
layout: single-blog-post
title: "Finished With Week 1 of Course 3 at Tealeaf Accademy"
modified:
categories:
excerpt: So it took a little longer than a week, but I have finished up the first "week" of course 3 at Tealeaf. This course focuses on Test Driven Development (TDD) and the main project is building a NetFlix clone.
tags: []
image:
  feature: tealeaf.png
  teaser: tealeaf-academy-logo.png
  thumb:
date: 2014-03-18T13:21:55-05:00
---

So it took a little longer than a week, but I have finished up the first "week" of course 3 at Tealeaf. This course focuses on Test Driven Development (TDD) and the main project is building a NetFlix clone.

This first week was extremely intense, but I think it is getting me a lot closer to working how an actual developer would. To start this project, we forked a "template" repo of the NetFlix clone. All it has is a basic Rails app with the UI already built. There is a special UI controller that lets you see all of the mockups for the different pages. Your job then is to take those mockups and put working code behind them. I really, really like this approach (which I think is a pretty popular way of doing it) because I am not a professional designer by any stretch of the imagination. I can make some good looking websites, but I borrow a lot of UI elements to achieve them. I don't normally build it completely from scratch. So, having the UI all taken care of helps me focus on what I can do from scratch, which is build a Rails app.

When adding new functionality to the app, we are taking a TDD approach for this course. We are doing our tests with RSpec, which I have to say, I really enjoy. RSpec's syntax is very clean, and makes a lot of sense to me. As a whole, TDD makes a lot of sense to me. Building the Blackjack and Reddit clone apps without automated testing has made me really appreciate it. I can just remember having to hardcode in a hand with 21 to test my Blackjack app and then going back and removing it and putting the appropriate code back. And that's just a simple little game, I can't imagine trying to manage all the moving parts of a full-scale app without automated testing.

This week we added a few different forms to the app, including a signup and login form. This app is styled using Twitter Bootstrap 3. To make adding forms a lot easier, I used the bootstrap_form gem, which you can find here: https://github.com/bootstrap-ruby/rails-bootstrap-forms. The only issues is that the current gem version is for Bootstrap v2, so if you want the gem to work with Bootstrap 3 styles, you need to set GitHub as the source for this gem. I did that by adding this line to my Gemfile:

{% highlight ruby %}
  gem 'bootstrap_form', :git => "git://github.com/bootstrap-ruby/rails-bootstrap-forms"
{% endhighlight %}

Figuring that out was actually what took the most time this week. Building the app up and implementing new features in a TDD way was relatively easy, but getting the styling to match perfectly to the pre-made UI took some extra work. Going through that though forced me to dig deep into Rails' form helpers to figure out what was going on behind the scenes. It helped demystify some of that "Rails magic" for me.

For this course, I have also switched over completely to a Linux environment, instead of running a virtual machine on my Windows laptop. I went with [Linux Mint](http://www.linuxmint.com/) running the Cinnamon desktop. I am really liking it. Also, since I was setting up a new development machine, I used rbenv as my ruby version manager, instead of RVM. I really like rbenv, it seems cleaner and more intuitive than RVM to me. For me, RVM always seemed a little hacky and overfilled with features that I never used.

I've also started tracking my coding using [codeivate](http://www.codeivate.com/) which is a pretty awesome plugin for Sublime Text that tracks when you're programming, what language you're using, and a bunch of other stuff. You can check my profile out here:

[![](http://www.codeivate.com/users/mebezac/signature.jpg)](http://www.codeivate.com/users/mebezac)

