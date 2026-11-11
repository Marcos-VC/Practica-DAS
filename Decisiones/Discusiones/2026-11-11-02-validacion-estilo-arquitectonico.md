# Validación de la selección del estilo arquitectónico (Cliente-Servidor)

- Estado: Aceptado (*Validado por Fran - ASC*).
- Decisores: ASS (Autores) y ASC (Revisores).
- Revisión preparada: 11/11/2026.
- Decisión revisada: [Decisión 2: Selección del estilo arquitectónico principal](../0001-selección-del-estilo-arquitectónico-principal-para-el-periódico-digital.md).

## Decisión tomada

Damos la revisión por cerrada y aprobada. Validamos la adopción de la alternativa 3: Arquitectura Cliente-Servidor como el estilo arquitectónico principal del sistema.

Consideramos que las respuestas resuelven bien las dudas planteadas y justifican el cambio según lo pedido por el profesor.

### Valoración de los puntos clave

- **Relación entre participantes**: Queda clara la separación entre el cliente web (PC/Tablet) y el servidor central, así como la comunicación síncrona por HTTP/REST (RD-03).
- **Justificación de alternativas**: Se argumenta de forma directa por qué se descartan las capas como estilo global (Alternativa 1) y los eventos (Alternativa 2). Priorizando la simplicidad y el menor coste de infraestructura para arrancar.
- **Organización interna**: Se resuelve la distinción entre estilo de distribución y orden interno del código. Aceptamos que el servidor centralice la lógica y la persistencia, sin necesidad de complicar la decisión con un modelo híbrido.
- **Impacto en UML**: Esto da luz verde a Álvaro y Alejandro para comenzar a trabajar en los diagramas de paquetes, clases y despliegue, incluyendo las conexiones con Stripe (RD-01) y las dos bases de datos PostgreSQL 19 (RD-02).
- **Trazabilidad de requisitos**: Aprobamos el uso del RD-01, RD-02 y RD-03 en esta decisión solo como contexto global, dejando los detalles y flujos de pago para la decisión específica de Stripe.

## Conclusión y consecuencias

- Estado de la decisión: Pasa a `Accepted` (Cliente-Servidor).
- Próximos pasos: Que Álvaro y Alejandro empiecen a actualizar los diagramas UML con la estructura Cliente-Servidor validada.

## Referencias

- [Decisión final sobre el estilo arquitectónico](../0001-selección-del-estilo-arquitectónico-principal-para-el-periódico-digital.md).
- [Requisitos y problemas de diseño](../../Requisitos/requisitos.md).
- [Revisión de la selección del estilo arquitectónico](2026-11-07-01-estilo-arquitectonico.md).