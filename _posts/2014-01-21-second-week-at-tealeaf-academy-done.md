---
layout: single-blog-post
title: "Second Week at Tealeaf Academy Done!"
modified:
categories:
excerpt: So I'm playing catch up a bit at Tealeaf Academy. I originally signed up to do the pre-course work in January and then join the February session. I got the pre-course work done so I just decided to jump in with the January session (but be a few weeks behind).
tags: []
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
date: 2014-01-21T17:09:30-05:00
---

So I'm playing catch up a bit at Tealeaf Academy. I originally signed up to do the pre-course work in January and then join the February session. I got the pre-course work done so I just decided to jump in with the January session (but be a few weeks behind).

Tealeaf is set up awesomely in that each session is broken down into weeks. All the content, discussion, and assignments are self contained within their own week. So, when I decided to jump in behind the live session and start week 1, I was pleasantly surprised to  find a good handful of students at the same place that I was, so I didn't have to worry about asking questions about things that the live session was already way past.

Week 2 for me was a lot easier than week 1. Week 2 is all about Object Oriented Programming (OOP). OOP just makes sense to me and is a much cleaner way to do things (probably one of the reasons I was drawn to ruby). So, this "week" of material only took me a few hours to do.

We were tasked with implementing Blackjack again, but this time using OOP. You can find mine here: https://github.com/mebezac/Blackjack_OOP

Something cool I learned about was breaking some pieces of your code into smaller files and then using <code class='highlight'>require_relative</code> to include the code in another file (as long as the files you are requiring are in the same folder). I did this with helpers.rb and card_game.rb in my Blackjack implementation.

{% highlight ruby %}
  # Helper Functions

  def say(msg)
    puts "=> #{msg}"
  end

  def press_enter_to_continue
    say "Press enter to continue"
    gets
  end
{% endhighlight %}

I split off 2 functions that I use a lot into a helper file so that they didn't clutter up and distract from my actual Blackjack file.

{% highlight ruby %}
  # Card and Deck class that can be reused in other card games.

  class Card
    attr_reader :suit, :value
    def initialize(v, s)
      @suit = s
      @value = v
    end

    def show_card
      "#{value} of #{suit}"
    end
  end

  class Deck
    attr_accessor :cards

    def initialize
      @cards = []
      suits = ["Clubs", "Spades", "Diamonds", "Hearts"]
      values = ["Ace", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Jack", "Queen", "King"]
      values.each do |value|
        suits.each do |suit|
          @cards << Card.new(value, suit)
        end
      end
      shuffle
    end

    def deal
      cards.pop
    end

    def shuffle
      cards.shuffle!
    end
  end
{% endhighlight %}

This is the Card and Deck class that I use in Blackjack. I split these off because they are not restricted to Blackjack like the other classes are. This means if in the future I wanted to program a new card game, I could just include this file to bootstrap my game with Cards and a Deck.

Now moving on to a web implementation using Sinatra!
