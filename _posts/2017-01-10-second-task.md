---
layout: post
title: "Refactoring views(HAML) in Ruby on Rails"
date: 2017-01-05
categories: Ruby
featured_image: ""
author: Thibault
permalink: refactoring-views-in-ruby-on-rails
comments: true
---

Refactoring makes your views easier to maintain. The problem is choosing where the refactored code goes.

Your two choices are partials and helpers. There's no stone-set rules dictating which should be used where. There are a couple of guidelines floating around like the one stating that helpers should not contain HTML.

Generally partials are better suited for refactoring sections that are more HTML/ERB/HAML than ruby. Helpers on the other hand are used for chunks of ruby code with minimal HTML or generating simple HTML from parameters.

However, I don't agree with the sentiment that helpers should contain no HTML at all. A little is ok, just don't over do it. The way helpers are processed hinder their use for producing large amounts of HTML. Which is why it's suggested that your helpers contain minimal amounts of HTML. If you look at the source the helpers that ship with rails you will notice that most of them generate html. The few that don't, are mainly used to generate parameters and evaluate common conditions.

For example, any of the form helpers or link_to variants fit the first form of helpers. While things like url_for and logged_in? as supplied by various authentication models are of the second kind.

This is the decision chain I use to determine whether to factor code from a view into a partial or helper.

Repeating or nearly identical statements producing a single shallow html tag? => helper.
Common expression used as an argument for another helper? => helper.
Long expression (more than 4 terms) used as an argument for another helper? => helper.
4 or more lines of ruby (that is not evaluated into HTML)? => helper.
Pretty much everything else => partial.
I'm going to use the code you're looking to refactor as an example:

I would refactor the view in the question this way:

app/helpers/beast_helper.rb:
{% highlight ruby %}
def beast_action(beast)
  if beast.dead?
    link_to "bury", bury_beast_path(beast)
  else
    link_to "kill!", kill_beast_path(beast)
  end
end
{% endhighlight %}

app/views/beasts/_beast.html.erb:
{% highlight ruby %}
<%= beast.body %>
<%= beast_action(beast) %>
{% endhighlight %}

app/views/beasts/index.html.erb:
{% highlight ruby %}
<%= render :partial => "beast", :collection => @beasts %>
{% endhighlight %}

It's technically more complicated, because it's 3 files, and 10 lines total as opposed to 1 file and 10 lines. The views are now only 3 lines combined spread over 2 files. The end result is your code is much more DRY. Allowing you to reuse parts or all of it in other controllers/actions/views with minimal added complexity.

As for your body tag id. You should really be using content_for/yield. For that kind of thing.

app/views/layouts/application.html.erb

...
<body id="<%= yield(:body_id) %>">
...
app/views/beasts/index.html.erb

<% content_for :body_id, controller_action %>
...
This will allow you to override the id of the body in any view that requires it. Eg:

app/views/users/preferences.html.erb

<% content_for :body_id, "my_preferences" %>
