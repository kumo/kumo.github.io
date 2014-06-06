---
title: The perils in trusting syntax highlighting
layout: post
categories: developing
---
Today after upgrading RSpec I ran a user story that I had just written and received a wonderful error: `You have a nil object when you didn't expect it! (NoMethodError). The error occurred while evaluating nil.add_scenario`. I didn't even think to check my user story because the syntax highlighting looked fine and it made sense to me:

{% highlight ruby %}
  Scenario: areas have different types
    As an administrator
    I want to assign different types to the areas
    So that I can structure the home page

    Scenario: footer shows page links
      Given an area
      And area has 5 pages
      And area is of type footer
      When I view the home page
      Then I should see the home page
      And I should see the footer section
      And the list section should contain 0 areas
      And the footer section should contain 5 links
{% endhighlight %}

Of course, if I actually bothered to read it properly then I would have seen that the first line should have read:

{% highlight ruby %}
  Story: areas have different types
{% endhighlight %}

Moral of the story: even if the code that you have to write is small, always assume that you are capable of making silly mistakes!
