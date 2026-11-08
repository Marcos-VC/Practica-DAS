# Revisión de la selección del estilo arquitectónico

- Semana: 1.
- Decisión revisada: [Decisión 1, selección del estilo arquitectónico](../0001-selección-del-estilo-arquitectónico-principal-para-el-periódico-digital.md).
- Estado: cuestiones propuestas por Fernán (ASC) y resueltas por los ASS.

## Cuestiones planteadas y respuestas de los ASS 

1. **Justificación de la elección.** ¿Por qué la arquitectura en capas es mejor para este periódico que las otras alternativas? Habéis puesto «because comes out best», pero deberíais desarrollar la comparación para este caso y para estos requisitos.
   - El texto “comes out best” era un texto por defecto de la plantilla,
que ya hemos sustituido en la versión final. La arquitectura en capas se selecciona porque
encaja mejor con las necesidades del proyecto y con los requisitos del enunciado:
●​ Permite aislar en la Capa de Presentación la discriminación de clientes HTTP/REST
(PC y Tablet, según los RD-08 y RD-09), asegurandose de que la lógica de
adaptación de la interfaz no intervenga en el negocio.
●​ Permite a la Capa de Acceso a Datos gestionar de manera directa las bases de
datos PostgreSQL 19 independientes (RD-07), manteniendo la persistencia para
redactores/administradores por un lado y suscriptores por el otro.
●​ Frente a Microservicios, evita la sobrecarga de infraestructura y la complejidad de
coordinar transacciones en su fase inicial.
●​ Frente a una arquitectura Basada en Eventos, evita añadir acoplamiento asíncrono
innecesario en las peticiones síncronas de lectura y filtrado de noticias (RF-04).
2. **Responsabilidades y representación UML.** ¿Qué responsabilidades tiene cada capa y qué dependencias puede haber entre ellas? ¿Cómo se van a representar las relaciones con Stripe y las dos bases de datos? Deberíais aclararlo para que los ASJ puedan hacer el UML a partir de esa decisión.
   - Las responsabilidades de cada capa se han respondido en la
primera pregunta. Los ASJ en el Diagrama de Paquetes UML pueden relacionar y
comunicar las capas con los imports y paquetes externos que sean estrictamente
necesarios, sin sobrecargar capas con funciones de otra capa.
3. **Fundamento de ventajas e inconvenientes.** ¿Por qué decís que los eventos garantizan anonimato y que los microservicios "duplican la sobrecarga de infraestructura"? Creo que deberíais pulir esos argumentos y explicar un poco sus condiciones.
   - Anonimato en la Arquitectura de Eventos: Funciona como un canal de avisos o una radio. El
redactor publica la noticia en un tema y el sistema simplemente manda el aviso a los que
estén escuchando esa categoría, sin necesidad de saber quiénes son, cómo se llaman ni
tener su usuario ni datos personales. Por eso, usar este mecanismo dentro de la capa de
negocio permite enviar las noticias manteniendo el anonimato que pide el enunciado.
Sobrecarga en Microservicios: Los microservicios "duplican la sobrecarga de
infraestructura" en este contexto porque el enunciado exige únicamente dos bases de datos.
Descomponer el sistema en microservicios independientes obliga a dividir el
almacenamiento en múltiples contenedores, desplegar un API Gateway y gestionar
transacciones distribuidas y latencia de red.
4. **Trazabilidad de requisitos.** La tabla asocia los requisitos RF-04 y RD-07, RD-08 y RD-09 con ADR-0001. ¿Cómo vais a delimitar las decisiones para asegurar que no se usa un mismo requisito en dos decisiones?
   - Para cumplir con el criterio de no duplicar requisitos entre
decisiones:
●​ Asignación en la Semana 1: RF-04 y RD-07, RD-08 y RD-09 han sido vinculados de
manera directa a la decisión principal 0001 mediante el problema de diseño PD-01 (y
sus derivados PD-14, PD-15,PD-16), ya que estos requisitos dictan las restricciones
estructurales del sistema global.
●​ Delimitación para decisiones futuras: Los requisitos asociados a decisiones
Pendientes en la tabla Requisitos.md (como la pasarela Stripe, el módulo de
métricas o el ciclo de autenticación) quedarán reservados para las decisiones de la
Semana 2 y Semana 3. Cada decisión futura abordará únicamente su set de
Requisitos y Problemas de Diseño (PD-02 a PD-13), impidiendo cualquier
solapamiento.

## Respuestas y resultado

Las aclaraciones se han hablado con el resto del equipo y se han tenido en cuenta para el UML.

## Referencias

- [Tabla de requisitos y consultas pendientes](../../Requisitos/requisitos.md).
