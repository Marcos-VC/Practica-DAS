# Revisión de la selección del estilo arquitectónico

- Propuesta inicial: semana 1, arquitectura por capas.
- Revisión preparada: 10/11/2026.
- Decisión revisada: [Decisión 1: Selección del estilo arquitectónico](../0001-selección-del-estilo-arquitectónico-principal-para-el-periódico-digital.md).
- Estado: Cuestiones preparadas por Fernán (ASC), pendientes de respuesta de los ASS y de una nueva propuesta de decisión.

## Contexto de la revisión

La propuesta inicial de los ASS selecciona una arquitectura por capas para organizar la presentación, la lógica de negocio y el acceso a datos. Tras revisar la propuesta, se plantean las cuestiones siguientes sobre su adecuación y su representación del caso.

Según la indicación del profesor comunicada por Fernán, el estilo que corresponde al sistema es cliente-servidor. Se solicita a los ASS que valoren esta indicación y elaboren una nueva propuesta de decisión.

## Cuestiones planteadas por ASC

1. **Relación entre los participantes.** La propuesta por capas separa responsabilidades, pero ¿deja suficientemente explícito que el periódico tiene clientes web que realizan peticiones y un servidor que las atiende? ¿Cómo se representa en la decisión la comunicación HTTP/REST exigida por RD-03?
2. **Adecuación al caso y al criterio docente.** Si el profesor ha indicado cliente-servidor como estilo para el sistema, ¿por qué la propuesta inicial selecciona capas como estilo principal? ¿Qué elementos del caso justifican cambiar la decisión a cliente-servidor y cómo se compararía esta propuesta con las alternativas consideradas?
3. **Organización interna.** Cliente-servidor describe la relación entre participantes y capas organizadas de responsabilidades. ¿Es necesario abandonar toda organización por capas o podrían mantenerse separaciones internas de responsabilidades dentro de la solución cliente-servidor? ¿Cómo se delimitarían ambas ideas en la nueva propuesta?
4. **Componentes y UML.** ¿Qué responsabilidades corresponden al cliente y cuáles al servidor? ¿Cómo se relaciona el servidor con Stripe y con las dos bases de datos? ¿Qué elementos y relaciones deben representar Álvaro y Alejandro en los diagramas de clases y paquetes?
5. **Requisitos y decisiones posteriores.** PD-01 referencia RD-01, RD-02 y RD-03 en la tabla actual. ¿Cómo se justifica la relación de cada requisito con la decisión de estilo y cómo se delimita esta frente a la decisión posterior de pagos para respetar la regla de no utilizar un mismo requisito en dos decisiones?

## Respuestas y resultado

Agradecemos vuestra revisión y las observaciones planteadas. Tras analizar vuestros comentarios y la directriz del profesor, estamos totalmente de acuerdo en que debemos reorientar la decisión principal hacia un estilo cliente-servidor, manteniendo una organización interna por capas para la gestión de las responsabilidades del servidor.

A continuación, damos respuesta a cada una de las 5 cuestiones planteadas, para incorporar este cambio en la nueva propuesta de decisión arquitectónica:

1. **Relación entre los participantes.** Sí, queda totalmente explícito. El enfoque cliente-servidor separa claramente al cliente (la interfaz web responsive para ordenadores y tablets) del servidor central que atiende las peticiones. La comunicación obligatoria del RD-03 se resuelve directamente mediante peticiones HTTP/REST entre ambos. Eso sí, somos conscientes de los riesgos: este servidor central es un punto único de fallo (SPOF) y requerirá escalar recursos si crece el tráfico.
2. **Adecuación al caso y al criterio docente.** La propuesta inicial eligió capas por inercia de diseño, confundiendo la estructura interna del código con el estilo arquitectónico global. Al alinearnos con el profesor, adoptamos la Alternativa 3 (Cliente-Servidor) porque encaja de manera natural con el proyecto:
    - Soporta la comunicación HTTP/REST (RD-03).
    - Se adapta a la interfaz web multidispositivo (PC y tablet).
    - Facilita integrar la pasarela Stripe (RD-01) y conectar con las dos bases de datos PostgreSQL 19 (RD-02).
    - Evita la complejidad innecesaria de un modelo basado en eventos y descarta las capas como estilo principal, manteniendo la lógica de negocio centralizada y fácil de mantener.
3. **Organización interna.** La arquitectura global será puramente cliente-servidor, evitando híbridos confusos. A nivel interno, el servidor centralizará la lógica de negocio para mantener el código ordenado y limpio (gestionando contenidos, estadísticas, Stripe y las bases de datos) sin necesidad de declarar un multicapa externo.
4. **Componentes y UML.** En los diagramas, Álvaro y Alejandro reflejarán la siguiente distribución:
    - Lado cliente: La interfaz web responsive (para PC y tablet), encargada de mostrar las vistas y lanzar las peticiones.
    - Lado servidor: El servidor central que procesa las solicitudes y alberga la lógica de negocio.
    - Relaciones y persistencia:
        - Comunicación síncrona HTTP/REST entre cliente y servidor.
        - Integración de pagos directa con la pasarela externa Stripe (RD-01).
        - Conexión directa a las dos bases de datos PostgreSQL 19 independientes (RD-02): una para redactores/administradores y otra para suscriptores.
    - Diagrama de paquetes: Se separará claramente el paquete del cliente web y el del servidor backend, indicando sus dependencias con las bases de datos y Stripe.
5. **Requisitos y decisiones posteriores.** Para no duplicar requisitos entre decisiones:
    - En la PD-01 (Cliente-Servidor), los requisitos RD-01, RD-02 y RD-03 se usan como contexto general para justificar por qué se elige este modelo frente a otras alternativas.
    - La decisión posterior de pagos se encargará específicamente del diseño detallado, los flujos de transacciones y la implementación de la pasarela Stripe (utilizando RD-01 en su vertiente técnica), evitando solapamientos en la memoria.

## Referencias

- [Decisión inicial sobre el estilo arquitectónico](../0001-selección-del-estilo-arquitectónico-principal-para-el-periódico-digital.md).
- [Requisitos y problemas de diseño](../../Requisitos/requisitos.md).