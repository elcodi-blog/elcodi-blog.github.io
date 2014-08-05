---
layout: post
title: "Nuestro roadmap"
date: 2014-08-05 12:46:22 +0200
comments: true
categories:
    - elcodi
    - ecommerce
    - symfony
---
##### Este post es la continuación de [uno anterior](/blog/2014/08/04/un-momento-para-reflexionar/).

Podemos decir con orgullos que tenemos proyecto por mucho tiempo.

La pregunta es... ¿y ahora qué? Pues bueno, ahora solo hemos andado un uno por
ciento del camino al completo (Creo que la palabra "completo" es algo
inexistente en nuestro ámbito... siempre hay cosas que hacer y mejorar...).
Almenos, y mientras nuestra lista de Issues y Features no esté vacía, tenemos
ganas de mejorar nuestro repositorio. Aquí algunas cosas importantes.

### GeoBundle

Consideramos que es uno de los problemas más trabajados en todas las empresas
ecommerce, tener un buen sistema geográfico. Tenerlo implica poder segmentar
mucho mejor y cotejar los datos, para sacar buenas métricas y así mejorar la
experiencia de usuario.

Queremos trabajar en un completo sistema relacional de entidades, de tal forma
que, con comandos simples se puedan customizar y poblar tablas enteras.

### Gestión de Reglas

Una parte interesante del proyecto es la utilización de reglas en todo el
modelo. Por ejemplo.

* ¿Cuando se aplica un cupón automático de cart?
* ¿Cuando es posible aplicar un cupón cuando se aplica?
* ¿Cuando es posible mezclar dos productos?
* ¿Cuando se puede utilizar un Carrier o un método de pago?

Podemos solucionar de una forma muy natural y intruitiva todas estas necesidades
con un buen sistema de reglas.

### Ports and Adapters

Una de las cosas que mas nos preocupa es el poder trabajar con los componentes
de forma desacoplada. Creo que estamos enmedio del camino, por lo que queremos
aprender como hacerlo mejor. Los "Ports and Adapters" es una forma de trabajar
con implementaciones propias del componente, implementando adapters para las
implementaciones externas. De esta forma, podemos decir que un Cart tiene un
Customer, sin tener la necesidad de trabajar con CustomerBundle.

Trabajaremos en ello. Seguro.

### Tests

Hemos hecho los deberes a medias, lo reconocemos, y dado que sabemos que parte
de la confianza de un proyecto open source se genera con la cantidad y,
importante, la calidad de sus tests, vamos a trabajar para que ambas cosas
crezcan a la vez, tanto en los bundles de Elcodi como en los proyectos
BambooStore y BambooAdmin.

Trabajaremos en las baterías de Tests unitarios y funcionales, como hemos hecho
hasta ahora, incluso incrementando notablemente el nivel, y en las baterias
de tests de comportamiento en las Bamboo.

No tenemos muy claro que proyectos utilizar para tales fines. Tenemos en mente
Behat3 y PHPSPec2, así que toda opinión sobre el tema serás más que bienvenida!

### PaymentSuite

El proyecto [PaymentSuite](http://github.com/PaymentSuite) nació dentro de
Elcodi.

Es un repositorio de pagos implementado en Symfony2, preparado para trabajar
100% sobre una capa de eventos. Esta capa te permite poder abstraer la
implementación del Cart o del Order, así como la implementación del método de
pago, por lo que la activación o desactivación de los métodos de pago se hacen
triviales, solo activando y configurando.

Desde Elcodi vamos a trabajar en el proyecto.

### ShippingSuite

Queremos hacer lo mismo que con PaymentSuite. Un proyecto que trabaje sobre la
capa de eventos de Symfony y abstraiga, tanto el Order como la implementación
del Carrier.

Con los pagos nos ha funcionado a la perfección, así que seguro que vamos a
seguir el mismo camino para esta necesidad.

### Documentación

El diamante que buscar, encontrar, limpiar y tallar. La famosa documentación.
Consideramos que es la verdadera joya de la corona, como dijo una vez @fabpot,
la documentación es la parte más complicada del proyecto, y lo sabemos.

Vamos a trabajar para hacer una buena documentación en inglés, así como la
traducción en el máximo de idiomas posibles.

* Documentación de implementación. Estilos que hemos seguido, así como puntos
importantes a conocer para los contribuidores.
* Documentación de customización. En realidad consistirá más en un conjunto de
recetas.
* Patrones que hemos utilizado y sus razones de ser
* Documentación por Bundle.
 * Modelo
 * Capa de eventos (dispatchers y listeners)
 * Capa de servicio
 * Capa de extensiones de Twig
 * Capa de configuración
 * Capa de controlador (en el caso de las implementaciones de front y back)
* Documentación de front. Que necesita un frontend para realizar
implementaciones propias de frontal

Sabemos que no será tarea fácil, pero al menos estamos seguros que parte del
éxito de cualquier proyecto, por pequeño que sea, es la capacidad de documentar
que pueda llegar a tener, así como lo acogedor que pueda llegar a ser el entorno
del proyecto.

### Comunidad

Si la documentación es nuestro diamante, la comunidad es nuestro carburante. Una
buena comunidad hace el buen proyecto.

Nacimos como un pequeño proyecto en Symfony y sabemos lo complicado y agresivo
que puede llegar a ser, para una persona no acostumbrada a estar en entornos
tan estrictos, el hecho de participar y involucrarse en el desarrollo de una
tecnología.

Pues bien, como lo hemos vivido y sabemos que es, en parte, la clave de nuestro
éxito, vamos a tratar este punto con especial interés y cariño. Elcodi no es tan
solo un código en un repositorio, es un conjunto de personas con una idea clara
en la cabeza y con la ilusión de trabajar juntas para solucionar un problema.

Es por esto que si queremos cuidar el código al máximo, debemos cuidar a las
personas que hacen posible que el código exista. Tenemos mucho interés en que la
gente que quiera aprender, lo haga, así que vamos a diseñar sistemas de learning
y de formación, tanto en Symfony como en Elcodi, para que la gente que quiera
participar con nosotros, lo pueda hacer con el máximo nivel posible.