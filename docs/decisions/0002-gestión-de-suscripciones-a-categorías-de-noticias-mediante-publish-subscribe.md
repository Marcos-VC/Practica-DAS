# Gestión de suscripciones a categorías de noticias mediante Publish-Subscribe

* Status: proposed
* Date: 2026-11-11

Technical Story: Gestión de suscripciones a categorías de noticias mediante Publish-Subscribe

## Context and Problem Statement

El sistema debe permitir que los lectores se suscriban y se den de baja de una o varias categorías de noticias de forma anónima. Entre las categorías disponibles se encuentran política, economía, sociedad, noticias nacionales e internacionales. Es necesario seleccionar una solución de diseño que permita gestionar de forma adecuada la relación entre los usuarios y las categorías de noticias.

## Decision Drivers

* RF-01.1: Suscripción a categorías de noticias.
* RF-01.2: Darse de baja de suscripciones.

## Considered Options

* Alternativa 1: Observer
* Alternativa 2: Publish-Subscribe
* Alternativa 3: Mediator

## Decision Outcome

Chosen option: "Alternativa 2: Publish-Subscribe", because permite modelar de forma natural el mecanismo de suscripción y cancelación requerido por el sistema.

### Positive Consequences

* Se adapta de forma natural al requisito de suscripción y cancelación de categorías de noticias.
* Reduce el acoplamiento entre usuarios y categorías.
* Facilita la escalabilidad cuando aumenta el número de lectores suscritos.
* Permite añadir nuevas categorías sin modificar el resto de componentes relacionados con las suscripciones.

### Negative Consequences

* Introduce una capa adicional para gestionar las suscripciones.
* El seguimiento del flujo de comunicación puede resultar más complejo que en una relación directa entre objetos.
* Puede dificultar la localización de errores cuando existen muchos publicadores y suscriptores.

## Pros and Cons of the Options

### Alternativa 1: Observer

El patrón Observer define una dependencia uno-a-muchos entre objetos, de forma que cuando un objeto cambia de estado, todos los objetos que dependen de él son notificados automáticamente. En este caso, las categorías de noticias actuarían como sujetos observados y los usuarios como observadores asociados a dichas categorías.

* Good, because Proporciona una implementación sencilla de la relación entre categorías y lectores.
* Good, because Permite notificar automáticamente cambios de estado a los usuarios asociados a una categoría.
* Bad, because Resulta menos flexible cuando aumenta el número de usuarios o categorías gestionadas.
* Bad, because Genera una dependencia más directa entre observadores y elementos observados.

### Alternativa 2: Publish-Subscribe

El patrón Publish-Subscribe permite que los publicadores envíen información a los suscriptores sin conocer directamente quiénes son. Las categorías de noticias actuarían como publicadores y los lectores podrían suscribirse o darse de baja de las categorías que deseen.

* Good, because Está específicamente orientado a mecanismos de suscripción y publicación de información.
* Good, because Reduce el acoplamiento entre categorías y lectores.
* Good, because Mejora la escalabilidad del sistema al gestionar un gran número de suscriptores.
* Good, because La suscripción a noticias aparece como uno de los usos típicos del patrón.
* Bad, because Introduce un mecanismo adicional para la gestión de las suscripciones.

### Alternativa 3: Mediator

El patrón Mediator encapsula la comunicación entre objetos mediante un componente intermediario. En este caso, un mediador central gestionaría las altas y bajas de los usuarios en las diferentes categorías de noticias, evitando que usuarios y categorías se comuniquen directamente.

* Good, because Centraliza la comunicación entre usuarios y categorías.
* Good, because Reduce las dependencias directas entre componentes.
* Bad, because Concentra gran parte de la lógica en un único elemento mediador.
* Bad, because Añade complejidad innecesaria para un problema centrado únicamente en la gestión de suscripciones y bajas.
