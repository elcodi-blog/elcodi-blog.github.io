---
layout: post
title: "A week of Elcodi #1 (27/10 - 3/11)"
date: 2014-11-04 09:32:57 +0100
comments: true
categories: A week of Elcodi
author: Aldo Chiecchia <alch@elcodi.com>
---

It is time to be more vocal about what we are doing and share with the community how we are moving forward along our roadmap and the progress we are making. 

Inspired by the [famous symfony weekly bulletin](http://symfony.com/blog/category/a-week-of-symfony), we would like to do something similar: *A week of Elcodi*, a series of post assembled in turn by each member of the team.

This week we've been working on [social connectivity for our bamboo store](https://github.com/elcodi/bamboo-store/issues/44), allowing our users to log in from a variety of OAuth services through integration of [HWI/OAuthBundle](https://github.com/hwi/HWIOAuthBundle) in our project.
A thin layer and a new entity [`Authorization`](https://github.com/elcodi/bamboo-store/blob/2134302656ae13768ab8e82e54053d01f105d861/src/Elcodi/StoreConnectBundle/Entity/Authorization.php#L31) stores and retrieves links between our [`CustomerInterface`](https://github.com/elcodi/elcodi/blob/master/src/Elcodi/Component/User/Entity/Interfaces/CustomerInterface.php) and the OAuth token. And voilà! You can login with your GitHub account in two clicks!

Sticking with our bamboo store, we prepared a feature branch with PayPal [web checkout](https://developer.paypal.com/docs/integration/web/web-checkout/) integration. It has been developed on top of the [PaymentSuite](https://github.com/PaymentSuite) project. You can check out the changes [here](https://github.com/elcodi/bamboo-store/compare/features/paypal-payment?expand=1).

We have also been working on a *Finite-state machine*. Until now we were managing [`Order`](https://github.com/elcodi/elcodi/blob/master/src/Elcodi/Component/Cart/Entity/Order.php#L38) and [`OrderLine`](https://github.com/elcodi/elcodi/blob/master/src/Elcodi/Component/Cart/Entity/OrderLine.php#L34) state changes using an [hardwired implementation](https://github.com/elcodi/elcodi/blob/master/src/Elcodi/Component/Cart/Services/OrderStateManager.php#L28) which was very specific to the `Cart` component, while any logic implementing a FSM should be reusable. While waiting for Symfony to implement its own state machine in the near futire, we wrote a very simple and useful component that can be used in any project. You can check and review the [pull request](https://github.com/elcodi/elcodi/pull/354)

In terms of specific e-commerce features, we just merged some improvements to the Shipping component and Bundle. See the [Pull Request](https://github.com/elcodi/elcodi/pull/309).

Stay tuned!

