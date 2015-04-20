---
layout: post
title: "Introducing the Directors in Symfony"
date: 2015-04-20 12:36:03 +0200
comments: true
author: Marc Morera
categories:
    - symfony
    - elcodi
    - director
    - facade
    - factory
    - repository
---
Have you ever worked with Entity Managers, Repositories or Factories in Symfony?
Yes, for sure you've done that. Indeed, if you work with entities in your 
project, these type of objects will be your daily, so we want to introduce you a
new concept, born in [Elcodi project](http://github.com/elcodi/elcodi) and 
created in order to simplify the management of an entity inside a service, 
fixture or functional test.

Please, welcome the Director class.

### Director

Imagine you have a service called `CartWrapper`. This service is meant to be a
wrapper of a cart, and its behavior is that easy.

* If you require an existing user's cart, retrieve it from database and return 
it
* Otherwise, create a new Cart, save it and return it.

Let's see a small implementation of such service.

``` php
/**
 * CartWrapper
 */
class cartWrapper
{
    /**
     * @var EntityManager
     *
     * Cart entity manager
     */
    protected $entityManager;

    /**
     * @var CartFactory
     *
     * Cart factory
     */
    protected $cartFactory;

    /**
     * @var CartRepository
     *
     * Cart repository
     */
    protected $cartRepository;

    /**
     * Construct
     *
     * @param EntityManager  $entityManager  Entity manager
     * @param CartFactory    $cartFactory    Cart factory
     * @param CartRepository $cartRepository Cart repository
     */
    public function __construct(
        EntityManager $entityManager,
        CartFactory $cartFactory,
        CartRepository $cartRepository
    )
    {
        $this->entityManager = $entityManager;
        $this->cartFactory = $cartFactory;
        $this->cartRepository = $cartRepository;
    }

    /**
     * Load cart
     *
     * @param UserInterface $user User
     *
     * @return CartInterface
     */
    public function loadCart($user)
    {
        $userCart = $this
            ->cartRepository
            ->findOneBy([
                'user' => $user
            ]);

        if (!($userCart instanceof CartInterface)) {

            $newCart = $this
                ->cartFactory
                ->create($user);

            $this
                ->entityManager
                ->persist($newCart);

            $this
                ->entityManager
                ->flush($newCart);

            $userCart = $newCart;
        }

        return $userCart;
    }
}
```

As you can see, this services needs 3 dependencies, and would be difficult to
reduce this number in order to reduce the complexity of the class. This service
is intended to do one single thing.

What is the strategy here? Do you know what the *Facade pattern* is, right? 
Well, the way for solving this amount of classes is creating a new facade called 
Director, intended to provide a small set of usable methods simplifying the 
management of the entities.

Let's see the implementation of this class. You can find the real implementation
in [elcodi/core](https://github.com/elcodi/elcodi).

``` php
namespace Elcodi\Component\Core\Services;

use Doctrine\Common\Persistence\ObjectManager;
use Doctrine\Common\Persistence\ObjectRepository;
use UnexpectedValueException;

use Elcodi\Component\Core\Factory\Abstracts\AbstractFactory;

/**
 * Class ObjectDirector
 *
 * This object is a facade for these different persistence-related elements
 *
 * * Object Manager
 * * Repository
 * * Factory
 *
 * Allows final user to manage in a simple way an entity or a set of entities.
 * Provides a reduced (but useful) set of methods.
 */
class ObjectDirector
{
    /**
     * @var ObjectManager
     *
     * Manager
     */
    protected $manager;

    /**
     * @var ObjectRepository
     *
     * Repository
     */
    protected $repository;

    /**
     * @var AbstractFactory
     *
     * Factory
     */
    protected $factory;

    /**
     * Construct
     *
     * @param ObjectManager    $manager    Manager
     * @param ObjectRepository $repository Repository
     * @param AbstractFactory  $factory    Factory
     */
    public function __construct(
        ObjectManager $manager,
        ObjectRepository $repository,
        AbstractFactory $factory
    ) {
        $this->manager = $manager;
        $this->repository = $repository;
        $this->factory = $factory;
    }

    /**
     * Finds an object by its primary key / identifier.
     *
     * @param mixed $id The identifier.
     *
     * @return object|null Fetched object
     */
    public function find($id)
    {
        return $this
            ->repository
            ->find($id);
    }

    /**
     * Finds all objects in the repository.
     *
     * @return array Set of fetched objects
     */
    public function findAll()
    {
        return $this
            ->repository
            ->findAll();
    }

    /**
     * Finds objects by a set of criteria.
     *
     * Optionally sorting and limiting details can be passed. An implementation may throw
     * an UnexpectedValueException if certain values of the sorting or limiting details are
     * not supported.
     *
     * @param array        $criteria
     * @param array|null   $orderBy
     * @param integer|null $limit
     * @param integer|null $offset
     *
     * @return array Set of fetched objects
     *
     * @throws UnexpectedValueException
     */
    public function findBy(
        array $criteria,
        array $orderBy = null,
        $limit = null,
        $offset = null)
    {
        return $this
            ->repository
            ->findBy(
                $criteria,
                $orderBy,
                $limit,
                $offset
            );
    }

    /**
     * Finds a single object given a criteria.
     *
     * @param array $criteria The criteria.
     *
     * @return object|null Fetched object
     */
    public function findOneBy(array $criteria)
    {
        return $this
            ->repository
            ->findOneBy(
                $criteria
            );
    }

    /**
     * Create a new entity instance, result of the factory creation method
     *
     * @return Object new Instance
     */
    public function create()
    {
        return $this
            ->factory
            ->create();
    }

    /**
     * Save the instance into database. Given data can be a single object or
     * an array of objects.
     *
     * This method will persist and flush given object/array of objects
     *
     * @param object|array $data Data to save into database
     *
     * @return object|array Saved data
     */
    public function save($data)
    {
        if (is_array($data)) {
            foreach ($data as $entity) {
                $this
                    ->manager
                    ->persist($entity);
            }
        } else {
            $this
                ->manager
                ->persist($data);
        }

        $this
            ->manager
            ->flush($data);

        return $data;
    }

    /**
     * Remove the instance from the database. Given data can be a single object or
     * an array of objects.
     *
     * This method will remove and flush given object/array of objects
     *
     * @param object|array $data Data to remove from database
     *
     * @return object|array Removed data
     */
    public function remove($data)
    {
        if (is_array($data)) {
            foreach ($data as $entity) {
                $this
                    ->manager
                    ->remove($entity);
            }
        } else {
            $this
                ->manager
                ->remove($data);
        }

        $this
            ->manager
            ->flush($data);

        return $data;
    }
}
```

This implementation provides us a simple set of actions for our entities (maybe
the most used).

* find(...)
* findAll()
* findBy(...)
* findOneBy(...)
* create()
* save()
* remove()

This implementation is so useful for many scenarios, but in our case what we 
found is that using Directors, our tests are reduced in many ways (less lines of
code, less complexity, more effectiveness), and the fixtures are totally 
improved, focusing on the most important thing: our business logic.

### Directors as a service

To use a Director as a service you need to define your repository as a service
as well, assuming that your factory is already a service called `cart_factory` 
and assuming as well that we will work with the default entity manager 
`doctrine.orm.default_entity_manager` (not a good practice, but let's suppose it 
is in this example).

Let's see how to define your repository as a service.

``` yml
services:

    #
    # Doctrine Repository
    #
    cart_repository:
        class: Doctrine\Common\Persistence\ObjectRepository
        factory:
            - @elcodi.provider.repository
            - getRepositoryByEntityNamespace
        arguments:
            - My\Cart\Namespace
```

You will find the service `elcodi.provider.repository` inside the Bundle
`elcodi/core-bundle`. This bundle is not coupled to any Elcodi functionality so
you can use it in your own projects as a set of isolated features.

Finally, let's create the Director definition by using the factory pattern.

``` yml
services:

    #
    # Directors
    #
    elcodi.director.cart:
        class: Elcodi\Component\Core\Services\ObjectDirector
        arguments:
            - @doctrine.orm.default_entity_manager
            - @cart_repository
            - @cart_factory
```

Take in account that each entity will have it's own Director instance, so this
is why we use the factory pattern here.

How can we use the new Director service in our first `CartWrapper` 
implementation by changing and improving the services dependencies? Let's do 
that!

``` php
/**
 * CartWrapper
 */
class cartWrapper
{
    /**
     * @var ObjectDirector
     *
     * Cart director
     */
    protected $cartDirector;

    /**
     * Construct
     *
     * @param ObjectDirector $cartDirector Cart Director
     */
    public function __construct(ObjectDirector $cartDirector)
    {
        $this->cartDirector = $cartDirector;
    }

    /**
     * Load cart
     *
     * @param UserInterface $user User
     *
     * @return CartInterface
     */
    public function loadCart($user)
    {
        $userCart = $this
            ->cartDirector
            ->findOneBy([
                'user' => $user
            ]);

        if (!($userCart instanceof CartInterface)) {

            $newCart = $this
                ->cartDirector
                ->create($user);

            $this
                ->cartDirector
                ->save($newCart);

            $userCart = $newCart;
        }

        return $userCart;
    }
}
```

[Elcodi](http://github.com/elcodi/elcodi) is fully based on the usage of 
Directors. This does **not** mean that we must use Directors everywhere. 
Sometimes you need to use only one of the wrapped objects, and in that case you
must inject them one by one, so please, before using this new object think about
it, think about the pros and the contras and be responsible about your
implementation.

Don't hesitate to comment about what do you think about this feature.

Have a nice day!