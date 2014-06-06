---
title: Designing a quiz with plain text user stories
layout: post
categories: developing, rails
---
Over the next couple of posts I thought that I would like to discuss the redevelopment of my hiragana and katakana quiz software. This software originally started out as a command-line Perl script, became a PHP web script, before turning into a Ruby on Rails web application. Unfortunately, although the development language changed and possibilities increased, the overall functionality remained very much trapped in the original design. As I have started in other posts, I have plans for my quiz software, but it remains rather static, so perhaps it would be a good idea to talk about user stories as a way of providing clarity and focus.

I tend to prefer to use development tools that simplify the tasks that I have come to find routine, but perhaps more importantly provide me with a fresh and powerful way of looking at new problems that I need to solve. The latest tool to find me is [user stories](http://rspec.info/documentation/stories.html) with [RSpec](http://rspec.info/). Now, I must say that I was already using RSpec to ‘test’ other Ruby on Rails applications and I think that everyone should try and play around with it for a bit, but user stories—in particular the plain-text flavour of them—are what currently occupy my notebook scribbles.

Let me show a somewhat simplistic and incomplete example of a user story:

{% highlight ruby %}
Story: starting a quiz
  As a visitor
  I want to start a quiz
  So that I can hopefully learn something

  Scenario: quiz doesn't exist
    Given a quiz named hiragana
    When I start another quiz
    Then I should see the 404 error page
    And I should see a link to the list of quizes

  Scenario: quiz does exist
    Given a quiz named hiragana
    When I start the quiz
    Then I should see the quiz
    And I should see a question
{% endhighlight %}

We have a story with two scenarios: when the quiz exists and when the quiz doesn’t exist. It is not very interesting, but at least we know that the user should see an error page if they try and start a quiz that doesn’t exist, and that they should also see a list of quizes that are available instead. If the quiz does exist then they should see a question.

This might be useful if we want to ensure that the 404 page is shown when the requested quiz doesn’t exist, but it is not very satisfactory to write down and seems a bit pointless. It also doesn’t help us understand what happens when a user starts a quiz.

One of the main thing that I like about plain text user stories is that I can easily scribble them down on a piece of paper. I can focus on describing my goals without worrying about the implementation details and afterwards even use this “pseudotestcode” directly. The plain text user story also is nicely separated from the implementation code that is needed to make it work and because of this, it is easy to fit an understandable and detailed story in a manageable page or two.

I think I will use the following story in my quiz redevelopment and next time I will discuss other stories, and also how to test these stories.

{% highlight ruby %}
Story: starting a quiz
  As a visitor
  I want to start a quiz
  So that I can hopefully learn something

  Scenario: quiz doesn't exist
    Given a quiz named hiragana
    And a quiz named katakana
    When I start another quiz
    Then I should see the 404 error page
    And I should see a list of quizes
    And the list of quizes should have 2 items

  Scenario: default configuration
    Given a quiz named hiragana
    When I start the quiz
    Then I should see the quiz
    And I should see a question
    And I should see the text "Question 1 of 5"
    And there should be 3 textboxes

  Scenario: 10 questions
    Given a quiz named hiragana
    And the quiz has been configured for 10 questions
    When I start the quiz
    Then I should see the quiz
    And I should see "Question 1 of 10"

  Scenario: 5 hiragana per question
    Given a quiz named hiragana
    And the quiz has been configured for 5 hiragana
    When I start the quiz
    Then I should see the quiz
    And there should be 5 textboxes
{% endhighlight %}
