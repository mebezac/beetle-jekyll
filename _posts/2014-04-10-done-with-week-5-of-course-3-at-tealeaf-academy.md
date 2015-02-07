---
layout: single-blog-post
title: Done With Week 5 of Course 3 at Tealeaf Academy
date: 2014-04-10 11:07:11.000000000 -04:00
categories:
tags: []
status: publish
type: post
excerpt: I just finished up week 5 of the 3rd course at Tealeaf Academy. This week didn't focus on as many new features as the previous weeks. We added the ability to invite a friend to join the site (We are working on a NetFlix clone). When the inviter invites a friend, the friend receives an email with a link that has a unique token. That token lets us look up the invitation and prefill the email address portion of the registration form. If the friend who was invited accepts and registers for the site using that url with their token in it, they are set up automatically to be following the person they accepted the invitation from and the person who invited them is also automatically following them.
meta:
image:
  feature: tealeaf.png
  teaser: tealeaf-academy-logo.png
  thumb:
---
I just finished up week 5 of the 3rd course at Tealeaf Academy. This week didn't focus on as many new features as the previous weeks. We added the ability to invite a friend to join the site (We are working on a NetFlix clone). When the inviter invites a friend, the friend receives an email with a link that has a unique token. That token lets us look up the invitation and prefill the email address portion of the registration form. If the friend who was invited accepts and registers for the site using that url with their token in it, they are set up automatically to be following the person they accepted the invitation from and the person who invited them is also automatically following them.

That was the biggest feature we added this week. We also refactored a bit by moving the token generation code into a concern since it was exactly the same for both the User and Invitation models. We also setup mailgun to be our mailing service. Their API is dead simple and there were no issues setting them up, especially since there is heroku add-on for them.

We were also introduced to Redis, Resque, and Sidkiq. We ended up using Sidekiq to que and process our emails in the background.

