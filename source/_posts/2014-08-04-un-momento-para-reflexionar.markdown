---
layout: post
title: "Un momento para reflexionar"
date: 2014-08-04 13:53:28 +0200
comments: true
categories:
    - elcodi
    - ecommerce
    - symfony
---
Algunos de vosotros ya conocéis Elcodi. Una serie de componentes hechos desde 0
con componentes de Symfony2 y Doctrine2, especialmente diseñados para
proporcionar el máximo de flexibilidad y de robustez a los proyectos Ecommerce.

Programar debe ser siempre un placer.

Este es nuestro objetivo desde el principio, y como todos los objetivos, no hay
eue olvidar que, hasta llegar a ellos, hay un camino no menos importante.

Creo que hemos llegado a un punto de inflexión donde es importante detenerse,
así que durante los próximos dias vamos a sacar una serie de posts referentes a
nuestra reflexiones donde pondremos sobre la mesa temas importantes como lo que
tenemos hecho hasta ahora, nuestra espectativas, y como vamos a hacer cada
trozo del proyecto durante los próximos meses.

El camino hasta ahora
=====================

Elcodi empezó hace unos meses en una bonita oficina de Barcelona. Nuestro
departamento técnico empezó a diseñar el proyecto a principios de año y hasta
ahora, y entre unos cuantos refáctorings interesantes, llegamos a un punto de
equilibrio, en mi opinión, bastante bueno, semilla de nuestra dirección actual.

Hasta ahora hemos trabajado sobretodo en funcionalidades básicas de interés
común en cuanto a ecommerce se refiere.

### ProductBundle, AttributeBundle

Todo lo referente al Producto, Categoría y Marca. El Bundle tiene definido tanto
el modelo como una serie de herramientas para trabajar con él. Una de las
últimas incorporacion ha sido la implementación de multiproducto, muy útil en
la mayoría de sites ecommerce de hoy en día.

También soporta multicategoría.

### UserBundle

Modelo y herramientas para trabajar tanto con Customer como con Administradores
para el Backoffice. También se ha creado una definición básica de autenticación
y de autentificación utilizando el ya conocido Secure de Symfony.

### CartBundle

Gestor de Carts y Orders. Una de las partes troncales de todo ecommerce viene
en un formato bastante sencillo, diseñado para poder ser sobreescrito
practicamente en su totalidad. Se ha implementado seguimiento de Orders mediante
histórico de estados, así las herramientas de transformación de Cart a Order.

### CouponBundle

Definición del modelo de cupón. Este bundle está particularmente desacoplado de
todos los demás, por lo que solo tiene dependencia con `core-bundle` y con
`currency-bundle`. Si se quiere trabajar con el CartBundle de Elcodi, también
se ha creado CartCouponBundle, un **nexo** entre ambos modelos que, a partir
de los eventos del EventDispatcher, se encarga del comportamiento asociado a los
cupones del cart.

### GeoBundle

Semilla para toda la definición y gestión de un modelo geográfico completo. Por
ahora, y dado que no hemos dedicado demasiado tiempo a esta parte, solo tenemos
Country como entidad, dejando como campos abiertos los demás campos como
`Region`, `City` y `Postalcode`

Otros Bundles más son

* BannerBundle
* LanguageBundle
* CurrencyBundle
* MediaBundle
* MenuBundle
* NewsletterBundle
* ReferralProgramBundle
* RuleBundle

### BambooStore

Hemos desarrollado una primera fase de un Store. Por ahora con camisetas muy
chulas y utilizando el
[FreePaymentBundle](https://github.com/PaymentSuite/FreePaymentBundle) de
PaymentSuite.

Es una versión muy simple, pero seguro muy útil como punto de partida para
muchos departamentos técnicos

### BambooAdmin

Algo nuevo. Hemos estado trabajando el último mes en una base (muy simple y algo
incompleta) de un administrador. Estará en constrante desarrollo (junto con
el store y el core, yendo de la mano en cuanto a versiones).


### Implementación

Sabemos que nuestra implementación no dejará a todo el mundo contento, y la
verdad no queremos que sea así. Consideramos que dejar a todos el mundo contento
es algo imposible, y lo que terminas teniendo es un producto sin ningún tipo de
identidad, por lo que nosotros preferimos definir bien nuestra dirección, y
desarrollar a pesar de los errores que podamos cometer.

Hemos recibido preguntas muy positivas del porqué estamos acoplándonos al
framework Symfony y al ORM de Doctrine2. Pues bien, lo hacemos porque nuestro
objetivo es realmente llegar a ser lo máximo útiles posible, tanto para el
desarrollador como para el usuario final, y la única forma de que sea así es
tratar con código mantenible, comprensible, documentado y estructurado.

En esta fase del proyecto no creemos que valga la pena añadir mas capas de
indirección para realizar esta desvinculación. El objetivo ahora mismo es
intentar involucrar la comunidad Symfony y recibir feedback de desarrolladores
que intenten montar algo con Elcodi.

En la busca de un tradeoff entre hacer las cosas lo máximo de bien posible, y
tratar de ser algo diferentes a los demás, hemos consideramos que ambas
relaciones son y serán buenas para el proyecto, así como para los
desarrolladores que decidan trabajar sobre él.