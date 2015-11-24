---
title: Continuous Integration for Phoenix Apps on CircleCI
date: 2015-11-24T08:25:42-05:00
layout: single-blog-post
excerpt: I just recently got turned on to Elixir and Phoenix. I decided to play around and write a small, stupid app that would function as a sort of bot for my company's Slack team. There are a handful of specs, and I wanted to be able use Continuous Integration to run those specs for me.
published: true
image:
  feature: post-images/conveyor_belt.jpg
  teaser: post-images/conveyor_belt_square.jpg
---

I just recently got turned on to [Elixir](http://elixir-lang.org/) and [Phoenix](http://www.phoenixframework.org/). I decided to play around and write a small, silly app that would function as a sort of bot for my company's Slack team. There are a handful of specs, and I wanted to be able use Continuous Integration to run those specs for me.

I tried using Travis CI first with their [instructions](https://docs.travis-ci.com/user/languages/elixir/) for running Elixir projects. I left it with the default settings which uses Elixir 1.0.2 and Erlang/OTP 17.4. It failed because my project needed Elixir >= 1.1.0, namely because it uses `Enum.random/1`. [My Travis build](https://travis-ci.org/mebezac/lunchify/builds/92854536)

Then I tried to install Erlang/OTP 18 and Elixir 1.1.1 on Travis CI, but it just timed out. [Timed out build](https://travis-ci.org/mebezac/lunchify/builds/92856484).

So, I gave up on Travis CI and decided to try CircleCI instead. I based my `circle.yml` file on instructions from [Kevin Disneur](https://archives.kevin.disneur.me/2015-06-14-elixir-on-circleci.html) and [goneflyin](https://discuss.circleci.com/t/building-elixir-projects/21). Neither of their instructions worked 100% for me, but in combining the two, I was able to get my specs to run on CircleCI!

This configuration uses [asdf](https://github.com/HashNuke/asdf) a multi-language version manager.<br>
To set the Erlang/OTP and Elixir versions, just add a `.tool-versions` file at the root of your project. My file looks like this:

{% highlight YAML %}
erlang 18.0
elixir 1.1.1
{% endhighlight %}

Once you have that file, all you need is a `circle.yml` file to tell CircleCI what to do.

## My `circle.yml` file

{% highlight YAML %}
  machine:
    environment:
      PATH: "$HOME/.asdf/bin:$HOME/.asdf/shims:$PATH"

  dependencies:
    cache_directories:
      - ~/.asdf
    pre:
      - if ! asdf | grep version; then git clone https://github.com/HashNuke/asdf.git ~/.asdf; fi
      - asdf plugin-add erlang https://github.com/HashNuke/asdf-erlang.git
      - asdf plugin-add elixir https://github.com/HashNuke/asdf-elixir.git
      - erlang_version=$(awk '/erlang/ { print $2 }' .tool-versions) && asdf install erlang ${erlang_version}
      - elixir_version=$(awk '/elixir/ { print $2 }' .tool-versions) && asdf install elixir ${elixir_version}
      - yes | mix deps.get
      - mix local.rebar
  test:
    override:
      - mix test
{% endhighlight %}

The first run will take significantly longer than subsequent runs as the Erlang/OTP and Elixir installations are not yet cached. My first run took around 9 minutes, whereas my second run took a little over 2 minutes.
