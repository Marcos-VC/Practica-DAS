# Cálculo y almacenamiento de estadísticas de uso del sistema

* Status: accepted
* Deciders: Marcos Vidal Castillo,Paula de la Fuente Ruiz
* Date: 2026-11-16


Technical Story: Semana 3- Cálculo y almacenamiento de estadísticas de uso del sistema

## Context and Problem Statement

El periódico digital debe proporcionar a los administradores y redactores un módulo de análisis para consultar métricas clave sobre la actividad de la plataforma: las noticias más leídas, el tiempo de permanencia de los usuarios, la conversión de lectores a suscriptores y las tasas de rebote (calculadas mediante la fórmula (Visitas-Conversión) /Visitas *100).

## Decision Drivers

* PD-04 (RF-03): Registro de métricas de uso para redactores y administradores (noticias más leídas, tiempo de permanencia, tasas de rebote y conversión de lectores a suscriptores).
* PD-04 (RF-03.1): Cálculo de las tasas de rebote aplicando la fórmula (Visitas-Conversión) /Visitas *100.


## Considered Options

* Alternativa 1: Patrón Decorator sobre los Controladores REST
* Alternativa 2: Patrón Observer asíncrono con procesamiento por lotes



## Decision Outcome

Chosen option: "Alternativa 2: Patrón Observer asíncrono con procesamiento por lotes", because responde a la necesidad de registrar las interacciones de los lectores (noticias más leídas, tiempo de permanencia, conversión a suscriptores y cálculo de la tasa de rebote mediante la fórmula) sin penalizar los tiempos de respuesta del servicio principal de lectura de noticias. Al desacoplar la captura de eventos mediante observadores en memoria y delegar el cálculo matemático y el volcado de datos a un proceso planificado periódico, se evita ejecutar escrituras síncronas en cada petición HTTP/REST que saturarían las conexiones.

### Positive Consequences

* Evita penalizar la latencia de las peticiones HTTP/REST de los lectores, ya que el cálculo de métricas y la escritura en la base de datos de administradores se realizan fuera del hilo de respuesta al usuario.
* Permite agrupar las escrituras en lotes periódicos, reduciendo la carga de conexiones y bloqueos ante picos de lectura.
* Centraliza la lógica de cálculo en un único componente agregador, facilitando añadir nuevas métricas en el futuro sin modificar los controladores de noticias o pagos.



### Negative Consequences

* Requiere gestionar una cola o búfer temporal en memoria en el servidor y controlar posibles pérdidas de métricas no volcadas si el servidor se reinicia inesperadamente.



## Pros and Cons of the Options

### Alternativa 1: Patrón Decorator sobre los Controladores REST

Esta alternativa envuelve los servicios principales de gestión de noticias y suscripciones mediante componentes Decorator. Cada vez que un lector realiza una lectura o se convierte en suscriptor, el decorador intercepta la petición HTTP/REST, actualiza inmediatamente los contadores de visitas, tiempo de permanencia o conversión, recalcula la tasa de rebote y ejecuta una sentencia Update.

* Good, because Garantiza que los administradores y redactores dispongan de estadísticas actualizadas en tiempo real absoluto tras cada interacción de un lector.
* Good, because No requiere procesos en segundo plano ni tareas programadas, manteniendo un flujo de ejecución lineal dentro de la arquitectura Cliente-Servidor.
* Bad, because Introduce una escritura síncrona adicional en la base de datos PostgreSQL de administradores en cada lectura de noticia, degradando el rendimiento y aumentando el tiempo de respuesta percibido por los lectores en PC y Tablet.
* Bad, because Un fallo o bloqueo en la base de datos de estadísticas de administradores podría provocar errores o retrasos en la visualización de noticias para los lectores, acoplando la disponibilidad de ambos subsistemas.


### Alternativa 2: Patrón Observer asíncrono con procesamiento por lotes

Esta alternativa desacopla la captura de métricas del flujo principal de lectura de noticias mediante un mecanismo Observer interno en el servidor. Cada vez que un lector consulta una noticia, abandona una página o completa una suscripción, se registra un evento ligero en memoria. Un proceso planificado periódico consolida estos datos, aplica las fórmulas de negocio (como el cálculo de la tasa de rebote y la conversión).

* Good, because Evita penalizar la latencia de las peticiones HTTP/REST de los lectores, ya que el cálculo de métricas y la escritura en la base de datos de administradores se realizan fuera del hilo de respuesta al usuario.
* Good, because Permite agrupar las escrituras en lotes periódicos, reduciendo la carga de conexiones y bloqueos ante picos de lectura.
* Good, because Centraliza la lógica de cálculo en un único componente agregador, facilitando añadir nuevas métricas en el futuro sin modificar los controladores de noticias o pagos.
* Bad, because Requiere gestionar una cola o búfer temporal en memoria en el servidor y controlar posibles pérdidas de métricas no volcadas si el servidor se reinicia inesperadamente.