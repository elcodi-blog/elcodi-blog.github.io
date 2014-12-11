---
layout: post
title: "A Week of Elcodi #5 (24/11 - 08/12)"
date: 2014-12-11 20:15:24 +0100
comments: true
categories: [A week of Elcodi]
author: Berny Cantos <berny@elcodi.com>
---
Hi there! These two weeks have been full of work, with our talk in [SymfonyCon](http://madrid2014.symfony.com) being the highlight. We're working on a post explaining the experience but, in short, it was amazing to present Elcodi to the people in the Symfony community. Stay tuned!

Meanwhile, on this side of the keyboard, we've been doing some things also, such as resume the implementation of our `StateTransitionMachine`, previously known as `StateMachine`, and polishing some details.

Besides, we wanted to implement an entity translator completely decoupled of our entities, because we think that the most important part of an e-commerce project is the model. So we worked on it for a while and now the [Translator component](https://github.com/elcodi/elcodi/pull/382) is born. You're welcome to comment and collaborate on the implementation.

To persist the settings for each shop, we are working on a [dynamic configuration component](https://github.com/elcodi/elcodi/pull/383) useful for storing simple values. As usual, it comes with adapters so that any backend storage can be supported.

From the viewpoint of desing, we want a more agile, easy and intuitive admin experience, so we're checking and improving the UX of Elcodi Admin's interface. First adjustments include some improvements about navigation, and an early approach to the first-time experience, something that we will look in depth in the next week.

Also we're putting together our [new Page component](https://github.com/elcodi/elcodi/pull/378) to be as generic as possible, but user-friendly too. This includes developers: we are making great efforts to provide a better DX (Developer eXperience), so feel free to drop by our [gitter](https://gitter.im/elcodi/elcodi) and ask your own questions. We'll be glad to help you. ;)

See you next week!
