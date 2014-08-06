---
layout: post
title: "Our roadmap"
date: 2014-08-06 17:55:48 +0200
comments: true
categories: 
---

##### This is the second part of a [previous post](/blog/2014/08/06/a-short-break-for-meditation/)

We can proudly state we have a lot to do in the following months.

Question is, what now? Maybe we are 1% done along our path, although "done" in our world sounds a bit odd since there is always room for improvement.
As long as our issue tracker list gets feeded, we will stay hungry for enhance our code. 

### GeoBundle

One of the most recurring challenges in e-commerce development is the modeling of geographic and administrative divisions. We will put our efforts in designing a simple yet extensible set of entities and relations as well as strategies to implement custom tree structures.

### Rule management

Rule-based decision logic is ubiquitous in e-commerce applications. A few examples:

* Which discount coupon can be automatically applied upon entering the checkout process?
* Can a particular discount code be applied?
* Can two specific products be added to the same cart?
* Can a specific product combination trigger a discount, an up-sell or a different offer?
* When is it possible to use a specific carrier or payment method?

Elcodi [RuleBundle](https://github.com/elcodi/rulebundle) aims at solving these kinds of issue.

### Ports and Adapters

We surely want to promote high-cohesion, loose-coupled software components. Right now we are halfway, as described in the [previous post](/blog/2014/08/06/a-short-break-for-meditation/), but we want to keep improving. A typical usage of P&A would be to decouple Symfony bundles from other significant objects in the domain, or to be able to use Elcodi Cart component without depending on the CustomerBundle.

### Tests