Most of the heavy lifting of this week was focused on deployment of the application. My app is now deployed at [zacflix.herokuapp.com](http://zacflix.herokuapp.com/). We learned how to use a Procfile to tell Heroku which processes to start. We also learned about the unicorn server this week, so we told Heroku to use the unicorn server for our app with this Procfile:

{% highlight ruby %}
  web: bundle exec unicorn -p $PORT -E $RACK_ENV -c ./config/unicorn.rb
{% endhighlight %}

The <span class="lang:ruby decode:true  crayon-inline ">/config/unicorn.rb</span> file looks like this:

{% highlight ruby %}
  wyorker_processes Integer(ENV["WEB_CONCURRENCY"] || 3)
  timeout 15
  preload_app true

  before_fork do |server, worker|
    Signal.trap 'TERM' do
      puts 'Unicorn master intercepting TERM and sending myself QUIT instead'
      Process.kill 'QUIT', Process.pid
    end

    @sidekiq_pid ||= spawn("bundle exec sidekiq -c 2")

    defined?(ActiveRecord::Base) and
      ActiveRecord::Base.connection.disconnect!
  end

  after_fork do |server, worker|
    Signal.trap 'TERM' do
      puts 'Unicorn worker intercepting TERM and doing nothing. Wait for master to send QUIT'
    end

    defined?(ActiveRecord::Base) and
      ActiveRecord::Base.establish_connection
  end
{% endhighlight %}

the <code class='highlight'>@sidekiq_pid ||= spawn("bundle exec sidekiq -c 2")</code> line lets us use Sidekiq on our free Heroku server.

After that, we moved onto learning about setting up a staging server to deploy to before the production server. The staging server is supposed to be as close to the exact same setup as the production server as possible. This allows you to test your code in an environment extremely similar to the public-facing production environment, without the risk of a huge unknown bug taking down your service for your customers. Once you test that everything is working as expected on the staging server, then you can deploy to your production server.

To help with deployment, were are also using the [paratrooper gem](http://github.com/mattpolito/paratrooper). This gem takes care of a lot things for you when deploying, such as

*   Create or update a git tag (if provided)
*   Push changes to Heroku
*   Run database migrations if any have been added to db/migrate
*   Restart the application
*   Warm application instance

This gem is great too because you can set it up so that you can not deploy to production with deploying to staging first, so that you never forget.

**UPDATE: the code in README has been updated in this [merge](https://github.com/mattpolito/paratrooper/pull/58), so it should be good to use now. I'll leave the following below for reference.**

The example usage in their readme, however will lead to some errors if you just copy and paste it in. It looks like this:

{% highlight ruby %}
  require 'paratrooper'

  namespace :deploy do
    desc 'Deploy app in staging environment'
    task :staging do
      deployment = Paratrooper::Deploy.new("amazing-staging-app", tag: 'staging')

      deployment.deploy
    end

    desc 'Deploy app in production environment'
    task :production do
      deployment = Paratrooper::Deploy.new("amazing-production-app") do |deploy|
        deploy.tag              = 'production',
        deploy.match_tag        = 'staging',
        deploy.maintenance_mode = !ENV['NO_MAINTENANCE']
      end

      deployment.deploy
    end
  end
{% endhighlight %}

If you make that your <code class='highlight'>deploy.rake</code> file and run <code class='highlight'>rake deploy:production</code> you will get an error because the author of the paratrooper gem has removed the <code class='highlight'>maintenance_mode=</code> method. So, you need to remove the <code class='highlight'>deploy.maintenance_mode = !ENV['NO_MAINTENANCE']</code> line so that your <code class='highlight'>deploy.rake</code> looks like this:

{% highlight ruby %}
  require 'paratrooper'

  namespace :deploy do
    desc 'Deploy app in staging environment'
    task :staging do
      deployment = Paratrooper::Deploy.new("zacflixstaging", tag: 'staging')

      deployment.deploy
    end

    desc 'Deploy app in production environment'
    task :production do
      deployment = Paratrooper::Deploy.new("zacflix") do |deploy|
        deploy.tag              = 'production',
        deploy.match_tag        = 'staging'
      end

      deployment.deploy
    end
  end
{% endhighlight %}

If you try to run <code class='highlight'>rake deploy:production</code> again you will get an error close to this:

{% highlight ruby %}
  ============================================================
  >> Updating Repo Tag: ["production", "staging"]
  ============================================================

  fatal: too many params
  fatal: remote part of refspec is not a valid name in [production,

  ============================================================
  >> Pushing refs/tags/["production", "staging"] to Heroku
  ============================================================

  fatal: remote part of refspec is not a valid name in refs/tags/[production,

  ============================================================
  >> Accessing zacflix.herokuapp.com to warm up your application
  ============================================================
{% endhighlight %}

The reason is you need to remove the comma after <code class='highlight'>deploy.tag = 'production',</code> . So, your <code class='highlight'>deploy.rake</code> file needs to look like this:

{% highlight ruby %}
  require 'paratrooper'

  namespace :deploy do
    desc 'Deploy app in staging environment'
    task :staging do
      deployment = Paratrooper::Deploy.new("zacflixstaging", tag: 'staging')

      deployment.deploy
    end

    desc 'Deploy app in production environment'
    task :production do
      deployment = Paratrooper::Deploy.new("zacflix") do |deploy|
        deploy.tag              = 'production'
        deploy.match_tag        = 'staging'
      end

      deployment.deploy
    end
  end
{% endhighlight %}

This time you can successfully call <code class='highlight'>rake deploy:production</code>

Another small issue I ran into this week had to do with adding new gems and deploying to Heroku. There are a couple gems for this project that aren't used on my development machine, but only once the code is live on Heroku. Even still, make sure you run <code class='highlight'>bundle</code> on your local machine before deploying to Heroku, because your <code class='highlight'>Gemfile.lock</code> needs to be up to date.

One last thing, I found a pretty cool site for honing your ruby (and other language) skills. It's called [codewars](http://www.codewars.com/). Basically it is a whole bunch of problems that you need to solve by coding. The first one I tried was writing a method that converted any number from 1 to 1000000000 to a string with commas in the right place.

The great thing about this site is that it has a simple to use testing framework so you can use TDD to work on the problems. Working on your ruby skills could also help you get a job since you are almost definitely going to be asked to code during an interview. After you complete one of the challenges, you can see how other people have answered it so you pick up new tricks, or different ideas about how to solve problems.

That's all for this week, only 3 weeks left!
