# Práctica DAS

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![UML](https://img.shields.io/badge/Architecture-UML%202.0-blue)
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

## Punto de entrada

| Contenido | Acceso |
| --- | --- |
| Requisitos funcionales y de dominio | [Requisitos](Requisitos/requisitos.md) |
| Decisiones arquitectónicas | [Decisiones](Decisiones/) |
| Plantilla MADR | [Plantilla de decisión](Decisiones/Plantillas/plantilla-madr.md) |
| Discusiones y revisiones | [Discusiones](Decisiones/Discusiones/) |
| Diagramas UML por iteración | [Arquitecturas](Arquitecturas/) |

---

## 👥 Equipo de Arquitectura Software

* **ASS 1 (Senior Software Architect):** [Paula de la Fuente Ruiz](https://github.com/Paula-2704)
* **ASS 2 (Senior Software Architect):** [Marcos Vidal Castillo](https://github.com/Marcos-VC)
* **ASC 1 (Cognitive Software Architect):** [Francisco García González](https://github.com/Fran-G-G)
* **ASC 2 (Cognitive Software Architect):** [Fernán Rama Hombreiro](https://github.com/fernanrama)
* **ASJ (Junior Software Architect / Modeler):** [Álvaro Fernández Jiménez](https://github.com/minicruck)

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Consulta el archivo `LICENSE` para más detalles.
