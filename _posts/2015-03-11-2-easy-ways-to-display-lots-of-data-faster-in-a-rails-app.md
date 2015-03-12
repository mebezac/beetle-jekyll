---
layout: single-blog-post
title: 2 Easy Ways to Display Lots of Data Faster in a Rails App
date: 2015-03-11 18:53:58.000000000 -05:00
excerpt: Rendering views is <em>almost</em> always the slowest part of a rails app. The more data you need to display, the more rendering needs to take place, the slower the app loads. This post is going to show you how to use a couple tools to display lots of data in a rails app in a faster way.
published: true
image:
  feature: post-images/faster.jpg
  teaser: post-images/faster_square.jpg
---
Rendering views is *almost* always the slowest part of a rails app. The more data you need to display, the more rendering needs to take place, the slower the app loads. This post is going to show you how to use a couple tools to display lots of data in a rails app in a faster way.

In our app, [bookacoach](https://www.bookacoach.com/) (*A site that helps sports academies manage all their training online*), I need to display a lot of data in 2 forms. 1. Data in a table form. 2. Data in a calendar form.

### Speeding up pages with lots of data

A great way to speed up the loading of pages with lots of data is to let everything do what it's good at. Databases are really good at storing, searching, and sorting. So lets' have the database handle that work (through our ORM, ActiveRecord). Rails is really good getting data from the database to the views through a controller, so let it do that. jQuery in conjunction with AJAX is really good at loading data from a source and then changing the data the user sees, so we'll use it for that.

For both our needs, displaying lots of data in a table form, and displaying lots of data in a calendar form, there happen to be some awesome pre-built tools I can implement.

## 1. Displaying data in table form

Quite aptly named, [DataTables](http://www.datatables.net/), is a jQuery plugin that is great for displaying, searching and sorting data in a table form. DataTables can load a JSON data source using AJAX, so it's perfect for what I want to do. There's even a great little wrapper for DataTables' AJAX functionality in rails again aptly named [ajax-datatables-rails](https://github.com/antillas21/ajax-datatables-rails), and that's what I used in our app.

The ajax-datatables-rails gem is pretty straightforward to use. You can read the docs to see how you would set it up for a DataTable displaying data from a single model. It's a great little gem because it uses the database to do all the searching and sorting, something the database is very good (and fast) at.

In our app, I needed to display a lot of different models (different kinds of trainings) all on the same DataTable and still be able to sort, search, and paginate the data. What I ended up doing was creating a <code class='highlight'>TrainingSearchEntry</code> model. Each <code class='highlight'>TrainingSearchEntry</code> object shares the same attributes, even though the attributes/methods in each of the different training models may be different. Each training  <code class='highlight'>has_one: training_search_entry</code> that is created or updated each time a training is created or updated. This allowed me to have a standard interface for my DataTable to search and sort, even if the trainings backing the data are all vastly different. The end result is something like this:

[![]({{ site.url }}/img/post-images/data_table_50_percent.png)]({{ site.url }}/img/post-images/data_table.png)

### A couple tricks learned along the way

#### Correctly sorting single-day events with multi-day events

In our app there basically two different categories trainings can fall into, time-wise. Either a single-day event (e.g. May 3rd 2PM - 3:30 PM) or multi-day event (e.g. May 3rd 2PM - 3:30 PM, May 10th 2PM - 3:30 PM, and May 15th 1PM - 2PM) You'll notice one of the columns in my DataTable is "Date & Time". So how do you sort in the database a bunch of different trainings with their own models based on their date and time when some of them have only one date and time and some have multiple? This is one of the reasons I decided have a dedicated <code class='highlight'>TrainingSearchEntry</code> model. When building the <code class='highlight'>date_and_time</code> value for any given <code class='highlight'>TrainingSearchEntry</code> object, I have code that creates the time in this format <code class='highlight'>sortable-datestring|Human Readable Datestring</code>. So for example, a lesson that happens on Sat, 04/11/15 12:30 PM - 1:00 PM would have a <code class='highlight'>date_and_time</code> of this <code class='highlight'>"201504111230|Sat, 04/11/15 12:30 PM - 1:00 PM EST"</code>. A multi-day training might have one that looks like this <code class='highlight'>"201504021045|Thu, 04/02/15 - Thu, 04/09/15"</code>. As you can see, the earliest date is put in a sortable format before the `"|"` character.

Then in my <code class='highlight'>app/datatable/training_datatable.rb</code> I have some handy little code that splits that string up so the sure doesn't see the weird string used for sorting in the database, but instead only sees the human readable part.

{% highlight ruby %}
def build_hidden_search_and_sort_string(string)
  if string
    array = string.split('|')
    "<span class='date-sort-span'>#{array[0]}</span>#{array[1]}"
  else
    ''
  end
end
{% endhighlight %}

The class <code class='highlight'>date-sort-span</code> is then hidden with CSS. This allows DataTables to sort by date correctly.

#### Filtering based on training type.
[![]({{ site.url }}/img/post-images/filters.png)]({{ site.url }}/img/post-images/filters.png)

Creating filters also takes advantage of hiding the text on the left side of a `"|"` character. Each <code class='highlight'>TrainingSearchEntry</code> has a title attribute that is something similar to this <code class='highlight'>is: some categorical description|Title of training</code>. For example a camp's title is setup like this <code class='highlight'>"is: camp not day|Camp #{training.title}"</code>. I use the same <code class='highlight'>build_hidden_search_and_sort_string</code> method from above to hide the text to the left of the `"|"` character and show the text on the right to the user.

To make the filters work with DataTables is a simple as searching for the required text (e.g. "is: camp not day") when the filter is clicked. That's done with this JavaScript:

{% highlight javascript %}
// Get the instance of the DataTable
var table = $("#scheduled-training-table").DataTable();

// On click of the filter, search for the phrase, then draw the results on the table
$( "#camp-filter" ).click(function() {
  table.search('is: camp not day').draw();
});
{% endhighlight %}


#### Loading indicator
[![]({{ site.url }}/img/post-images/datatable.gif)]({{ site.url }}/img/post-images/datatable.gif)

By default, DataTable shows some small text when loading that is pretty easy to miss. I wanted to give the users a harder to miss indicator that something is going on while their data is loading. It also makes the whole process seem even faster because there's no "Hey I clicked that column to sort, but nothing is happening." It gives the user a visual cue that something _is_ happening and tells them when it's done.

Luckily, DataTable also gives you a really easy class to hook into when it's loading. It unhides a div with the class <code class='highlight'>.dataTables_processing</code> anytime it's loading. From that, it was easy to just add on the awesome CSS-only spinner I got [here](http://codepen.io/MattIn4D/pen/LiKFC)

<hr>

## 2. Displaying data in calendar form.

[FullCalendar](http://fullcalendar.io/) is a great jQuery plugin for displaying events on a calendar. It's very versatile and easy to use. You can get it wrapped up for rails in [this gem](https://github.com/bokmann/fullcalendar-rails). I also got some inspiration and direction from reading the source of the [fullcalendar-rails-engine](https://github.com/vinsol/fullcalendar-rails-engine). I didn't need all that it had to offer since I already had my own way of managing events and times. It did however get me started on using the AJAX functionality of FullCalendar.

FullCalendar handles JSON feeds easily [http://fullcalendar.io/docs/event_data/events_json_feed/](http://fullcalendar.io/docs/event_data/events_json_feed/). So all I needed to do was give it some events in JSON to put on the calendar.

It's as easy as giving the url that hits this controller method as the event source to FullCalendar:

{% highlight ruby %}
def get_time_slots
  start_at = parse_time_param(params[:start])
  end_at   = parse_time_param(params[:end])

  collect_time_slots(start_at, end_at)

  formatted_time_slots = build_formatted_time_slots
  render json: formatted_time_slots.to_json

end
{% endhighlight %}

To get the time_slots (the model used in our app to represent anything dealing with time) in a certain range, I pushed the work down to the database using this scope:

{% highlight ruby %}
scope :in_time_range, -> (start_at, end_at) {where('
      (start_time >= :start_at and end_time <= :end_at) or
      (start_time >= :start_at and end_time > :end_at and start_time <= :end_at) or
      (start_time <= :start_at and end_time >= :start_at and end_time <= :end_at) or
      (start_time <= :start_at and end_time > :end_at)',
      start_at: start_at, end_at: end_at)}
{% endhighlight %}

Then once I had the time_slots collected, I formatted them so that FullCalendar could understand them using a method similar to this:

{% highlight ruby %}
def build_formatted_availability_time_slot(raw_time_slot)
  {
    id: raw_time_slot.id,
    start: raw_time_slot.start_time.in_time_zone(get_time_zone).iso8601,
    end: raw_time_slot.end_time.in_time_zone(get_time_zone).iso8601,
    url: edit_time_slot_path(raw_time_slot.id),
    title: 'Lesson Availability',
    textColor: 'white',
    color: '#31b0d5'
  }
end
{% endhighlight %}

Finally I just render that result as JSON and FullCalendar handles the rest of the work of actually showing the events on a calendar. This process again allows the user to see all of the events on their calendar as far back or as far forward as they want without ever loading more than a month at any one time. This keeps the pages very fast, as opposed to loading all the events at once.

#### Loading indicator

FullCalendar doesn't having a loading indicator built in, but it's really easy to add one. All you have to do is add a div right above your calendar like this:

{% highlight html %}
<div id="calendar-loading" style="display: none;"></div>
{% endhighlight %}

and then add this to your FullCalendar JavaScript:

{% highlight javascript %}
loading: function(bool) {
  if (bool)
    $('#calendar-loading').show();
  else
    $('#calendar-loading').hide();
}
{% endhighlight %}

Now you have a div that shows when the FullCalendar is loading, and hides when it is not. I just use that same [CSS-Spinner](http://codepen.io/MattIn4D/pen/LiKFC) as my loading indicator.

[![]({{ site.url }}/img/post-images/fullcalendar.gif)]({{ site.url }}/img/post-images/fullcalendar.gif)

### Conclusion

I needed to display thousands of rows of data in a table and thousands of events on a calendar. I tried using [caching](http://edgeguides.rubyonrails.org/caching_with_rails.html) but the first (uncached) load of the page was still too expensive with that amount of data to display. Pushing all of the sorting and searching to the database and then using AJAX and jQuery to render the data helped speed up our pages incredibly.

So if you need to display data in table form, check out [DataTables](http://www.datatables.net/) and the [ajax-datatables-rails gem](https://github.com/antillas21/ajax-datatables-rails). If you need to display data in calendar form, check out [FullCalendar](http://fullcalendar.io/) and its AJAX functionality.
