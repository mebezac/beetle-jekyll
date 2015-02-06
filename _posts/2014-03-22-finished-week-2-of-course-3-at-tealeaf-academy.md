---
layout: single-blog-post
title: "Finished Week 2 of Course 3 at Tealeaf Academy"
modified:
categories:
excerpt: This week was really great for me. I think I am really starting to get the hang of the testing process and test driven design. In the first week and for most the of the first half of this week, I had to kind of hobble along, not writing much code before taking a look at the solution videos. I had an idea of what I needed to do, but I didn't really know exactly how to do it.
tags: []
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
date: 2014-03-22T09:31:15-05:00
---

This week was really great for me. I think I am really starting to get the hang of the testing process and test driven design. In the first week and for most the of the first half of this week, I had to kind of hobble along, not writing much code before taking a look at the solution videos. I had an idea of what I needed to do, but I didn't really know exactly how to do it.

The second half of this week is when things really started to click. I was able to look at the assignment, and start implementing the features using TDD before watching the solution videos. Most of the time what I did was really close to how the solutions did it, or accomplished the same goal in a different way.

It wasn't perfect, I still picked up some new tricks this week. The thing that I learned that I liked the most was the idea of delegation on the model. This allows you to basically delegate attributes to an associated models. The example in our code is in the QueueItem model.

{% highlight ruby %}
delegate :categories, to: :video
delegate :title, to: :video, prefix: :video
{% endhighlight %}

So each queue item has an associated video. On the UI, we wanted to show the title for the video associated with a queue item. We could have called something like <code class='highlight'>queue_item.video.title</code> but we wanted it to be a little cleaner and readable. So, we wanted to be able to call something like this: <code class='highlight'>queue_item.title</code>. Before this week, I would just write a method on the model to return the title for the video. However, delegation lets you tell rails that when you call <code class='highlight'>queue_item.title</code> what you really mean is: "The title attribute belongs to video associated with a queue item. So, whenever I call <code class='highlight'>queue_item.title</code> what I really mean is <code class='highlight'>queue_item.video.title</code>" I really like this feature, and I'm definitely going to use it in my future projects.