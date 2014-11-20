---
layout: post
title: "Security component"
date: 2014-11-19 22:39:51 +0100
comments: true
categories: [One Section One Component]
author: Berny Cantos <berny@elcodi.com> 
---
The next in line in our ["One section, one component"](/blog/categories/one-section-one-component/) series is the [`Security component`](http://symfony.com/doc/current/components/security). It's a complex one, sure, but we'll take a look just to the nuts and bolts of it. Ready? Set… Go!

## Description

This component brings authentication and authorization to your applications, with implementations for common use cases, like form login, HTTP or X.509 authentication, authorization by user, roles and ACL systems.

## Basic classes

- **Token**: Stores current user credentials.
- **Listener**: extract information from the Request by which the user might be authenticated. It should create a valid token and set it into the **SecurityContext**[^1].
- **FirewallMap**: maps a [`RequestMatcher`](https://github.com/symfony/HttpFoundation/blob/master/RequestMatcher.php) to a set of `Listener`s.
- **Firewall**: Integrates the `FirewallMap` with `EventDispatcher` and `HttpKernel` components.

## Example of usage

```php
$exceptionListener = new ExceptionListener( … );
$matcher = new RequestMatcher('^/admin/');
$listeners = [
	new AnonymousAuthenticationListener( … ),
	new LogoutListener( … ),
];

$firewallMap = new FirewallMap();
$firewallMap->add($matcher, $listeners, $exceptionListener);

$firewall = new Firewall($firewallMap, $dispatcher);
```

## Other classes

- **UserProvider**: Finds a user given his credentials.
- **AutheticationEntryPoint**: When a match is found but no authentication info can be extracted by the request, this has the chance to return a specific response asking for credentials. For example, the [FormAuthenticationEntryPoint](https://github.com/symfony/Security/blob/master/Http/EntryPoint/FormAuthenticationEntryPoint.php) redirects to a login form.

## In Symfony2 framework

The [`SecurityBundle`](https://github.com/symfony/SecurityBundle) integrates this component into Symfony2, reading configuration from `security.yml` and adding some sane defaults.

## In Elcodi

[`Elcodi/User`](https://github.com/elcodi/User) depends on the `Security`  component for authentication. The main implementation of the `CustomerWrapper` extracts the current user from the `SecurityContext`. Also, in [our example application, Bamboo](http://bamboo.elcodi.com/), we depend on the excelent [`hwi/oauth-bundle`](https://github.com/hwi/HWIOAuthBundle), which is a good example of extending this component, for OAuth authentication.

## Conclusion

Almost every non-trivial application requires some kind of authentication. The `Security` component gives you all the power you should need, although it may be hard to use standalone. The integration with the framework makes for a much more enjoyable experience. We're already using this component! And you?

Have fun!

## Urls
- [Repository](https://github.com/symfony/Security)
- [Documentation](http://symfony.com/doc/current/components/security)
- [Packagist](https://packagist.org/packages/symfony/security)

[^1]: [**SecurityContext** is deprecated since Symfony 2.6 in favor of **TokenStorage**](http://symfony.com/blog/new-in-symfony-2-6-security-component-improvements).
