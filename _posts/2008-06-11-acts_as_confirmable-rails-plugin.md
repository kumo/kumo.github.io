---
title: acts_as_confirmable rails plugin
layout: post
category: rails
redirect_from: "/articles/2008/06/11/acts_as_confirmable-plugin/"
---
`acts_as_confirmable` is a Ruby on Rails plugin that is useful when you want to know who ticked a check box and when they did so. It can be found on [GitHub](http://github.com/kumo/acts_as_confirmable).

This plugin treats a datetime attribute and an integer attribute as a boolean. This boolean can then be used as normal attribute in a check box (or in any part of the program) and when it is checked, the datetime records the moment and the integer records the id of the current user.

> It is assumed that there is a Users table and that the current user can be found at User.current_user. If the current user cannot be accessed then 1 is used as the id.

## Installation

{% highlight sh %}
$ cd vendor/plugins
$ git checkout git://github.com/kumo/acts_as_confirmable/tree
{% endhighlight %}

## Usage

- On models where you want to be able to confirm X add a `X_confirmed_at` datetime and `X_confirmed_by` integer.
- In the model put `acts_as_confirmed :X`
- In the views add check boxes (`check_box :X`)

## Example

### create a new rails app

{% highlight sh %}
$ rails confirm
$ cd confirm
{% endhighlight %}

### create users table and items table with 3 confirmable fields

{% highlight sh %}
$ script/generate scaffold user name:string
$ script/generate scaffold item name:string \
      started_confirmed_by:integer started_confirmed_at:datetime \
      finished_confirmed_by:integer finished_confirmed_at:datetime \
      ready_confirmed_by:integer ready_confirmed_at:datetime
{% endhighlight %}

### add the plugin to the model

{% highlight ruby %}
class Item < ActiveRecord::Base
  acts_as_confirmable :started, :finished, :ready
end
{% endhighlight %}

### add 3 check boxes in items/new.html.erb and items/edit.html.erb

{% highlight erb %}
<p>
  <%= f.label :started %><br />
  <%= f.check_box :started %>
</p>
<p>
  <%= f.label :finished %><br />
  <%= f.check_box :finished %>
</p>
<p>
  <%= f.label :ready %><br />
  <%= f.check_box :ready %>
</p>
{% endhighlight %}

### show the confirmation info in items/list.html.erb

{% highlight erb %}
<td>
  <%=h item.started? %>
  <%=h item.finished_confirmer.name if @item.finished? -%>
</td>
{% endhighlight %}

### example of assigning a user to current_user

{% highlight ruby %}
class User < ActiveRecord::Base
  cattr_accessor :current_user
end

class ItemsController < ApplicationController
  before_filter :load_user

  def load_user
    User.current_user = User.find(:first)
  end
end
{% endhighlight %}

## Available fields

If a model has the attributes fields `started_confirmed_at` and `started_confirmed_by`, then the plugin provides:

- `started?`
  true if it has been confirmed by someone on a specific date
- `started`
  same as above but is normally used by a check box tag
- `started=`
  the assignment method that is used by the check box when posting
- `started_confirmer`
  the user that confirmed it
- `started_at`
  when it was confirmed
