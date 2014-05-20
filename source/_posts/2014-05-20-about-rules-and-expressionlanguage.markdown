---
layout: post
title: "About Rules and ExpressionLanguage"
date: 2014-05-20 16:06:59 +0200
comments: true
categories: 
---

> Puedes leer este post en castellano en [blog de Marc Morera](http://mmoreram.com/blog/2014/05/20/about-rules-and-expression-language/)

This bundle works with Symfony's ``ExpressionLanguage``

To install it, understand how it works or just get a reference for the syntax of the Expressions you can read the [Official Documnentation](http://symfony.com/doc/current/components/expression_language/index.html)

## Our needs

We actually might be facing one of the trickiest problems that any experienced e-commerce developer could encounter: behaviour rules. One of the most significant example on this are coupon codes to be applied during the checkout process. How can we actually model the validation rules for a coupon code? The number of variables to be considered in order to validate (and apply) those rules can be daunting, especially when those rules belong to very specific use cases, so the challenge here is how to generalize the programming logic in coupon rules in a project-agnostic way to promote reusability.

Solving this problem is a major concern for us, since it would iron out a lot of similar scenarios where programmable actions should be applied. We could, for instance, design a flexible ``eventDispatcher`` that triggers some events during ``Request`` lifecycle when certains conditions (rules) are met.

## Our proposal

[Elcodi RuleBundle](https://github.com/elcodi/RuleBundle)

As usual, we try to sort the solution out enforcing our _single responsibility_ and _loose coupling_ obsessions. The rule engine must be capable of handling them in spite of the context where those rule should be checked and evaluated, and its only responsibility is to return a ``true`` or ``false`` value. 

We modeled two kinds of such evaluable entities, ``Rule`` and ``RuleGroup``. The first one, as simple as it looks, represents a single rule that returns a ``boolean`` upon validation. The second entity, ``RuleGroup`` represents a ``Collection`` of ``Rule`` so that we can validate the whole group by acting upon each ``Rule`` in the collection. Needles to say that the interface used to validate either ``Rule`` or ``RuleGroup`` should be homogeneous in terms of client code.

This all smells like a _Composite Pattern_ doesnt it? Be it ``Rule`` or ``RuleGroup`` we just want to issue those objects, that can now be modeled as a self-referencing entity, one command: *evaluate yourself*.

## Composite Pattern using Doctrine

Let's step backwards a bit and skim over the [Composite Pattern](http://sourcemaking.com/design_patterns/composite) definition.

![Composite pattern](http://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Composite_UML_class_diagram_%28fixed%29.svg/900px-Composite_UML_class_diagram_%28fixed%29.svg.png)

In this class diagram we have ``Leaf`` and ``Composite`` as separate entities. ``Leaf`` (called also ``Primitive``) can be see as a ``Rule`` and the ``Composite`` could be our ``RuleGroup``. Since both extends a common abstract class called ``Component``, it is easy to share common behaviors between the class hierarchy. Thus the ``Component`` abstract class defines an ``operation()`` method that can be overriden by child classes. Obviously this can also be achieved by using an interface as a Component instead of an abstract class.

In our case the ``operation()`` method should physically evaluate and resolve the expressions associated with the ``Component``, be it a single ``Rule`` or a ``RuleGroup`` or a combination of them. Of course this is done by implementing a concrete ``operation()`` method in derived classes.

The ``RuleGroup`` owns a collection of ``Component``, that can be ``Rule`` or ``RuleGroup``. This is the key for correctly mapping those entities to a concrete relational model. Once we do that, we would have forged a _whole-part_ awareness into our Rule model, and we could easily handle complex relations such as ``RuleGroup`` containing ``RuleGroup``. 

[Doctrine inheritance](http://docs.doctrine-project.org/en/2.0.x/reference/inheritance-mapping.html) is needed to implement this hierarchy. Either [Single Table Inheritance ](http://docs.doctrine-project.org/en/2.0.x/reference/inheritance-mapping.html#single-table-inheritance) or [Class Table Inheritance](http://docs.doctrine-project.org/en/2.0.x/reference/inheritance-mapping.html#class-table-inheritance) can be used. In this example, we will use STI.


The ``Rule`` entity is defined as follows:

```php
<?php

/**
 * This file is part of the Elcodi package.
 */

namespace Elcodi\RuleBundle\Entity;

/**
 * Class Rule
 */
class Rule extends AbstractRule implements RuleInterface
{
    /**
     * @var ExpressionInterface
     *
     * Expression
     */
    protected $expression;

    /**
     * Specific class parameters, getters and setters
     */

    /**
     * Return all object contained expressions
     *
     * @return ArrayCollection Collection of expressions
     */
    public function getExpressionCollection()
    {
        return new ArrayCollection(array($this->getExpression()));
    }
}
```

This is ``RuleGroup ``:

```php
<?php

/**
 * This file is part of the Elcodi package.
 */

namespace Elcodi\RuleBundle\Entity;

/**
 * Class RuleGroup
 */
class RuleGroup extends AbstractRule implements RuleGroupInterface
{
    /**
     * @var Collection
     *
     * Rules
     */
    protected $rules;

    /**
     * Specific class parameters, getters and setters
     */

    /**
     * Return all object contained expressions
     *
     * @return ArrayCollection Collection of expressions
     */
    public function getExpressionCollection()
    {
        $expressions = array();

        /**
         * @var AbstractRuleInterface $rule
         */
        foreach ($this->getRules() as $rule) {

            $expressions = array_merge(
                $expressions,
                $rule->getExpressionCollection()->toArray()
            );
        }

        return new ArrayCollection($expressions);
    }
}
```

And then the ``Component`` abstract class, which is called ``AbstractRule``:

```php
<?php

/**
 * This file is part of the Elcodi package.
 */

namespace Elcodi\RuleBundle\Entity\Abstracts;

/**
 * Class AbstractRule
 */
abstract class AbstractRule extends AbstractEntity
{
    /**
     * Specific class parameters, getters and setters
     */

    /**
     * Return all object contained expressions
     *
     * @return Collection Collection of expressions
     */
    abstract public function getExpressionCollection();
}
```

The method ``getExpressionCollection()`` can be called to each member of a ``AbstractRule`` ArrayCollection and the correct implementation will be invoked according to the actual type of the object.

```php
<?php
/**
 * @var Collection $rules 
 */
foreach ($rules as $rule) {

    /** 
     * @var ArrayCollection $expressionCollection 
     */
    $expressionCollection = $rule->getExpressionCollection();
    ...
}
```

This is how we would map our classes to Doctrine ORM using [yaml syntax](http://docs.doctrine-project.org/en/2.0.x/reference/yaml-mapping.html);

The ``Rule`` entity has an _unidirectional_ ``oneToOne`` relation with ``Expression``. In UML terms this should be a [composition association](http://en.wikipedia.org/wiki/Object_composition#UML_notation).

```yml
Elcodi\RuleBundle\Entity\Rule:
    type: entity
    repositoryClass: Elcodi\RuleBundle\Repository\RuleRepository
    table: rule

    oneToOne:
        expression:
            targetEntity: Elcodi\RuleBundle\Entity\Expression
            cascade: [ all ]
```

``RouleGroup`` holds a ``manyToMany`` _unidirectional_ relation with ``AbstractRule``. We are not interested in accesing the _parent_ for a given ``AbstractRule``.

```yml
Elcodi\RuleBundle\Entity\RuleGroup:
    type: entity
    repositoryClass: Elcodi\RuleBundle\Repository\RuleGroupRepository
    table: rule_group

    manyToMany:
        rules:
            targetEntity: Elcodi\RuleBundle\Entity\Abstracts\AbstractRule
            joinTable:
                name: rule_group_rule
                joinColumns:
                    rule_group_id:
                        referencedColumnName: id
                inverseJoinColumns:
                    rule_id:
                        referencedColumnName: id
```

And finally the mapping for the Component interface, the ``AbstractRule``


```yml
Elcodi\RuleBundle\Entity\Abstracts\AbstractRule:
    type: entity
    repositoryClass: Elcodi\RuleBundle\Repository\AbstractRuleRepository
    inheritanceType: single_table
    discriminatorColumn:
        name: discr
        type: string
    discriminatorMap:
        rule: Elcodi\RuleBundle\Entity\Rule
        rule_group: Elcodi\RuleBundle\Entity\RuleGroup
    fields:
        ...
```

Since we used STI, this mapping will only create a single table with a discriminator column used by Doctrine in order to identify the actual type of the entity stored on a given row.

As for now, we have a working implementation that can be used to create associations between ``AbstractRule`` and other doctrine entities. What we miss is *how* rules are processed, or better, how *expressions* owned by the rules can be evaluated and in which *context*.

## Context configuration and customization

We define a *rule execution context* as a collection of elements that the rule has access to at expression evaluation time.

The natural way of evaluating generic expressions in Symfony is certainly the [ExpressionLanguage](http://symfony.com/doc/current/components/expression_language/index.html), which lies at the heart of our implementation and provides the expression evaluation. We will just use its standard parameter passing syntax to handle our context.

```php
$language = new ExpressionLanguage();

class Apple
{
    public $variety;
}

$apple = new Apple();
$apple->variety = 'Honeycrisp';

$this->assertTrue($language->evaluate(
    'fruit.variety === "Honeycrisp"',
    array(
        'fruit' => $apple,
    )
));
```

In this case, as seen in the [official documentation](http://symfony.com/doc/current/components/expression_language/introduction.html#passing-in-variables), the expression ``fruit.variety`` is evaluated in a context where ``fruit`` is an instance of the ``Apple`` class. The ``assert`` obviously resolves to ``true``.
In the next example, the ``assert`` will fail.

```php
$language = new ExpressionLanguage();

class Apple
{
    public $variety;
}

$apple = new Apple();
$apple->variety = 'Arlet';

$this->assertTrue($language->evaluate(
    'fruit.variety === "Honeycrisp"',
    array(
        'fruit' => $apple,
    )
));
```

Evaluation contexts can be passed to the rule engine in two different ways:

* The easy way: pass the context to the ``RuleManager`` when evaluating a specific rule.
* The complex yet powerful way: by using **Context configurators** (see next paragraph below for details)

In the following example we are retrieving the ``somecode`` named rule, whose stored expression is ``customer.id in [1..100]``. We evaluate this rule passing ``array('customer' => $myCustomer)`` as the evaluation context.

The expression will be evaluated according to the actual ``id`` for ``$myCustomer`` passed object being a number between 0 and 100.

```php
/**
 * Rule with code "somecode" has this expression assigned: 
 * "customer.id in [1..100]"
 */
$myCustomer = ...;
$ruleManager = $this->container->get('rule_manager');
$result = $ruleManager->evaluateByCode('somecode', array(
    'customer'  =>  $myCustomer
));
```

### Using Context Configurators

In real scenarios, we would need to access several services from within our expressions in order to lay powerful rules. 

```php
/**
 * Rule with code "somecode" has this expression assigned: 
 * "customer_wrapper.getCustomer().getId() in [1..100]"
 */

$ruleManager = $this->container->get('rule_manager');
$result = $ruleManager->evaluateByCode('somecode');
```

In the example above our expression is accessing the ``customer_wrapper`` service, which holds the logic necessary to return on instance of the current ``Customer``. If we want to evaluate the rule **without** having to pass the ``customer_wrapper`` as a parameter to the execution context in the ``evaluateByCode()`` method as seen in the other example we will have to exploit **context configurators**, which are tagged services implementing ``ContextConfigurationInterface``.

```php
<?php

/**
 * This file is part of the Elcodi package.
 */

namespace Elcodi\RuleBundle\Configuration;

use Doctrine\Common\Persistence\ObjectManager;

use Elcodi\RuleBundle\Configuration\Interfaces\ContextConfigurationInterface;
use Elcodi\RuleBundle\Services\Interfaces\ContextAwareInterface;

/**
 * Class ContextConfiguration
 */
class ContextConfiguration implements ContextConfigurationInterface
{
    /**
     * @var ObjectManager
     *
     * Object manager
     */
    protected $objectManager;

    /**
     * Construct method
     *
     * @param ObjectManager $objectManager Object manager
     */
    public function __construct(ObjectManager $objectManager)
    {
        $this->objectManager = $objectManager;
    }

    /**
     * @param ContextAwareInterface $contextAware
     */
    public function configureContext(ContextAwareInterface $contextAware)
    {
        $contextAware->addContextElement('manager', $this->objectManager);
    }
}
```

The example above shows how we can use a context configurator so that a service like the ``doctrine.orm.default.entity_manager`` can be bound to the rule execution context named as ``manager``.
Whis means that _any_ expression can refer ``manager`` and be sure that it refers to the default entity manager. ``manager.getRepository('ElcodiUserBundle:Customer')`` would be perfecly valid and will work globally, no need to pass the entity manager at expression evaluation time.

The context configurator must be tagged as follows in order to be collected by ``RuleBundle`` compiler passes and passed to the rule engine.

```yml
services:
    elcodi.core.rule.configuration.context:
        class: %elcodi.core.rule.configuration.context.class%
        arguments:
            entity_manager: @doctrine.orm.entity_manager
        tags:
            - { name: elcodi.rule_context_configuration }
```

## ExpressionLanguage configuration and customization

We also can exploit a very powerful feature that Symfony *ExpressionLannguage* deliver us: creating functions. We can create EP functions and inject them to the rule engine, making them available to all our expressions. This is done by defining a service that implement ``ExpressionLanguageConfigurationInterface``.

```php
<?php

/**
 * This file is part of the Elcodi package.
 */

namespace Elcodi\RuleBundle\Configuration;

use Symfony\Component\DependencyInjection\ContainerInterface;

use Elcodi\RuleBundle\Configuration\Interfaces\ExpressionLanguageConfigurationInterface;
use Elcodi\RuleBundle\Services\Interfaces\ExpressionLanguageAwareInterface;

/**
 * Class ExpressionLanguageConfiguration
 */
class ExpressionLanguageConfiguration implements ExpressionLanguageConfigurationInterface
{
    /**
     * @var ContainerInterface
     *
     * Container
     */
    protected $container;

    /**
     * Construct method
     *
     * @param ContainerInterface $container Container
     */
    public function __construct(ContainerInterface $container)
    {
        $this->container = $container;
    }

    /**
     * Configures expression language
     *
     * @param ExpressionLanguageAwareInterface $expressionLanguageAware Expression Language aware
     */
    public function configureExpressionLanguage(ExpressionLanguageAwareInterface $expressionLanguageAware)
    {
        $expressionLanguage = $expressionLanguageAware->getExpressionLanguage();
        $expressionLanguage
            ->register('service', function ($arg) {
                return sprintf('$this->get(%s)', $arg);
            }, function (array $variables, $value) {
                return $this->container->get($value);
            });

        $expressionLanguage
            ->register('parameter', function ($arg) {
                return sprintf('$this->getParameter(%s)', $arg);
            }, function (array $variables, $value) {
                return $this->container->getParameter($value);
            });
    }
}
```
In this example we just defined a couple of functions, ``service()`` and ``parameter()``. Those are the same functions that get registered when using EL for configuring services in the ``DependencyInjection`` component. 
They also need the whole container in order to work properly (yes, normally you shouldn't do that but this is an example and in this case you actually have full control control of what is exposed since *you* write the expression function). 

For this to work, this service also must be tagged. Definition follows:

```yml
services:
    elcodi.core.rule.configuration.expression_language:
        class: %elcodi.core.rule.configuration.expression_language.class%
        arguments:
            service_container: @service_container
        tags:
            - { name: elcodi.rule_expression_language_configuration }
```

This will make it possible to execute a rule of the likes ``service("my_service")->getValue(parameter("my_value"))``. No need to define a context configurator to access ``my_service``. 

The ``RuleManager`` will silently discard any ``SyntaxError`` and return ``false`` on such cases. This is because it is not possible to differentiate between real syntax errors and inconsistent use of the object/variables passed to the context. An expression that depends on an outdated version of an object, be it a service or an entity, should not break the evaluation loop.

## Cupons and Rules

We not have all the ingredients to design a rule based coupon system for our e-commerce site. Let's see how we can bake the cake.

### Prerequisites

* We would model a ``ManyToMany`` unidirectional relation between ``Cupon`` and ``AbstractRule``.
* We would model a use case for customers who apply a discount to theirs checkouts by inserting a coupon code. 
 
### Steps

* Customer enters the coupon.
* The ``CouponManager`` tries to apply the Coupon to the customer's cart.
* A ``PreApplyToCart`` event is fired.
* An observer for the ``PreApplyToCart`` event searches for all Rules associated with the Coupon and evaluates their expressions.
* If one of those expression evaluates to ``false``, the whole evaluation will fail. A specific ``Exception`` will be thrown.
* If all expressions evaluates ``true``, the Coupon discount will be applied to current customer's cart.

## Conclusion

``RuleBundle`` is a fairly powerful tool that can be used as a seed for more specific needs such as **discount campaigns**, **customer groups**, **cart rules**, **catalog rules** and pretty much everything that is based on chaining expressions. 

As always, there is room for improvement. You can help us with that by [contribuiting](https://github.com/elcodi/elcodi/tree/master/src/Elcodi/RuleBundle) or just by letting us know your opinions.