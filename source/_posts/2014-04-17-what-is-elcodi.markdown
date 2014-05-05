---
layout: post
title: "What is Elcodi?"
date: 2014-04-17 17:23:39 +0200
comments: true
categories: 
---

How many e-commerce solutions are there out in the wild?

Let me rephrase that: how many usable _developer friendly_ e-commerce solutions are there?

Once again. how many development platform are there that won't end with you saying "Ok, I'll do it myself"

We do not a have definitive answer to this challenging problem. As many of you, we have been asking those questions to ourselves when we needed to start new projects from scratch. Yet those doubts kept on bouncing in our ``HEAD``:

* What is the optimal balance between _flexibility and development speed_?
* What are the basic components that should be there _without reinventing the wheel_?
* How can we design those components _in a generalized way_?

Symfony developers already got very powerful tools that can be exploited to resolve these doubts. We would like to share with you our path along the search for this holy grail by publishing the internals of the e-commerce applications we've been developing over the last months.

Meet [Elcodi](http://elcodi.io), a suite of Symfony2 e-commerce bundles that focus on:

* Designing loosely coupled components
* Liskov is your friend: using abstract as when defining contracts among components
* Exploiting Symfony2 DependencyInjection component to expose easy customization of behaviours and models
* Using Factory injections in services to that objects are always created in a consistent state
* EventDispatcher FTW: events, events, events. First rule to avoid code entanglement
* Rigorous taxonomy: giving name to things may be an art, but the reward is massive

Are you willing to join us in this journey?

Let's talk about it on https://github.com/elcodi/elcodi

Cheers!
