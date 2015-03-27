---
title: Subtitles and timed texts
layout: post
categories: random
---
I am currently working on a Swift parser for various types of subtitles and timed texts. As always the Xcode Playground makes it very useful to try things out, although I would like a way of adding some sort of unit tests.

At the moment my aim is on trying to see what sort of resources are available online; my interest is in working with foreign languages to help teach students or to improve my own knowledge.

The following is some code that as a way of trying to identify what type of timed text the user has given us. It uses a `NSScanner` to try and look for features and although it doesn't work with everything that I found[^lrc], it seems to work well most of the time.

Obviously this code is for when the user pastes text that they have found online.

{% gist kumo/adbede0c0c092683b5c2 %}

[^lrc]: I found some .lrc files that have extra/invalid text at the start
