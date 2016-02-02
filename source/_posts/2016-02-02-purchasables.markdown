---
layout: post
title: "Purchasables"
date: 2016-02-02 16:20:49 +0100
comments: true
author: Marc Morera
categories:
    - purchasable
    - elcodi
    - roadmap
---
It's been a long time since our last blog post, but we have some very good news
to share with you.

During the last months we've been working on new projects for Elcodi. Some of
them are not available yet, but some of them have been merged recently. This
post is basically to show you what has changed and improved in Elcodi and to
spread the word about our new Roadmap for the next months. This roadmap is
exactly the same one, but with different dates due to these new changes.

### Purchasable

From almost the beginning of the life of our project, and because the entity
product and related ones are one of the main topic of all e-commerce projects,
we have tried to manage how abstract should be all this entity family. For
example, should we create a Purchasable interface to be used in Cart and Order?
Should we use only one entity to manage all kind of purchasable entities? Should
we use traits for that? How many interfaces?

Like all software projects, this is a question that can be solved in several
ways. One of these ways is analyzing our needs and creating a software model
from these needs. Of course, this is the right way if you have time, money and
resources. Another way is just by trying one approach. Working with it, dealing
with it as much as possible, and listening yourself, looking for your needs.
This approach needs time, only time, and the capacity of refactoring, of action,
of change.

Well. This one, the last one, is the approach we've used some time ago. At the
beginning, using two entities, Product and Variant, to define both purchasable
types has been a good choice. And it's been like this until now. Since some
months ago, one of the most asked questions have been... *well, how can I create
my own purchasable?*, and with this approach, was not simple, was not logical.

Well. We have reacted and a new Purchasable implementation is here.

Instead of dealing with products and variants, Elcodi's cart is going to deal
only with purchasable implementations. We've started using Doctrine CTI (Class
Table Inheritance) to allow other implementation coexist with current ones, so
if you need to work with your own purchasable implementation... just create it,
implement this interface and it's done :)

That simple!

### Elcodi v2.0.0

Our Roadmap was clear. Elcodi *v1.1.0* had to be tagged the first of January,
but we really wanted this feature to be available and in master as soon as
possible.

A change like that can needs a major version change, and even we know that is
too soon to create a new major version, we think that this is the best for the
project and the best for our evolution.

We are not specialists in maintaining several branches at all, so we will not
support versions *v1.0* actively. What we will do for sure is supporting somehow
all projects that want to go from *v1.0* to *v2.0*. At this moment we don't have
any tool for that but, if there are some requests, we will do it for sure.

So, if you need us, just [Ask for help](http://gitter.im/elcodi/elcodi)

### New Roadmap

Of course our roadmap has changed, but only with dates.  
Let's take a look at the new plan.

#### v2.1

* Released the 1st of June, 2016
* Search engine using ElasticSearch will be introduced here
* Product API will be added
* User API will be added
* Cart API will be added
* Some Media improvements, like new media types
* New shop template will be added
* Back panel documentation
* Code Coverage of all components and bundles over 60%
* Templates documentation

#### v2.2

* Released the 1st of September, 2016
* Multistore. Switch between stores in the same installation
* Shop API will be added
* New shop template will be added
* New tempates and plugins website with tons of cool new stuff for all Elcodi and Bamboo implementations
* Code Coverage of all components and bundles over 70%

#### v2.3

* Released the 1st of January, 2017
* We will remove all deprecated code
* Bamboo will use Symfony3 and maybe new version of Doctrine
* Updated all dependencies
* Marketplace
* Elcodi community site will be created.
* Unknown bunch of new and cool stuff
* Code Coverage of all components and bundles over 80%

### Try us

You can start using Elcodi in a very very fast way, just... DO IT!

[Install Elcodi](http://elcodi.io/docs/quick-start/)

### Last thoughts

Well, Elcodi project is keep doing. Remember that we need a lot of help, and we
really want you to test our project, take a small tour around what we offer and
if you like, only if you like...

[Star us](https://github.com/elcodi/elcodi)
[Join us](https://gitter.com/elcodi/elcodi)
[Help us](https://github.com/elcodi/elcodi/issues)
