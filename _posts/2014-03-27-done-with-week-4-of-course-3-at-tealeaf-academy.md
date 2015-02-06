---
layout: single-blog-post
title: Done With Week 4 of Course 3 at Tealeaf Academy
date: 2014-03-27 16:11:44.000000000 -04:00
categories:
tags: []
excerpt: We covered a whole lot this week, I learned quite a few new things. A few things stood out however. Testing non-standard models with Shoulda matchers in RSpec.
type: post
published: true
meta:
  dsq_thread_id: '2526847121'
image:
  feature:
  teaser: tealeaf-academy-logo.png
  thumb:
---
We covered a whole lot this week, I learned quite a few new things. A few things stood out however.

### Testing non-standard models with Shoulda matchers in RSpec

So this week we added some social networking features. We added the ability for users to follow other users. So one user is a follower and the other user is the leader in the way we set it up. To accomplish this we set up a <code class='highlight'>Relationship</code> model to keep track of these leader/follower relationships. This is what that model looks like:

{% highlight ruby %}
    class Relationship < ActiveRecord::Base
      belongs_to :follower, class_name: "User"
      belongs_to :leader, class_name: "User"

      validates_uniqueness_of :follower_id, scope: [:leader_id]
    end
{% endhighlight %}

The user model then needed these lines to get the connection set up:

{% highlight ruby %}
  has_many :following_relationships, class_name: "Relationship", foreign_key: :follower_id
  has_many :leading_relationships, class_name: "Relationship", foreign_key: :leader_id
{% endhighlight %}

I had to dig around a bit in the excellent Shoulda [README](https://github.com/thoughtbot/shoulda-matchers#readme) to figure out how to test these models that don't really fit the default Rails behavior.

Here is my spec for the Relationships model

{% highlight ruby %}
  require 'spec_helper'
    describe Relationship do
      it { should belong_to(:follower).class_name('User') }
      it { should belong_to(:leader).class_name('User') }
      it { should validate_uniqueness_of(:follower_id).scoped_to(:leader_id) }
    end
{% endhighlight %}

And the relevant parts of my User Model spec:

{% highlight ruby %}
  it { should have_many(:following_relationships).class_name("Relationship").with_foreign_key('follower_id') }
  it { should have_many(:leading_relationships).class_name("Relationship").with_foreign_key('leader_id') }
{% endhighlight %}

So, this week really got me to dive deeper into Rails models. How they work, and how to properly test them.

### Set host in test environment config for mail tests

This week we also got introduced to mailers and how to test them. Most of it was straight-forward. However, a source of frustration for me involved urls in the email view templates.

In order to point a link in an email to your site, you have to specify the host. The way we did this in our development environment was to add this line to the config/environments/development.rb file:

{% highlight ruby %}
  config.action_mailer.default_url_options = { host: 'localhost:3000' }
{% endhighlight %}

That works great for playing around with the UI, but when I was trying to run tests I kept getting errors telling me to specify a host. The problem is that you need to add that line to the config/environments/test.rb file as well. This is pretty obvious looking back, but it took me a little while to figure that out, so if anyone else runs into that issue, that's how you fix it :).

### Resetting the password

This week we also learned how to implement a pretty advanced feature, allowing the user to reset their password. It was really awesome to learn how to implement something like this. Everyday I am getting more and more confident in my ability to build a full-fledged production-quality app.