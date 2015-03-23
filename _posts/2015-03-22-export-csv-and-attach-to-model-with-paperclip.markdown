---
title: "Export CSV and Attach to Model With Paperclip"
date: 2015-03-22T21:53:44-04:00
layout: single-blog-post
excerpt: From time to time, users will want export their data from your app. An easy way to do that is to build a CSV (comma separated values) file and then let them download it. I'll show you how build that CSV and then attach to a model with Paperclip
published: true
image:
  feature: post-images/csv.jpg
  teaser: post-images/csv_square.jpg
---

From time to time, users will want export their data from your app. An easy way to do that is to build a CSV (comma separated values) file and then let them download it. I'll show you how build that CSV and then attach to a model with [Paperclip](https://github.com/thoughtbot/paperclip).

First things first, add an attachment to your model:

{% highlight ruby %}
rails generate migration add_csv_to_foo
{% endhighlight %}

Then in your migration:

{% highlight ruby %}
def change
  add_attachment :foos, :csv
end
{% endhighlight %}

Then in your model:

{% highlight ruby %}
class Foo < ActiveRecord::Base
  has_attached_file :csv
  validates_attachment :csv, content_type: { content_type: "text/csv" }
end
{% endhighlight %}

Next, for you model, create a method for exporting whatever it is you want to export to a CSV. There's a great [RailsCast about it](http://railscasts.com/episodes/362-exporting-csv-and-excel) you can check out to get it set up. In my app, I'm allowing users to export their
contacts to a CSV. Some users have upwards of 3,000 contacts, so exporting contacts can take quite a long time. Therefore, I put my export method in background job.

{% highlight ruby %}
def perform(foo, options = {})
  CSV.generate(options) do |csv|
    csv << make_column_names(foo)
    build_row_values(foo).each do |row|
      csv << row
    end

    file = StringIO.new(csv.string)
    foo.csv = file
    foo.csv.instance_write(:content_type, 'text/csv')
    foo.csv.instance_write(:file_name, make_filename(foo))
    foo.save!

  end
end
{% endhighlight %}

The first part of that method generates the CSV, which is really just a big string. The second part of the method is:

{% highlight ruby %}
file = StringIO.new(csv.string)
foo.csv = file
foo.csv.instance_write(:content_type, 'text/csv')
foo.csv.instance_write(:file_name, make_filename(foo))
foo.save!
{% endhighlight %}

That part takes the big string part of the CSV, turns into a [pseudo I/O](http://ruby-doc.org/stdlib-2.2.0.preview1/libdoc/stringio/rdoc/StringIO.html) that Paperclip can attach to the model and upload to S3.

Now, you can call `foo.csv.url` to get a link to the CSV file stored on S3. If you just link to that however, most browsers will simply render the CSV as text. What you want is to force the file to download. To do that, create a controller action that will send the file as data, instead of just linking to the url. It looks like this:

{% highlight ruby %}
class FoosController < ApplicationController
  def download_csv
    foo = Foo.find(params[:id])
    data = open(foo.csv.url)
    send_data data.read, filename: "#{foo.csv_file_name}", type: "text/csv", disposition: 'attachment'
  end
end
{% endhighlight %}

That will force the file to be downloaded instead of just being rendered in the browser.

#### Bonus: Progress bar

In my app, users are exporting sometimes 3,000 or more contacts. I use DelayedJob for the background work in the app. There's a great gem [progress_job](https://github.com/d4be4st/progress_job) that gives you an easy way to display the progress of background jobs.

[![]({{ site.url }}/img/post-images/export.gif)]({{ site.url }}/img/post-images/export.gif)

The demo code in the progress_job repo has a JavaScript function with a 100ms timeout, which hits your server and database a lot. I just upped that 800ms and it still looks great.
