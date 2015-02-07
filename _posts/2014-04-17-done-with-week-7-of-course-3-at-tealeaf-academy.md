---
layout: single-blog-post
title: Done With Week 7 of Course 3 at Tealeaf Academy
date: 2014-04-17 15:32:35.000000000 -04:00
categories:
tags: []
excerpt: As I race towards the end of the last of Tealeaf's courses (only 1 week after this!), I find myself learning more in each lesson than a few of the previous ones combined. The amount of learning is growing at an exponential rate!
type: post
published: true
meta:
image:
  feature: tealeaf.png
  teaser: tealeaf-academy-logo.png
  thumb:
---
As I race towards the end of the last of Tealeaf's courses (only 1 week after this!), I find myself learning more in each lesson than a few of the previous ones combined. The amount of learning is growing at an exponential rate!

### Stuff We Learned

This week we covered all kinds of cool stuff. The first half of the week we covered building wrappers for connecting with APIs, fully integrating APIs into your testing, using the webmock and VCR gems to "record" the responses of API requests so only your first test actually hits the server. Learning about that led us to cover some advanced topics in testing and RSpec. Specifically, we covered test doubles, stubs, mocks, and testing JS using Selenium.

In the second half we covered going beyond the simple MVC architecture of Rails to accommodate more complex applications. This included things like extracting various parts of logic to decorators, policy objects, domain objects, and service objects. We then learned how to test this object composition using message expectation in RSpec.

### First Half of The Week

So we started out the first half of the week by moving our all of the logic concerning interacting with the external Stripe API into an API wrapper (<code class='highlight'>/app/models/stripe_wrapper.rb</code>). Moving that kind of logic into a wrapper is great because it allows you to remove unnecessary noise from your controllers, and it allows you to test the API interactions in one place.

Speaking of testing API interactions, we made use of the webmock and VCR gems to "record" the response coming back from Stripe's servers the first time we hit them with the test. This is great because it significantly cuts down the time it takes to run subsequent tests. The first time I went to use VCR however, I was given an error saying that my YAML version was out of date. This was an easy fix be re-installing ruby using rbenv. This updated YAML for me.

We are using stripe.js to handle our payments, because it allows you to capture user's credit card numbers without them ever touching your server. Because of this however, our feature tests need to be able to interact with JavaScript. We accomplished this by using the selenium-webdriver with capybara and the database_cleaner gem. Here again I ran into a bit of an issue. This time, the issue was really two issues.

First, capybara wasn't able to open the email sent by my server and click the link in it. This is solved by manually setting the capybara and test port to be the same. So in my <code class='highlight'>spec_helper.rb</code> file, I added the following at the very bottom:

{% highlight ruby %}
  Capybara.server_port = 52662
{% endhighlight %}

Then in my <code class='highlight'>config/test.rb</code> file I changed the line

{% highlight ruby %}
  config.action_mailer.default_url_options = { host: 'localhost:3000' }
{% endhighlight %}

to

{% highlight ruby %}
  config.action_mailer.default_url_options = { host: 'localhost:52662' }
{% endhighlight %}

The second issue I was having was with the database_cleaner gem. For one of my tests, it wasn't cleaning the database correctly and was causing it to fail. To fix this I just needed to change the <code class='highlight'>DatabaseCleaner.strategy</code> to <code class='highlight'>:deletion</code>.

So, after moving our API interaction logic to a wrapper, we could test that thoroughly and be confident that any calls to it would function as they should. This allowed us to remove explicit calls to the wrapper from our controller tests and instead replace them with test doubles and stubs. Basically, this means that we "mocked" or faked the response of calling a method from the wrapper and forced it have the response we want.

For example, in our users_controller_spec, we test a user signing up for our service, which includes paying with a credit card. Instead of actually going through all the way of actually hitting a method in our Stripe wrapper and getting a response from it, we can stub it. All we really care about it is what our UsersController does when there is a successful charge. So, we can simulate the response of a successful charge without actually having to create a successful charge. Once again, this gives us the ability to test the logic of our wrapper in only one place without having to worry in other specs about how to get a successful response from it. If later down the road we were to change our API wrapper, we would only need to update the spec for it. Since we stubbed the method call on our wrapper in our controller spec, we don't care how a charge is successfully placed, we just care that our controller knows what do with a successful charge.

Testing like this is especially useful for large organizations because it allows individuals or teams to work on one part of the application without really having to worry about the logic of another part. It makes code a lot more reusable, changeable, and stable.

### Second Half of The Week

In this part we covered some of the aspects of application design that go beyond just a simple MVC structure. While you can push logic from your controller down to your model to achieve a "skinny controller and fat model", eventually your model might become a huge, bloated monster. That's when it may be a good idea to start moving some of the logic into their own objects.

We covered decorators, policy objects, domain objects, and service objects.

**Decorators** are where you put the presentation logic. For example in our application we had a small conditional in our video show template. Basically it showed the average rating of the video if there were one, and displayed "N/A" if there weren't any reviews for that video. We moved that logic into a decorator.

**Policy Objects** are a great place to put logic about users' privileges and access. For example, our app only has the simple distinction of being an admin or not being an admin. So the <code class='highlight'>admin?</code> method in the User model is good enough for us. However, in a more complex application this is where you could put the logic to determine what kind of access different users of different levels have.

**Domain Objects** are where you extract the "nouns" to. This is a good place to wrap up logic you have for interacting with an attribute of a model. For example, say you have a User model that has a credit attribute. In your application, you may have to reference and modify that credit attribute a lot. It doesn't really make sense to make credits their own model extended from ActiveRecord with a database table. However, it would be nice to not have to call <code class='highlight'>current_user.credit</code> all the time. This is what domain objects are for. You can create a Credit object that handles all the logic of accessing the user and manipulating the credit attribute.

**Service Objects** if domain objects are where you extract your nouns to, service objects are where you extract your "verbs" to. In our application, we have a somewhat complicated set of actions that occur when a user signs up. We were able to extract this process into a UserSignup object. This gives us a really clean, easy to read controller. It also gives us the ability to test the sign up logic in isolation.

It's not always necessary, or even beneficial at all to move logic to any of these objects. If your logic is simple enough and not cluttering your controllers or models, it might help to keep it all in one place. However, if you're dealing with a really complicated process in your controller, or a really bloated model, it makes sense to move that logic out.

After you start splitting your logic into smaller pieces, you can write very specific, focused specs that cover the permutations of that piece of logic. It is then no longer necessary, or useful to test that logic again in another spec. However, you do need to test that your different pieces are communicating with each other correctly. This is where message expectation comes in handy. This allows you to assert that, for example, a controller delegates a certain action to a service object. Once again, this allows you to test the logic of the service object and the controller isolated from each other.

### Final Thoughts

This week was awesome because we covered a whole lot of advanced topics that I think are going to help me in my future projects and in landing a job doing something I love.

I only have one more week of class and then I am completely done! It has been a wonderful journey so far and I can't wait to see what is in store for me next.

