---
layout: single-blog-post
title: "First week of Tealeaf Academy is done!"
modified:
categories:
excerpt: So I just finished up the last exercises for Pine's book <em>Learn to Program</em>. This was part of the curriculum for the first week of Tealeaf Academy's Introduction to Ruby and Web Development course.
tags: []
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
date: 2014-01-20T17:50:56-05:00
---

So I just finished up the last exercises for Pine's book _Learn to Program_. This was part of the curriculum for the first week of Tealeaf Academy's Introduction to Ruby and Web Development course.

I think I've learned more in this week than in all the other months of trying to learn programming on my own. Ruby is an awesome language to learn as it is "relatively" easy and very readable. I tried to teach myself python before taking this class so I had some programming ideas already in my head. One thing that took some getting used to was variable scope.

To illustrate what I'm talking about take a look at this code:


{% highlight ruby %}
# Variable Scope
num1 = 13
puts "At the beginning I am:  #{num1}" #num1 = 13

def change(num)
  num1 = num
  puts "Inside of the change method I am: #{num1} " #num1 = 47 because num1 is local inside change
end

change 47

puts "But outside of the change method I am still: #{num1}" #num1 = 13 because change did nothing to the original num1

array  = ["crazy example"]

array.each do |e|
  num1 = e
end

puts "But look now, I've completely changed num1 to: #{num1}" #num1 = crazy example because it was changed in array.each
{% endhighlight %}

and run it here: [http://repl.it/OA9](http://repl.it/OA9).

Speaking of which, two cool resources. First syntax highlighting on your wordpress blog is made pretty simple by using the [Crayon Syntax Highlighter](http://wordpress.org/plugins/crayon-syntax-highlighter/). It's super easy to use and very customizable.

The second resource is [repl.it](http://repl.it/languages/Ruby). This lets you enter your Ruby code in and then executes it in an online version of irb (running 1.8.7, however). It's pretty good for sharing little pieces of code with your friends and family to impress them ;)

I'm looking forward the rest of the course!
