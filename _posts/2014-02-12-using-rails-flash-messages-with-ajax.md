---
layout: single-blog-post
title: "Using Rails Flash Messages With Ajax"
modified:
categories:
excerpt: Ok, so I probably spent a little too much time working on this, but I really wanted to make it work and make it work on my own.
tags: []
image:
  feature: flash.jpg
  teaser: flash-messages.png
  thumb:
date: 2014-02-12T15:51:30-05:00
---

Ok, so I probably spent a little too much time working on this, but I really wanted to make it work and make it work on my own.

### tl;dr ###
I wanted to have ajax flash messages instead of JavaScript alerts when a vote failed validation in a reddit-like clone. [This](http://gist.github.com/mebezac/8944150) is basically how I did it.

### What I was trying to accomplish ###
For class, we are building a reddit-like clone. You can see the final solution [here](http://tl-postit.herokuapp.com/). One of the functionalities that we were supposed to add is the ability for the user to vote on a post or comment. We were also supposed 'ajaxify' the voting so that the entire page doesn't need to be reloaded just to update a single number. That part works beautifully, as you can see in the [solution](http://tl-postit.herokuapp.com/). Just register and play around and you'll see how it works. The part I didn't like was how errors were handled (i.e. trying to vote on a post more than once.).
I wanted to add the functionality for rails' flash messages to be displayed using ajax if a vote were invalid.

### How the solution does it ###
* The vote being created in memory in the vote function of the <code class='highlight'>posts_controller</code> is saved as an instance variable (<code class='highlight'>@vote</code>).
* That <code class='highlight'>@vote</code> instance variable then flows through to the <code class='highlight'>vote.js.erb</code> view in the posts view folder.
* There is logic inside the <code class='highlight'>vote.js.erb</code> view that checks if the <code class='highlight'>@vote</code> object is valid
* That logic then displays a JavaScript alert message if the <code class='highlight'>@vote</code> object is invalid

You can see all of this in action [here](http://tl-postit.herokuapp.com/).

### Why I didn't want to do it that way ###
* For my own sake, I didn't want to have that kind of validity checking logic and workflow in a view instead of a controller where the rest of that kind of logic is. I wanted to follow as closely as possible to the pattern in other database-hitting methods inside the controller. For example something like this:

{% highlight ruby %}
  def update
    if @post.update(post_params)
      flash[:notice] = "Your post was updated!"
      redirect_to post_path
    else
      render :edit
    end
  end
{% endhighlight %}

* I didn't want to use flash messages for some errors/messages and JavaScript alerts for others, I wanted to be consistent.
* I wanted <code class='highlight'>vote</code> to be a local variable to the vote method, not an instance variable that I could accidentally mess up somewhere else.
* I wanted my <code class='highlight'>vote.js.erb</code> view file to be as clean as possible (It's actually just 2 lines)

{% highlight javascript %}
  $("#post_<%= @post.id %>_votes").html("<%= @post.total_votes %>");
  <%= ajax_flash('#top-div') %>
{% endhighlight %}

### The way I did it ###
So after a lot (I mean a LOT) of trial and error, the best way I could come up with was to create a helper method called <code class='highlight'>ajax_flash</code>. Basically what this method does is inject the same alert div that is injected in the template from rendering the <code class='highlight'>_messages.html.erb</code> partial. It takes the flash message and determines the correct div to put it in. The method itself returns ajax code that does 2 things:

1. Clears out the previous ajax flash message so the user can't get a huge endless list of messages.

2. Injects the current, relevant message as the first child element of the div you specify.

If you're more interested in exactly how I got it to work, the relevant code is in a [gist here](https://gist.github.com/mebezac/8944150)

### How the method it is used ###
I call the method like this inside my <code class='highlight'>vote.js.erb</code> view:
<code class='highlight'><%= ajax_flash('#top-div') %></code>

The method takes one argument, <code class='highlight'>div_id</code> which needs to be a string. You *could* pass a class like this <code class='highlight'><%= ajax_flash('.some-class') %></code> if you really wanted, but you should be passing a <code class='highlight'>div_id</code> into it if you want it to work correctly.

The <code class='highlight'>div_id</code> is the id of the div that you want to inject the message into as the first child element. In this project, it was a div with a class of <code class='highlight'>"span12"</code> from twitter bootstrap that I latched onto. Just put some unique id in the div that contains the rendering of your message partial in your application layout file. Again, to see how I did that, take a look at [this gist](https://gist.github.com/mebezac/8944150)

### Overall ###
I am very pleased with how it works and I think it fits into the project much more seamlessly than JavaScript alert messages and helps me keep the code more organized.
