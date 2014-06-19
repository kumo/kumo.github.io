---
title: Roman Numerals & Swift
layout: post
categories: developing, swift
---
I don't know if I will convert any of my existing iOS apps into [Swift](https://developer.apple.com/swift/), but I do like the idea of it and the playground, in particular, is a nice way of experimenting.

I have made a simple roman numerals converter (2014 -> MMXIV) in Swift which you can find below or on [GitHub](https://gist.github.com/kumo/a8e1cb1f4b7cff1548c7) . The main thing that I like is the `enumerate` and the `for j in 0..div` although it would be nice to have something like `j.times do {}`.

Originally, I wanted to use a dictionary, but since they are not ordered I used two arrays instead, but `enumerate` does keep it clear.

{% gist kumo/a8e1cb1f4b7cff1548c7 %}

**Update**: an alternative way of doing can be found in the following gist. This time I have used tuples rather than two arrays, which is a bit easier to read perhaps. I no longer need to use `enumerate` and the `for` loop no longer cares about the index `for _ in 1...count`.

More importantly, the function returns an optional string, so it is clear if the conversion worked or not. Alternatively, I could just check for an empty string or nil, but for the upcoming roman to arabic code, that wouldn't make much sense. 

{% gist kumo/43781a552ee2b845da6c %}
