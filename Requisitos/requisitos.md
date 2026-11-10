# Requisitos del sistema

Estado: pendiente de revisión el antes del miercoles 11.

### 4.1. Requisitos funcionales 

| ID | Requisito | Descripción |
| --- | --- | --- |
| RF-01.1 | Suscripción a categorías de noticias | El sistema debe permitir a los lectores suscribirse de manera anónima a una o varias categorías de noticias (política, economía, sociedad, nacionales o internacionales). |
| RF-01.2 | Darse de baja de suscripciones | El sistema debe permitir que los usuarios puedan darse de baja de las suscripciones. |
| RF-02.1 | Gestión de modalidades de suscripción | El sistema debe gestionar los ciclos de facturación con tres modalidades. |
| RF-02.2 | Gestión del pago | El sistema debe permitir al usuario realizar el pago de la suscripción seleccionada y reintentarlo un máximo de 3 veces. |
| RF-03 | Módulo de estadísticas de uso | El sistema debe registrar métricas de uso para redactores y administradores (noticias más leídas, tiempo de permanencia, tasas de rebote y conversión de lectores a suscriptores). |
| RF-03.1 | Cálculo tasa de rebote | El sistema calculará las tasas de rebote con la fórmula correspondiente. |

---

### 4.2. Requisitos de dominio 

| ID | Requisito | Descripción |
| --- | --- | --- |
| RD-01 | Pasarela de pago | Todos los pagos deben procesarse a través de Stripe como proveedor externo de servicios de pago. |
| RD-02 | Almacenamiento de datos | La parte de back-end incluye una base de datos PostgreSQL 19 para los administradores/redactores y otra para los suscriptores. |
| RD-03 | Protocolo de comunicación | La comunicación entre el cliente web y los servicios backend se realiza mediante HTTP/REST. |

---

### 4.3. Problemas de diseño identificados 

| ID | Problema de diseño | Requisitos relacionados |
| --- | --- | --- |
| PD-01 | Definición del estilo arquitectónico que servirá como base para la organización del sistema. | RD-01, RD-02, RD-03 |
| PD-02 | Gestión de las suscripciones y cancelaciones de los usuarios a categorías de noticias. | RF-01.1, RF-01.2 |
| PD-03 | Implementación de las modalidades de suscripción y su ciclo de facturación mediante Stripe. | RF-02.1, RF-02.2 |
| PD-04 | Cálculo y almacenamiento de estadísticas de uso del sistema. | RF-03, RF-03.1 |

## Supuestos y consultas pendientes

| Fecha | Cuestión | Respuesta y fuente | Efecto en los requisitos |
| --- | --- | --- | --- |
| 2026-11-07 | Aclarar el alcance del anonimato de la suscripción y su relación con el registro y la autenticación añadidos al caso. Identificar estos requisitos como supuestos si corresponde. | Pendiente de revisión en la clase del 10/11. | RF-01.1, RF-05.1, RF-05.2 |
| 2026-11-07 | El anexo permite reintentar el pago hasta tres veces; aclarar si los reintentos deben ser automáticos o si esto es un supuesto adicional. | Pendiente de revisión en la clase del 10/11. | RD-05 |
| 2026-11-10 | ¿Los problemas de diseño van en el directorio de requisitos? ¿O el de decisiones?. | RD-05 |
