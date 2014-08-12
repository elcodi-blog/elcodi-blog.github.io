---
layout: post
title: "Our roadmap"
date: 2014-08-12 09:55:48 +0200
comments: true
categories:
    - elcodi
    - ecommerce
    - symfony
---

##### This is the second part of a [previous post](/blog/2014/08/11/a-short-break-for-meditation/)

We can proudly state we have a lot to do in the following months.

Question is, what now? Maybe we are 1% done along our path, although "done" in our world sounds a bit odd since there is always room for improvement.
As long as our issue tracker list gets fed, we will stay hungry for enhance our code. 

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

A great deal of credibility for an open source project is earned by instilling a thorough and sound test methodology. We assume that we've been lacking on this specific subject, doing our homeworks halfway. This will be addressed both for Elcodi core repository and web applications ([BambooStore](https://github.com/elcodi/bamboo-store) and [BambooAdmin](https://github.com/elcodi/bamboo-admin)) by raising the coverage of unit and functional tests and start working with acceptance tests for the web apps.

We have not decided yet which toolset we will be using, surely one between [Behat3](http://docs.behat.org/) and [PHPSpec2](http://www.phpspec.net/). As always, suggestions are more than welcome!

### PaymentSuite

The [PaymentSuite](http://github.com/PaymentSuite) was conceived and grown inside Elcodi.

It is a payment processing suite developed in Symfony2 which follows our vision of extensibility using adapters and dispatched events to give maximum flexibility to implementers and to decouple specific `Cart` or `Order` implementations. It promotes easy and sensible defaults for configurations.

We will take care of the project and will evolve it as the default payment platform. The [BambooStore](http://bamboo.elcodi.com) is already using a dummy [free payment](https://github.com/PaymentSuite/FreePaymentBundle) method.

### ShippingSuite

We would like to follow a similar path as the PaymentSuite: a separated and parallel project that does not depend on Elcodi and that can be easy integrated in web applications. Same vision, same resilience philosophy: work with `Order` or `Carrier` as *adaptees*.

### Documentation

Akin to the phisolophers' stone, sibling of the Holy Grail, the documentation is the most precious gemstone that an open source project should strive to shape. As Symfony2 author @fabpot once said, it is the most challenging part of any project.

We are working on the foundation of an [english documentation](http://elcodi.readthedocs.org), localized and translated version will follow later on.

* Implementation guidelines. General vision and coding philosophy, style and everything needed for developers wishing to contribute.
* Customizations. Craft a project using only Elcodi core. Architectural constraints to keep in mind. Recipes and examples.
* Core bundles documentation, including
  * Provisioned Model
  * Event system rationale and taxonomies
  * Services
  * Configuration
  * Controllers and extensions, where it may apply
* [Bamboo](https://github.com/elcodi/bamboo-store) [web applications](https://github.com/elcodi/bamboo-admin) documentation for Symfony and front-end developers: the starting point to develop your e-commerce site.

This is going to be an epic effort and the real main entrance to the project for the community. No matter the size of your codebase is, documentation is a key measure of a project success.

## Community

If documentation should be our *diamond*, the community should be our *propellant*. A good community make a project good.

We started as a small project and we know how demanding it can be to get involved and contribute to the development of an open source based product.

We will take special care of this particular topic, since motivating people to get involved is essential for our vision: Elcodi was never mean to be a mere code repository, it is a group of visionary people with a clear view in their minds, working together to work out a specific problem.

Taking care of the code means caring for the people that make it possible for the code to be crafted. We are especially interested in helping those who want to learn, so we will soon arrange training and learning mechanisms, both for Symfony and Elcodi. 

Get ready to be effective!

