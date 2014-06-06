---
title: an assert_difference that works with arrays
layout: posts
category: rails
---
I have started using the better assert_difference that was posted at [blog.caboo.se](http://blog.caboo.se/articles/2006/06/13/a-better-assert_difference) to improve my rails tests. This allowed me to write tests such as:

{% highlight ruby %}
assert_difference Item, :count do
  Item.create(:name => “Monkey magic”)
end

# This replaces:
#   item_count = Item.count
#   Item.create(:name => "Monkey magic")
#   assert_difference, item_count + 1, Item.count
{% endhighlight %}

but I also wanted to be able to test if emails were sent by writing:

{% highlight ruby %}
assert_difference ActionMailer::Base.deliveries, :size do
   SystemNotifier.deliver_important_message
end
{% endhighlight %}

That didn’t work and it was noted on that website that the code didn’t work properly with arrays so I have modified the assert_difference method to check if the objects respond to the method (e.g. size) and act accordingly:

{% highlight ruby %}
def assert_difference(objects, method = nil, difference = 1)
  initial_values = []

  if objects.respond_to? method
    initial_values = objects.send(method)
  else
    objects = [objects].flatten
    initial_values = objects.inject([]) { |sum,obj| sum << obj.send(method) }
  end

  yield
  if difference.nil?
    objects.each_with_index { |obj,i|
      assert_not_equal initial_values[i], obj.send(method) #, "#{obj}##{method}"
    }
  else
    if objects.respond_to? method
      assert_equal initial_values + difference, objects.send(method) #, "#{objects}##{method}"
    else
      objects.each_with_index { |obj,i|
        assert_equal initial_values[i] + difference, obj.send(method) #, "#{obj}##{method}"
      }
    end
  end
end
{% endhighlight %}

I have tested it for an array such as ActionMailer::Base.deliveries but I haven’t tested it in other situations so hopefully it won’t create any problems.

The rspec version of this is:

{% highlight ruby %}
def assert_difference(objects, method = nil, difference = 1)
  initial_values = []

  if objects.respond_to? method
    initial_values = objects.send(method)
  else
    objects = [objects].flatten
    initial_values = objects.inject([]) { |sum,obj| sum << obj.send(method) }
  end

  yield
  if difference.nil?
    objects.each_with_index { |obj,i|
      initial_values[i].should_not_equal obj.send(method) #, "#{obj}##{method}"
    }
  else
    if objects.respond_to? method
      (initial_values + difference).should_eql objects.send(method) #, "#{objects}##{method}"
    else
      objects.each_with_index { |obj,i|
        (initial_values[i] + difference).should_eql obj.send(method) #, "#{obj}##{method}"
      }
    end
  end
end
{% endhighlight %}
