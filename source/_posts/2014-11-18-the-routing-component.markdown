---
layout: post
title: "The Routing Component"
date: 2014-11-18 22:39:54 +0100
comments: true
categories: [One Section One Component]
author: Berny Cantos <berny@elcodi.com>
---

As a complement to the [#SymfonyWalk](http://mmoreram.com/blog/2014/08/07/symfony-walk-zaragoza-madrid/) we start ["One section, one component"](/blog/categories/one-section-one-component/), a series of posts about some of the symfony components. Let's start with routing!

## Description

As each component does, the [Routing component](https://github.com/symfony/Routing) focuses and solves a small problem, in this case, matching urls to routes, so we can extract related information, and vice versa.

Things you can do with this component:

- Load your routes from anywhere into a collection.
- Check if a request matches any of the routes.
- Extract route-specific info from the request.
- Generate a url from a route and route specific info.

## Basic classes

The cast of this component is:

- **`Route`**: definition of a route, with path, requirements, options and conditions…
- **`RouteCollection`**: this bundles one or more `Routes` and allows to configure all of them at once.
- **`Matcher`**: Searches a `RouteCollection` for a matching uri.
- **`RequestContext`**: Context from the current request.

## Example of usage

```php
$route = new Route('/{name}');
$route->setDefaults([
    '_controller' => 'GreeterController',
]);
$route->setRequirements([
	'name' => '[a-z]+',
]);

$routes = new RouteCollection();
$routes->add('greet', $route);
$routes->addPrefix('/hello');

$context = new RequestContext();
$matcher = new UrlMatcher($routes, $context);
$parameters = $matcher->match('/hello/fabien');

/*
 * $parameters === [
 *   '_route' => 'greet',
 *   '_controller' => 'GreeterController',
 *   'name' => 'fabien',
 * ]
 */
```

## Other classes

Other supporting actors:

- **Loader**: Loads a `RouteCollection` from a source.
- **Generator**: The opposite of `Matcher`, given a route name and route-specific information, generates the uri.
- **Dumper**: The opposite of `Loader`, dumps a `RouteCollection` in different formats.
- **Router**: basically a [facade](http://en.wikipedia.org/wiki/Facade_pattern) to `Matcher` and `Generator`.

## In Symfony2 framework

Symfony2 framework reads routes from routing files with this component, matches the current `Request` and chooses which `Controller` and action to run. It also extends the `Router` so you can use parameter placeholders from [`DependencyInjection`](https://github.com/symfony/DependencyInjection) and integrates with the [`Config`](https://github.com/symfony/Config) component to allow caching.

## In Elcodi

In Elcodi, as a library, we try not to couple our very few controllers to a specific route and we leave that responsibility to the layers above. For more complex scenarios, like the [`MediaBundle`](https://github.com/elcodi/MediaBundle) and the forthcoming [`PageBundle`](https://github.com/elcodi/elcodi/tree/ec8fabe99489ae9dd1ef671eefaaa7ac89e24cdb/src/Elcodi/Bundle/PageBundle), we implement our own custom `Loader` so we can route through them.

## Conclusion

This component deals with the next step just after getting your [HTTP Request](http://symfony.com/doc/current/components/http_foundation) and gives you great power and flexibility to split your application into several routes.

I hope you find this component useful!

## Urls
- [Repository](https://github.com/symfony/Routing)
- [Documentation](http://symfony.com/doc/current/components/routing)
- [Packagist](https://packagist.org/packages/symfony/routing)
