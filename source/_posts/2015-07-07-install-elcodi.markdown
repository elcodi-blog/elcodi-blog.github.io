---
layout: post
title: "Install Elcodi"
date: 2015-07-07 12:59:39 +0200
comments: true
author: Marc Morera
categories: 
    - elcodi
    - ecommerce
    - symfony
    - install
---
In order to decrease the time of installation of Elcodi, and with our new Beta
release 

[Bamboo v1.0.0-beta3](https://github.com/elcodi/bamboo/blob/master/CHANGELOG-1.0.md#v100-beta3-10-07-2015)  
[Elcodi v1.0.0-beta3](https://github.com/elcodi/elcodi/blob/master/CHANGELOG-1.0.md#v100-beta3-10-07-2015)

we've been working on how fast you can create a new shop in your localhost and 
start tuning it. It is important right?

## New elcodi:install command

Elcodi is Symfony based. This means that to make it work you need to execute 
some commands in order to do some common actions (create database, update
schema, populate fixtures...)

To improve this step, we have created a new console command in order to decrease
this installation time and the number of steps required to play with your
localhost.

4 steps to rule them all, and less than 3 minutes to start with Elcodi.

``` bash
$ curl -s http://getcomposer.org/installer | php
$ php composer.phar create-project elcodi/bamboo bamboo -sdev
$ cd bamboo/
$ php app/console elcodi:install
```

As soon as Bamboo is installed, you can create a temporary server by using the
PHP built-in server

``` bash
$ php app/console server:run
```

And check your Bamboo installation in your browser by accessing to 
`http://localhost:8000`.

## GD instead of ImageMagick

Because we have implemented some needs using adapters, we have implemented a new
image resize adapter based on the PHP GD engine. The good thing is that GD is 
already installed in all standard PHP versions, so you don't really need to
install any extra library.

This new adapter has been proposed and accepted as the default one, and of 
course, ImageMagick adapter can be still used by changing the configuration
value.

``` yml
elcodi_media:
    images:
        resize:
            adapter: elcodi.media_resize.imagemagick
```

## Location repository

In Elcodi you can populate new countries. With this new installation command, 
and by using `--country` option, you can add already generated countries in 
seconds. You can see available pregenerated countries in our
[LocationDumps](https://github.com/elcodi/LocationDumps) repository. We will
generate all countries over time.

> If you want to use the demo application, make sure you install Spain. All
> already built addresses are from Spain. Otherwise, feel free to build your
> Location database as you need.

Imagine you want to install France and Italy. Well, you should do that

``` bash
$ php app/console elcodi:install --country=FR --country=IT
```
