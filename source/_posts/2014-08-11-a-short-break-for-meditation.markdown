---
layout: post
title: "A short break for meditation"
date: 2014-08-11 09:05:38 +0200
comments: true
categories:
    - elcodi
    - ecommerce
    - symfony
---

##### Puedes encontrar la versión en castellano de este post [aqui](/blog/2014/08/04/un-momento-para-reflexionar/).

Some of you already know Elcodi, a suite of components built on Symfony2 and Doctrine, aiming to provide maximum flexibility and soundness to e-commerce projects.

We believe that coding should always be entertaining and fun.

This has been our main mission since the beginning and as every ambitious goal you cannot forget that along the journey you might discover an equally important path.

I think that we reached the point where it is important to take a breath, so that in the following days we will publish some posts regarding our views and what we've been learning, where we will talk about what we have developed so far, our expectations and how we would like to drive the development of each part of the project in the following months.

The path so far
===============

Elcodi was born a few months ago in a nice workplace located in Barcelona, Spain. We started designing the platform at the beginning of the year, developed and refactored the pieces up the present day where in my opinion we reached a fairly good balanced point, which is the growing seed of our natural direction.

### ProductBundle, AttributeBundle

Those components are the home for everything regarding products, categories, manufacturers. Product attribute and options were added in a separate bundle and are the dimensions for working in the product variants space, which is one of the latest features developed and is a required trait for almost every e-commerce software implementation.

Multi-category is also supported for products.

### UserBundle

Contains the models and tools necessary for working with front-end customers or back-end administrators. A basic authentication & authorization definition was also added, exploiting Symfony Security component.

### CartBundle

It handles the purchase process, carts and orders. Undoubtedly one of the core pillars of any e-commerce, it has been designed to be simple to get a grasp of and to be overridden almost completely by custom logic. You can read about its internal in the [official documentation](http://elcodi.readthedocs.org/en/latest/bundles/CartBundle/index.html).
Point-in-time snapshots of order states or cart to order transitions are examples of what is provided in CartBundle core services default implementation.

### GeoBundle

Basic building block for a complete hierarchical aggregation of administrative entities. Due to lack of time, at the present day only `Country` is modeled as a mapped class, leaving `State`, `Province` or `City` as text fields. 

### MediaBundle

Allows the storage-agnostic handling of media files, such as images. It also controls image resizing and consolidates HTTP headers for media files.

Other bundles are:

* BannerBundle
* LanguageBundle
* CurrencyBundle
* MenuBundle
* NewsletterBundle
* ReferralProgramBundle
* RuleBundle

### BambooStore

We developed a prototype of a front-end store, the [Bamboo](http://bamboo.elcodi.com/). It is a showcase of Elcodi core features and uses [PaymentSuite](https://github.com/PaymentSuite) [FreePaymentBundle](https://github.com/PaymentSuite/FreePaymentBundle) as payment method.

### BambooAdmin

Something we just published. An initial release of a [backoffice application](https://github.com/elcodi/bamboo-admin), providing basic features yet usable. Likewise Elcodi core and the Bamboo frontend, it will be in constant development. Versions and releases of the three projects will stay synchronized, not to break dependencies.

### Implementation

We know that unfortunately, at least at this stage, our design choices and implementation cannot satisfy everyone. Trying to do it would require a massive effort and can lead to a loss of focus and direction. We prefer stick with our vision despite the errors we would make. 

A question we are frequently asked is why we are coupling with Symfony and Doctrine: this is a trade-off we had to settle for trying to maximize acceptance from both developers and end users by enforcing clean, coherent, structured and documented code.

Right now we don't think it is feasable to assemble more layers of indirection to achieve this decoupling. The goal is to involve the Symfony community and work on the feedbacks we will receive from developers who want to build applications with Elcodi. Mind you that Symfony decoupling *is* possible, since we are trying to follow the **SOLID** path, but the eternal struggle between being different and effective flows to a point of balance that surely will benefit both the project and our users.

Cheers!

