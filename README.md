# Practica-DAS

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![UML](https://img.shields.io/badge/Architecture-UML%202.0-blue)](https://madr.github.io/)
[![ADR Status](https://img.shields.io/badge/ADR-MADR%20Format-green)](https://adr.github.io/madr/)

> Plataforma web responsiva para periódicos digitales con sistema de suscripciones anónimas, pasarela de pago recurrente y analítica para redactores.

---

## 📌 Visión General

Este repositorio contiene la especificación de requisitos, las decisiones de diseño de arquitectura (ADRs) y los modelos UML para un **Periódico Digital** multi-dispositivo.

El sistema permite a los lectores consumir noticias y suscribirse de forma anónima a canales temáticos, gestionar planes de pago recurrentes integrados con Stripe y proporcionar a los administradores métricas clave de uso y conversión.

---

## Roadmap
```mermaid
timeline
    title Planificación 

    Antes del 2 de Noviembre : Comprender instrucciones
                            : Acordar cómo trabajar
                            : Preparar repositorio y documentación

    Semana 1 (2 al 7 Nov) : Analizar requisitos
                          : Tomar primeras decisiones de estilo
                          : Producir UML inicial

    9 y 10 de Noviembre : Revisar requisitos y estilo antes de fijar

    Resto semana 2 (hasta 14 Nov) : Incorporar nuevas decisiones
                                  : Refinar clases y paquetes

    Semana 3 (16 al 19 Nov) : Completar el diseño
                            : Cerrar contenido del repositorio

    20 al 22 de Noviembre : Terminar y revisar memoria
                          : Preparar paquete de entrega

    23 de Noviembre : Entrega del comprimido

```

---

```mermaid
gantt
    dateFormat  YYYY-MM-DD
    title  

    section Fase 1: Preparación
    Comprender instrucciones, acordar metodología y preparar repo y doc  :a1, 2026-10-26, 2026-11-02

    section Fase 2: Análisis y UML
    Analizar requisitos, estilo y UML inicial                                :a2, 2026-11-02, 2026-11-08
    Revisar requisitos y estilo                                              :a3, 2026-11-09, 2026-11-11

    section Fase 3: Diseño y Refino
    Incorporar decisiones y refinar clases/paquetes                         :a4, 2026-11-11, 2026-11-15
    Completar diseño y cerrar contenido del repo                           :a5, 2026-11-16, 2026-11-20

    section Fase 4: Entrega
    Terminar y revisar memoria y paquete de entrega                         :a6, 2026-11-20, 2026-11-23
    Entrega del comprimido                                                   :milestone, m1, 2026-11-23, 0d
```

---

## 📄 Documentación Completa

Puedes consultar el informe técnico completo y la memoria del proyecto en el siguiente enlace:
* 📥 [Descargar Memoria de la Práctica (PDF)](./Documentacion/Memoria_DAS_P1.pdf)

---

## 🛠️ Estructura del Repositorio

* **Requisitos/** - Especificación detallada de requisitos funcionales (RF) y de dominio
* **Decisiones/** - Registros de Decisiones Arquitectónicas en formato MADR
* **Arquitecturas/** - Diagramas UML de clases y paquetes por iteración (SVG / PNG)

---

## 🏗️ Estilo y Decisiones Arquitectónicas

La arquitectura del sistema responde a los requisitos de desacoplamiento, adaptabilidad responsive y aislamiento de datos:

* **Front-end Web Responsivo:** Adaptación dinámica según dispositivo (PC, Tablet) mediante APIs HTTP/REST.
* **Persistencia Segregada:** Uso de bases de datos PostgreSQL independientes para el dominio administrativo/redacción y el dominio de suscriptores.
* **Integración de Pagos:** Módulo de pagos recurrentes con Stripe y lógica de reintentos (hasta 3 intentos) ante fallos de transacción.
* **Patrones y Estilos:** Capturados progresivamente en la carpeta `Decisiones/` usando la notación MADR.

---

## 📋 Módulos y Requisitos Principales

* **RF1 - Gestión de Contenidos y Lectura:** Discriminación de clientes (PC/Tablet), navegación HTTP/REST por categorías (Política, Economía, Sociedad, Nacional, Internacional).
* **RF2 - Canal de Suscripciones Anónimas:** Alta y baja de lectores en canales temáticos preservando la privacidad.
* **RF3 - Pasarela y Facturación (Stripe):** Gestión de suscripciones Trimestral (70€), Semestral (130€) y Anual (190€), con reintentos automáticos de pago.
* **RF4 - Módulo de Estadísticas y Analítica:** Panel para redactores con noticias más leídas, tiempo de permanencia, tasa de rebote y tasa de conversión.
* **RF5 - Gestión de Persistencia Dual:** Almacenamiento segregado en PostgreSQL para datos de suscriptores y gestión editorial.

---

## 👥 Equipo de Arquitectura Software

* **ASS 1 (Senior Software Architect):** [Paula de la Fuente Ruiz] (https://github.com/Paula-2704)
* **ASS 2 (Senior Software Architect):** [Marcos Vidal Castillo] (https://github.com/Marcos-VC)
* **ASC 1 (Cognitive Software Architect):** [Francisco García González] (https://github.com/Fran-G-G)
* **ASC 2 (Cognitive Software Architect):** [Fernán Rama Hombreiro] (https://github.com/fernanrama)
* **ASJ (Junior Software Architect / Modeler):** [Álvaro Fernández Jiménez] (https://github.com/minicruck)

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Consulta el archivo `LICENSE` para más detalles.
