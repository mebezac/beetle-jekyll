---
layout: single-blog-post
title: "Week 4 Completely Done, Ajax-ing It Up, Github Branches, Etc."
modified:
categories:
excerpt: So I'm completely done with the fourth and final week of the first Tealeaf session - Introduction to Ruby and Web Development. I think I can honestly say that I have learned more this month than in all of the previous months trying to learn on my own. Tealeaf is the perfect balance of guided instruction and self-learning. I didn't have to quit my job and move across the country to do it and because I had time, I could work ahead and catch up with the suggested pace.
tags: []
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
date: 2014-01-25T17:36:44-05:00
---

So I'm completely done with the fourth and final week of the first Tealeaf session: Introduction to Ruby and Web Development. I think I can honestly say that I have learned more this month than in all of the previous months trying to learn on my own. Tealeaf is the perfect balance of guided instruction and self-learning. I didn't have to quit my job and move across the country to do it and because I had time, I could work ahead and catch up with the suggested pace.

In these four "lessons" I've gone from understanding just the basics of programming to feeling very confident in programming in Ruby, Object Oriented Programming, debugging, using Sinatra, GitHub, and Heroku, and web development. I can't wait to start the next session so I can start learning rails.

Anyways, in the last week I finished up adding ajax to my Blackjack web app. The last bit I had to figure out was how to post data using ajax in a way that I could use the data and store it in the session. I wanted to do this so that I could have the player bet from the game page and not have to refresh the page but just update it with ajax.

I was having problems figuring out how to do that, but with some help from the great people in the Tealeaf forums, I got it. I was trying to use this bit of code:
{% highlight javascript %}
  $(document).on('click', '#game-page-bet input#bet-button', function() {
    $.ajax({
      type: 'POST',
      url: '/game/page/bet',
      data: {
        bet: $('#bet-amount').serialize()
            }
    }).done(function(msg) {
       $('#game').replaceWith(msg);
    });
    return false;
  });
{% endhighlight %}

The problem with that is that <code class='highlight'>.serialize()</code>  returns the key:value pair. If I wanted to explicitly assign that value to bet, I should have used <code class='highlight'>bet: $('#bet-amount').val()</code>. The way I fixed it though was to just set data equal to the <code class='highlight'>.serialize()</code> like this:

{% highlight javascript %}
$(document).on('click', '#game-page-bet input#bet-button', function() {
    $.ajax({
      type: 'POST',
      url: '/game/page/bet',
      data: $('#bet-amount').serialize()
    }).done(function(msg) {
       $('#game').replaceWith(msg);
    });
    return false;
  });
{% endhighlight %}

Running into this problem also helped me learn to do some other useful things though. First I got to branch my web app repo on GitHub so that I could test a way to make the bet form post using ajax while keeping my working code clean and the same. Also, I learned how to push that separate branch from the same local directory to a separate Heroku app to test it. After fixing the issue I got to submit a pull request to myself and then merge the testing branch into my master branch. All in all, making those mistakes helped me learn a lot more!

Anyways, you can play the final version here: [http://zacjack.herokuapp.com/](http://zacjack.herokuapp.com/) and check out the code here: [http://github.com/mebezac/Blackjack_web_app/](http://github.com/mebezac/Blackjack_web_app/)
