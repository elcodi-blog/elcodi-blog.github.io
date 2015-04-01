---
layout: post
title: "We have evolved"
date: 2014-08-28 11:42:47 +0200
comments: true
published: true
categories:
    - elcodi
    - ecommerce
    - symfony
---
You might say that we've evolved.

An evolution ought to be seen as a positive thing, and ours is no exception.
At first, we had serious doubts about the nature of our project. One of the
questions we kept asking ourselves was what factors should we consider in
choosing whether and when to engage external projects or libraries, and we
believe we made the right decision. In fact, that decision brought us forward in
a big way toward defining ourselves.

Flexible e-commerce components for Symfony2

Anyway, this phrase is actually very ambiguous, as we could be referring to
Symfony as a set of PHP components or as the Framework (I've always wondered
why they have the same name... it causes a lot of confusion).

That's why we wanted to reflect a bit, starting from the code itself. Do we want
to use the Symfony Framework or the Symfony Components? The answer seemed to
emerge spontaneously. We want to work with the Symfony components, but we also
want to propose bundles that use the framework.

This was the origin of this commit.

https://github.com/elcodi/elcodi/pull/239

We have only just planted the seed with this refactoring. We have realized, even
more, the importance of having both functional testing and unit testing (unit
tests are almost all for the components, while functional tests are for the
bundles). This needs more work, but at least we have lifted this project to a
level at which we truly feel comfortable. It's an excellent starting point for
a long and exciting path ahead.

This change has resulted in some collateral damage, as it has changed the path
of our bundles, meaning that the "subtree split" has resulted in totally new
repositories beginning with the first commit after refactoring. This means that
all tags prior to version 0.3.0 have been lost, so we apologize to those who
have started to tinkle with our new bundles. You'll now need to adapt to version
~0.3 and check that everything's working.

We are available for any questions or concerns.

Happy coding!