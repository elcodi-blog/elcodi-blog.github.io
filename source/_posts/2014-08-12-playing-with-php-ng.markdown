---
layout: post
title: "Playing with PHP-NG"
date: 2014-08-12 18:02:14 +0200
comments: true
published: false
categories:
    - elcodi
    - php
    - symfony
    - performance
---

There is a lot going on on the PHP community after Zeev Suraski published an [article](http://zsuraski.blogspot.com.br/2014/07/benchmarking-phpng.html) where he ventured on assembling a performance comparison between [PHPNG](https://wiki.php.net/rfc/phpng) and [hhvm](https://github.com/facebook/hhvm). Needles to say that the results are promising and the performace benefits of the soon-to-be next PHP major release look impressive.

For the sake of curiosity, we tried to run Elcodi [frontend store](https://github.com/elcodi/bamboo-store) on PHPNG and see how a Symfony applicaction would fare.

The test was made on an Amazon [c3.xlarge](http://aws.amazon.com/ec2/instance-types/#Compute_Optimized) EC2 instance ([HVM virtualization](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html)) running **Ubuntu Server 14.04 LTS**

## Installing PHPNG

There are several options available for installing PHPNG binaries. 

The first way is of course to follow the ninja path and [compile it on your own](https://wiki.php.net/phpng). Mind you that compiling PHPNG on an Ubuntu 14.04 is a source of headaches, since it requires [Bison 2.7](http://www.gnu.org/software/bison/) but `trusty` official repositories provide it at version `3.0.2`, so you'll need to manually download and build it.

The easiest way is of course to add an `apt` repository providing the package. For the sake of simpliciy we can stuck with apprentice path, katanas can wait for the next bad guy.

Luckily Zend prepared [an apt repo](http://repos.zend.com/zend-server/early-access/phpng/) for the `amd64` arch:

```bash
    # Remember to run these commands as root, or use sudo
	$ wget http://repos.zend.com/zend.key -O- 2> /dev/null | apt-key add -
	$ echo "deb [arch=amd64] http://repos.zend.com/zend-server/early-access/phpng/ trusty zend" > /etc/apt/sources.list.d/phpng.list
	$ apt-get update
	$ apt-get install php5
```
This wil also install `apache2.2` and `libapache2-mod-php5`.

In order for a Symfony application to work, we will have to add at least the [i18n](http://php.net/manual/en/book.intl.php) and the [mysqlnd](http://php.net/manual/en/book.mysqlnd.php) extensions.
Elcodi will also need [imagemagik](http://www.imagemagick.org/) as the default image resize adapter for the [MediaBundle](https://github.com/elcodi/MediaBundle). Since we will work with MySQL, we also need `mysql-server`.

**NO** For this test, to speed up things a little, we also need [APCu](https://github.com/krakjoe/apcu) since we will configure Doctrine [metadata](http://docs.doctrine-project.org/en/2.0.x/reference/caching.html#metadata-cache) [cache](http://symfony.com/doc/current/reference/configuration/doctrine.html#configuration-overview)

Summing it up:

```bash
    # As root user:
    $ apt-get install php5-intl php5-mysqlnd mysql-server imagemagik
```

Before proceeding, we are making a few assumptions:

* we created an apache `VirtualHost` for `test.example.com`, or whatever domain we'd like to use, in `/var/www/bamboo` 
* /var/www/bamboo is *writable* for user www-data, which is the user running apache in ubuntu
* [composer.phar](https://getcomposer.org/download/) is available and in PATH

Now we can install Elcodi front store as follows:

```bash
	# Run command as apache user, which in ubuntu is www-data
	# We assume that /var/www/bamboo is *writable* for user www-data
	$ cd /var/www/bamboo
    $ git clone https://github.com/elcodi/bamboo-store.git .
    $ composer.phar install -o --no-dev
    # Follow parameters in app/config/parameters.yml to configure MySQL access
    $ php app/console doctrine:database:create -e=prod
    $ php app/console doctrine:schema:create
    $ php app/console doctrine:fixtures:load --fixtures=vendor/elcodi/bamboo-fixtures/
    $ php app/console assets:install web && php app/console assetic:dump -e=prod --no-debug
```    
