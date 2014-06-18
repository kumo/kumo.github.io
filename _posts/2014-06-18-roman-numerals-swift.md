---
title: Roman Numerals & Swift
layout: post
categories: developing, swift
---
I don't know if I will convert any of my existing iOS apps into [Swift](https://developer.apple.com/swift/), but I do like the idea of it and the playground, in particular, is a nice way of experimenting.

I have made a simple roman numerals converter (2014 -> MMXIV) in Swift which you can find below or on [GitHub](https://gist.github.com/kumo/a8e1cb1f4b7cff1548c7) . The main thing that I like is the `enumerate` and the `for j in 0..div` although it would be nice to have something like `j.times do {}`.

Originally, I wanted to use a dictionary, but since they are not ordered I used two arrays instead, but `enumerate` does keep it clear.

{% gist kumo/a8e1cb1f4b7cff1548c7 %}
