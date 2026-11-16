# Implementación de las modalidades de suscripción y su ciclo de facturación mediante Stripe

* Status: accepted
* Deciders: Marcos Vidal Castillo,Paula de la Fuente Ruiz
* Date: 2026-11-16


Technical Story: Semana 3- Implementación de las modalidades de suscripción y su ciclo de facturación mediante Stripe

## Context and Problem Statement

El sistema debe gestionar el cobro recurrente de las suscripciones de los lectores ofreciendo tres planes de facturación diferenciados: Trimestral (70€ con descuento base), Semestral (130€ con descuento intermedio) y Anual (190€ con máximo ahorro mensual). Todos los pagos deben procesarse exclusivamente a través de la pasarela externa Stripe y, en caso de que una operación de cobro falle, el sistema debe garantizar hasta un máximo de tres reintentos antes de dar la transacción por fallida.

## Decision Drivers

* PD-03 (RF-02.1): Gestión de los ciclos de facturación con tres modalidades (Trimestral 70€, Semestral 130€ y Anual 190€).
* PD-03 (RF-02.2): Permitir al usuario realizar el pago de la suscripción seleccionada y reintentarlo un máximo de 3 veces.
* PD-03 (RD-01): Procesamiento exclusivo de todos los pagos a través de Stripe como proveedor externo de servicios de pago.



## Considered Options

* Alternativa 1: Combinación de Patrones Adapter + Strategy con gestor de reintentos
* Alternativa 2: Patrón Proxy con bucle de reintentos



## Decision Outcome

Chosen option: "Alternativa 1: Combinación de Patrones Adapter + Strategy con gestor de reintentos", because permite desacoplar la lógica de negocio del periódico digital respecto a la API externa de Stripe mediante una interfaz común, evitando que futuras actualizaciones en la pasarela afecten al núcleo del sistema y facilitando la realización de pruebas sin ejecutar transacciones reales. Además, la aplicación del patrón Strategy permite encapsular de manera limpia y extensible las reglas de facturación y descuentos correspondientes a las tres modalidades exigidas sin recurrir a estructuras condicionales complejas.

### Positive Consequences

* Aísla el SDK de Stripe detrás de una interfaz común, evitando que cambios en la API externa afecten a la lógica de negocio del periódico y facilitando las pruebas sin cobros reales.
* Encapsula el cálculo de tarifas y descuentos de los tres planes (70€, 130€ y 190€) en estrategias independientes, permitiendo añadir nuevas modalidades en el futuro sin modificar el flujo de facturación.
* Permite llevar un control estricto y auditable en la base de datos PostgreSQL 19 de suscriptores del número exacto de reintentos (máximo 3) antes de dar por fallida la operación.



### Negative Consequences

* Incrementa el número de clases en el diagrama de paquetes y exige implementar en el backend la lógica de planificación de reintentos para no bloquear las peticiones HTTP/REST del cliente.



## Pros and Cons of the Options

### Alternativa 1: Combinación de Patrones Adapter + Strategy con gestor de reintentos

Esta alternativa desacopla la comunicación con el servicio externo Stripe mediante una interfaz propia implementada por un StripeAdapter. Para modelar los tres ciclos de facturación y sus respectivos precios y descuentos (70€ trimestral, 130€ semestral y 190€ anual), se utiliza el patrón Strategy (Plan Trimestral, Plan Semestral, Plan Anual). Finalmente, un gestor interno de cobros en el servidor controla de forma explícita el contador de intentos (hasta un máximo de 3 reintentos).

* Good, because Aísla el SDK de Stripe detrás de una interfaz común, evitando que cambios en la API externa afecten a la lógica de negocio del periódico y facilitando las pruebas sin cobros reales.
* Good, because Encapsula el cálculo de tarifas y descuentos de los tres planes (70€, 130€ y 190€) en estrategias independientes, permitiendo añadir nuevas modalidades en el futuro sin modificar el flujo de facturación.
* Good, because Permite llevar un control estricto y auditable en la base de datos PostgreSQL 19 de suscriptores del número exacto de reintentos (máximo 3) antes de dar por fallida la operación.
* Bad, because Incrementa el número de clases en el diagrama de paquetes y exige implementar en el backend la lógica de planificación de reintentos para no bloquear las peticiones HTTP/REST del cliente.


### Alternativa 2: Patrón Proxy con bucle de reintentos

Esta alternativa interpone un componente Proxy entre el controlador REST de suscripciones y el cliente de Stripe. Cuando un usuario solicita suscribirse a una de las tres modalidades (70€, 130€ o 190€), el proxy intercepta la petición, valida los datos de la tarjeta y ejecuta un bucle síncrono de hasta 3 llamadas consecutivas a Stripe en caso de error antes de devolver una respuesta al cliente web.

* Good, because Presenta una estructura muy sencilla de implementar y modelar en UML, manteniendo todo el flujo de cobro dentro de una única petición síncrona HTTP/REST.
* Good, because No requiere colas de tareas diferidas ni procesos en segundo plano en el servidor para gestionar los reintentos.
* Bad, because Ejecutar 3 reintentos de forma síncrona e inmediata bloquea el hilo de la petición HTTP/REST, provocando tiempos de espera elevados o timeouts en el front-end (PC/Tablet) y resultando ineficaz ante rechazos por falta de saldo puntual.
* Bad, because Al no separar la lógica de tarifas mediante estrategias, el proxy tiende a concentrar demasiadas responsabilidades (validación, cálculo de precios de los 3 planes y control de errores).    