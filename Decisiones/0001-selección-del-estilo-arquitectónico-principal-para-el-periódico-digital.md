# Selección del estilo arquitectónico principal para el periódico digital

* Status: accepted
* Deciders: Marcos Vidal Castillo,Paula de la Fuente Ruiz
* Date: 2026-11-07

Technical Story: Semana 1- Selección del estilo arquitectónico principal para el periódico digital

## Context and Problem Statement

El sistema del periódico digital requiere definir un estilo arquitectónico global para estructurar el back-end y la interacción con los clientes. El sistema debe gestionar peticiones HTTP/REST desde ordenadores y Tablets, ejecutar la lógica de negocio relativa a contenidos, suscripciones y métricas, y coordinar la persistencia en dos bases de datos PostgreSQL 19 independientes. Se necesita seleccionar una estructura que mantenga una clara separación de responsabilidades y permita un modelado directo en el Diagrama de Paquetes UML para la primera iteración.

## Decision Drivers

* PD-01: Definición del estilo arquitectónico que servirá como base para la organización del sistema.
* PD-13: Organización del almacenamiento y acceso a la información persistente del sistema.
* PD-14: Diseño de la comunicación entre cliente y servidor mediante servicios REST.
* PD-15: Adaptación de la interfaz a distintos dispositivos y tamaños de pantalla.

## Considered Options

* Alternativa 1: Alternativa multicapa
* Alternativa 2: Alternativa Basada en Eventos
* Alternativa 3: Alternativa Microservicios

## Decision Outcome

Chosen option: "Alternativa 1: Alternativa multicapa", because permite una separación limpia de las responsabilidades en tres capas, aislando la discriminación de clientes en la capa de presentación y facilita el acceso directo a las dos bases de datos PostgreQSL 19 sin la sobrecarga ni complejidad de las alternativas 2 y 3.

### Positive Consequences

* Permite una separación limpia de responsabilidades
* Separa la lógica de adaptación del Front-end responsive HTTP/REST en la capa de presentación de la lógica de negocio o la capa de datos
* Mantiene una complejidad baja en la primera iteración del proyecto.

### Negative Consequences

* En caso de que la lectura de noticias sea demasiada, la capa de la lógica de negocio centralizada puede convertirse en un cuello de botella, pudiendo ralentizar en ese momento el procesamiento de otros módulos

## Pros and Cons of the Options

### Alternativa 1: Alternativa multicapa

Esta alternativa organiza el sistema en tres capas con responsabilidades delimitadas de forma estricta: 
   
   Capa de Presentación: Capa encargada de atender peticiones HTTP/REST y ejecutar la lógica de discriminación y adaptación de la interfaz según el cliente (PC o Tablet). 

   Capa de Lógica de Negocio: Contiene los servicios principales del periódico digital (gestión de noticias, suscripciones, planes de cobros y cálculo de métricas de uso). 

   Capa de Acceso a Datos: Guarda los componentes para gestionar la persistencia hacia las dos bases de datos PostgreSQL 19 independientes.

* Good, because Permite una separación limpia de responsabilidades
* Good, because Separa la lógica de adaptación del Front-end responsive HTTP/REST en la capa de presentación de la lógica de negocio o la capa de datos
* Good, because Mantiene una complejidad baja en la primera iteración del proyecto.
* Bad, because En caso de que la lectura de noticias sea demasiada, la capa de la lógica de negocio centralizada puede convertirse en un cuello de botella, pudiendo ralentizar en ese momento el procesamiento de otros módulos

### Alternativa 2: Alternativa Basada en Eventos

Esta alternativa organiza el sistema en torno a un canal de eventos central. La publicación de noticias, las solicitudes de suscripciones/bajas y la generación de métricas de uso se gestionan mediante la emisión y el consumo de eventos asíncronos.

* Good, because Desacoplamiento total entre redactores y lectores, cumpliendo el anonimato en las suscripciones al no requerir mantener conexiones síncronas ni datos personales en el servidor.
* Good, because Tolera los picos de tráfico masivos de publicaciones sin saturar las peticiones de facturación en la pasarela Stripe o el acceso de redactores.
* Bad, because Introduce una sobrecarga de infraestructura, mayor complejidad en la trazabilidad y depuración, y una complicación adicional innecesaria para las operaciones síncronas simples de consulta y filtrado de noticias desde el cliente HTTP/REST.

### Alternativa 3: Alternativa Microservicios

Esta alternativa descompone el sistema en un conjunto de servicios autónomos e independientes tras un API Gateway (Servicio de Contenidos, Servicio de Suscripciones y Pagos Stripe, y Servicio de Métricas de Uso), donde cada microservicio gestiona su propio almacenamiento de datos.

* Good, because Permite escalar e independizar el despliegue del servicio de lectura de noticias en momentos de alta concurrencia, sin necesidad de escalar los módulos de administración o pagos.
* Bad, because Añade una elevada complejidad en la gestión de transacciones, orquestación de servicios y latencia de red en las peticiones HTTP/REST habituales.
* Bad, because Duplica la sobrecarga de infraestructura y despliegue al dividir la persistencia de datos en múltiples microservicios, en lugar de aprovechar de forma directa la estructura de persistencia bifuncional requerida sobre las dos bases de datos PostgreSQL 19.
