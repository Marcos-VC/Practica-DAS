# Selección del estilo arquitectónico principal para el periódico digital

* Status: accepted
* Deciders: Marcos Vidal Castillo,Paula de la Fuente Ruiz
* Date: 2026-11-07

Technical Story: Semana 1- Selección del estilo arquitectónico principal para el periódico digital

## Context and Problem Statement

El sistema del periódico digital requiere definir un estilo arquitectónico global para estructurar el back-end y la interacción con los clientes. El sistema debe gestionar peticiones HTTP/REST desde ordenadores y Tablets, ejecutar la lógica de negocio relativa a contenidos, suscripciones y métricas, y coordinar la persistencia en dos bases de datos PostgreSQL 19 independientes. Se necesita seleccionar una estructura que mantenga una clara separación de responsabilidades y permita un modelado directo en el Diagrama de Paquetes UML para la primera iteración.

## Decision Drivers

* PD-01 (RD-01): Integración de la pasarela de pago Stripe como proveedor externo de servicios de pago.
* PD-01 (RD-02): Organización de la persistencia backend en dos bases de datos PostgreSQL 19 independientes (administradores/redactores y suscriptores).
* PD-01 (RD-03): Definición del protocolo de comunicación HTTP/REST entre el cliente web y los servicios backend.


## Considered Options

* Alternativa 1: Arquitectura por capas
* Alternativa 2: Arquitectura Basada en Eventos
* Alternativa 3: Arquitectura Cliente-Servidor


## Decision Outcome

Chosen option: "Alternativa 3: Arquitectura Cliente-Servidor", because se adapta de forma natural al requisito de comunicación mediante HTTP/REST entre cliente y servidor (RD-03), centraliza la lógica de negocio facilitando el mantenimiento, permite ofrecer una interfaz única accesible tanto desde ordenadores como Tablets, simplifica la integración con la pasarela de pago Stripe (RD-01) y responde a la estructura de persistencia bifuncional sobre PostgreSQL 19 (RD-02) sin introducir la sobreingeniería de las alternativas 1 y 2.

### Positive Consequences

* Se adapta de forma natural al requisito de comunicación mediante HTTP/REST entre cliente y servidor.
* Centraliza la lógica de negocio, facilitando el mantenimiento.
* Permite ofrecer una interfaz única accesible tanto desde ordenadores como Tablet.
* Simplifica la integración con servicios externos.


### Negative Consequences

* El servidor constituye un punto central de fallo, pudiendo afectar al funcionamiento del sistema.
* A medida que aumenta el número de usuarios, puede ser necesario aumentar los recursos del servidor.


## Pros and Cons of the Options

### Alternativa 1: Arquitectura por capas

Esta alternativa organiza el sistema en tres capas con responsabilidades delimitadas de forma estricta: 
   
   Capa de Presentación: Capa encargada de atender peticiones HTTP/REST y ejecutar la lógica de discriminación y adaptación de la interfaz según el cliente (PC o Tablet). 
   Capa de Lógica de Negocio: Contiene los servicios principales del periódico digital (gestión de noticias, suscripciones, planes de cobros y cálculo de métricas de uso). 
   Capa de Acceso a Datos: Guarda los componentes para gestionar la persistencia hacia las dos bases de datos PostgreSQL 19 independientes.

* Good, because Permite una separación limpia de responsabilidades
* Good, because Separa la lógica de adaptación del Front-end responsive HTTP/REST en la capa de presentación de la lógica de negocio o la capa de datos
* Good, because Mantiene una complejidad baja en la primera iteración del proyecto.
* Bad, because En caso de que la lectura de noticias sea demasiada, la capa de la lógica de negocio centralizada puede convertirse en un cuello de botella, pudiendo ralentizar en ese momento el procesamiento de otros módulos

### Alternativa 2: Arquitectura Basada en Eventos

Esta alternativa organiza el sistema en torno a un canal de eventos central. La publicación de noticias, las solicitudes de suscripciones/bajas y la generación de métricas de uso se gestionan mediante la emisión y el consumo de eventos asíncronos.

* Good, because Desacoplamiento total entre redactores y lectores, cumpliendo el anonimato en las suscripciones al no requerir mantener conexiones síncronas ni datos personales en el servidor.
* Good, because Tolera los picos de tráfico masivos de publicaciones sin saturar las peticiones de facturación en la pasarela Stripe o el acceso de redactores.
* Bad, because Introduce una sobrecarga de infraestructura, mayor complejidad en la trazabilidad y depuración, y una complicación adicional innecesaria para las operaciones síncronas simples de consulta y filtrado de noticias desde el cliente HTTP/REST.


### Alternativa 3: Arquitectura Cliente-Servidor

Esta alternativa organiza el sistema mediante una separación entre los clientes que consumen los servicios y un servidor central encargado de procesar las peticiones y gestionar el acceso a datos. Los usuarios acceden al periódico digital a través de una interfaz web responsive que se comunica mediante HTTP/REST con un servidor encargado de la gestión de las suscripciones, pagos y estadísticas.

* Good, because se adapta de forma natural al requisito de comunicación mediante HTTP/REST entre cliente y servidor.
* Good, because centraliza la lógica de negocio, facilitando el mantenimiento.
* Good, because permite ofrecer una interfaz única accesible tanto desde ordenadores como Tablet.
* Good, because simplifica la integración con servicios externos.
* Bad, because el servidor constituye un punto central de fallo, pudiendo afectar al funcionamiento del sistema.
* Bad, because a medida que aumenta el número de usuarios, puede ser necesario aumentar los recursos del servidor.
