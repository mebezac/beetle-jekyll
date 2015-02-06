---
layout: single-blog-post
title: How to seed file uploads from a remote URL for CarrierWave.
date: 2014-04-23 09:13:38.000000000 -04:00
categories:
tags: []
excerpt: I recently deployed an app to Heroku. It's a Netflix clone called MyFlix. The cover art for the different videos are image files stored in an S3 bucket. The files are uploaded when an admin creates a video through the app's interface. The wonderful CarrierWave gem, in combination with the Fog gem take care of uploading the image files to S3 and associating the appropriate URLs with the Video objects.
type: post
published: true
meta:
  dsq_thread_id: '2632974204'
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
---
I recently deployed an app to Heroku. It's a Netflix clone called [MyFlix](http://www.zaccodes.com/project/myflix/). The cover art for the different videos are image files stored in an S3 bucket. The files are uploaded when an admin creates a video through the app's interface. The wonderful CarrierWave gem, in combination with the Fog gem take care of uploading the image files to S3 and associating the appropriate URLs with the Video objects.

When I deployed the application however, I didn't want to have to go in and create a bunch of videos manually. I wanted to create the videos in the same way I created the rest of the starting data for the application, using my seeds.rb to set up some seed data.

In my example, using videos, there is a  <code class='highlight'>small_cover</code> attribute and a  <code class='highlight'>large_cover</code> attribute. I had already uploaded the cover images to S3 during development, so I had all their URLs.

Adding those files to my seed video objects was actually pretty easy. All I had to do was specify  <code class='highlight'>remote_small_cover_url</code> and <code class='highlight'>remote_large_cover_url</code>. CarrierWave basically just grabs the URL, opens it, and uploads it in whichever way you set. So, even though my files were already uploaded to S3, I really could of put in any image url and it still would have worked.

So, in summary, say you have <code class='highlight'>some_attribute</code> on your model that is a file, in order to seed data from a URL all you have to do is specify <code class='highlight'>remote_some_attribute_url</code> in your seed data. Here's what a whole line might look like:

{% highlight ruby %}
  v9 = Video.create(title: "Monk", description: "Adrian Monk is a brilliant San Francisco detective, whose obsessive compulsive disorder just happens to get in the way.", remote_small_cover_url: "http://s3.amazonaws.com/zacflix/uploads/monk.jpg", remote_large_cover_url: "http://s3.amazonaws.com/zacflix/uploads/monk_large.jpg", categories: [dramas])
{% endhighlight %}
