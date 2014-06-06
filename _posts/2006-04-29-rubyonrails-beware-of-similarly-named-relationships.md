---
title: rubyonrails - beware of similarly named relationships
layout: post
categories: developing, rails
---
I recently developed a <a href="http://www.rubyonrails.org">rubyonrails</a> application in which a user had a province (as part of their address) and also a collection of selected provinces.

{% highlight ruby %}
class Users
  belongs_to :province
  has_and_belongs_to_many :selected_provinces, :class_name => "Province
end
{% endhighlight %}

I could very easily find out which provinces a user had selected and also which users had selected a specific province. However I didn't realise that if I accessed a user via the province then the `province_id` which previously pointed the user's address province was replaced by the id of the selected province.


{% highlight ruby %}
province = Province.find_first
province.users.each do |user|
  # do something
  # however user.province_id now points to province
end
{% endhighlight %}

I assume the `province_id` is added by the habtm join so that we can understand which `selected_province` object this user object comes from however in my case it meant that I lost the original `province_id` and so if I saved that user then I would end up changing the address province.

This is probably not a bug as such but something that should be watched out for as it took me a while to discover this problem with my application.
