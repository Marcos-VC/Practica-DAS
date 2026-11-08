# Requisitos del sistema

Estado: pendiente de revisión el martes 10.

## Requisitos funcionales

| ID | Nombre | Descripción detallada | Procedencia y referencia | Decisión asociada |
| --- | --- | --- | --- | --- |
| RF-01.1 | Suscripción a categorías de noticias | El sistema debe permitir a los lectores suscribirse de manera anónima a una o varias categorías de noticias, dotando de los beneficios correspondientes al suscriptor. | Anexo I (Sección Suscripciones) | Pendiente (Semana 2) |
| RF-01.2 | Darse de baja de suscripciones | El sistema debe permitir que los usuarios puedan darse de baja de las suscripciones. | Anexo I (Sección Suscripciones) | Pendiente (Semana 2) |
| RF-02.1 | Gestión de modalidades de suscripción | El sistema debe permitir la elección de la tarifa de suscripción al usuario (Trimestral, Semestral o Anual). | Anexo I (Sección Tarifas) | Pendiente (Semana 2) |
| RF-02.2 | Gestión del pago | El sistema debe permitir al usuario realizar el pago de la suscripción seleccionada mediante la pasarela de pago. | Anexo I (Sección Cobros) | Pendiente (Semana 2) |
| RF-03 | Módulo de estadísticas de uso | El sistema debe registrar y calcular métricas de uso para redactores y administradores. | Anexo I (Sección Estadísticas) | Pendiente (Semana 3) |
| RF-04 | Acceder a noticias | El sistema debe permitir que el usuario acceda a una serie de noticias, que pueden estar filtradas por categorías temáticas. | Anexo I (Sección Lectura) | ADR-0001 |
| RF-05.1 | Registro de usuario | El sistema debe permitir a un lector crear una cuenta proporcionando la información necesaria para la gestión de su suscripción. | Anexo I (Sección Usuarios) | Pendiente (Semana 2) |
| RF-05.2 | Autenticación de usuario | El sistema debe permitir al usuario acceder a su cuenta mediante credenciales válidas. | Anexo I (Sección Usuarios) | Pendiente (Semana 2) |
| RF-06 | Consultar estado de suscripción | El sistema debe permitir al suscriptor conocer la modalidad de suscripción activa y su fecha de vencimiento. | Anexo I (Sección Suscripciones) | Pendiente (Semana 2) |
| RF-07 | Gestionar noticias | El sistema debe permitir que los redactores puedan crear, modificar y eliminar noticias. | Anexo I (Sección Redacción) | Pendiente (Semana 2) |
| RF-08 | Clasificar noticias | El sistema debe permitir asociar noticias a una o varias categorías temáticas. | Anexo I (Sección Categorización) | Pendiente (Semana 2) |

## Requisitos de dominio

| ID | Nombre | Descripción detallada | Procedencia y referencia | Decisión asociada |
| --- | --- | --- | --- | --- |
| RD-01 | Categorías de noticias | Las noticias se organizan estrictamente en las categorías política, economía, sociedad, nacional e internacional. | Anexo I (Especificación de dominio) | Pendiente (Semana 2) |
| RD-02 | Modalidades de suscripción | El sistema dispone de tres modalidades de suscripción: trimestral, semestral y anual. | Anexo I (Especificación de tarifas) | Pendiente (Semana 2) |
| RD-03 | Tarifas de suscripción | Las modalidades de suscripción tienen un coste fijo de 70 €, 130 € y 190 € respectivamente. | Anexo I (Especificación de tarifas) | Pendiente (Semana 2) |
| RD-04 | Pasarela de pago | Todos los pagos deben procesarse obligatoriamente a través de Stripe como proveedor externo de servicios de pago. | Anexo I (Integración externa) | Pendiente (Semana 2) |
| RD-05 | Reintentos de pago | Ante un fallo en una operación de cobro, el sistema debe realizar un máximo de tres reintentos automáticos. | Anexo I (Tolerancia a fallos) | Pendiente (Semana 2) |
| RD-06 | Roles del sistema | Existen tres tipos principales de actores en el sistema: suscriptores, redactores y administradores. | Anexo I (Modelo de actores) | Pendiente (Semana 2) |
| RD-07 | Almacenamiento de datos | La parte de back-end incluye una base de datos PostgreSQL 19 para administradores/redactores y otra independiente para suscriptores. | Anexo I (Arquitectura de persistencia) | ADR-0001 |
| RD-08 | Acceso multidispositivo | La aplicación debe poder ser utilizada desde ordenadores y Tablets mediante una interfaz responsive adaptada. | Anexo I (Clientes soportados) | ADR-0001 |
| RD-09 | Protocolo de comunicación | La comunicación entre el cliente web y los servicios backend se realiza exclusivamente mediante HTTP/REST. | Anexo I (Protocolo de red) | ADR-0001 |
| RD-10 | Métricas de negocio | El sistema debe gestionar métricas relacionadas con noticias más leídas, tiempo de permanencia, tasa de rebote y conversión de lectores a suscriptores. | Anexo I (Módulo estadístico) | Pendiente (Semana 3) |

## Supuestos y consultas pendientes

| Fecha | Cuestión | Respuesta y fuente | Efecto en los requisitos |
| --- | --- | --- | --- |
| 2026-11-07 | Aclarar el alcance del anonimato de la suscripción y su relación con el registro y la autenticación añadidos al caso. Identificar estos requisitos como supuestos si corresponde. | Pendiente de revisión en la clase del 10/11. | RF-01.1, RF-05.1, RF-05.2 |
| 2026-11-07 | El anexo permite reintentar el pago hasta tres veces; aclarar si los reintentos deben ser automáticos o si esto es un supuesto adicional. | Pendiente de revisión en la clase del 10/11. | RD-05 |
