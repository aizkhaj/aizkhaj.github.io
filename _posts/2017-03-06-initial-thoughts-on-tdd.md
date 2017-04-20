---
id: 22
title: 'First Exposure to TDD &#8211; Initial Thoughts'
date: 2017-03-06T22:07:22+00:00
author: Aizaz
layout: post
guid: http://aizaz.tech/?p=22
permalink: /2017/03/06/initial-thoughts-on-tdd/
categories:
  - Backend
  - Ruby
tags:
  - rspec
  - ruby
  - TDD
---
For the uninitiated, TDD = test driven development.

Now that I am beginning to work on back end development, in Ruby, I have been introduced to Test Driven Development by Bloc&#8217;s curriculum.

I must say, so far I am intrigued by the concept of TDD. As someone coming from the other side of the fence being a business analyst responsible for testing new implementations (read: breaking things), I can appreciate the idea of integrating a test driven development environment.

My mentor from Bloc says it&#8217;s one of those things that developer teams all love in theory, but suck at implementing in practice.

At the moment, I&#8217;m working with rspec and following examples of how to implement tests for simple pieces of code. In a way, it really helps you think about your code in modular terms, achieving singularity. I can definitely see some drawbacks too though. What if some of your code doesn&#8217;t account for some scenarios you didn&#8217;t conceive of, but your users manage to find? What about accidental false positives?

Overall, I find it more useful to try and work with than not implement at all. After all, a lot of our job as developers is not just creating new pieces of cool software, but also fixing bugs as they come. Some foresight and integrating tests that determine how a piece of the software should function is a good approach.

Now to see how well I can continue this practice and integrate tests going forward&#8230;

_Note: The current mini project I&#8217;m working on to get Ruby + TDD skills is a simple address book app. It works through the terminal with a menu interface. I think it&#8217;s a neat approach at teaching back end. I&#8217;d love to give this a shot with node.js when the time comes 😀_