# Revisión de la decisión sobre cálculo y almacenamiento de estadísticas de uso

- Semana: 3.
- Decisión revisada: [0004, cálculo y almacenamiento de estadísticas de uso del sistema](../0004-cálculo-y-almacenamiento-de-estadísticas-de-uso-del-sistema.md).
- Revisión preparada por: Fran (ASC).
- Estado: respuestas de los ASS registradas

## Cuestiones para los ASS

1. **Contradicción con el modelo síncrono de la Decisión 0001.** En la revisión del estilo arquitectónico (Decisión 0001) acordamos un modelo Cliente-Servidor, descartando infraestructuras asíncronas complejas, como colas o eventos, por su sobrecarga. Sin embargo, en esta decisión elegís un *"Patrón Observer asíncrono con procesamiento por lotes"* que requiere *"gestionar una cola o búfer temporal en memoria"*. ¿Cómo encaja esto con la simplicidad de la arquitectura elegida? ¿Cómo gestionaríamos el riesgo de pérdida de métricas si el servidor se reinicia inesperadamente o se sobrecarga?

2. **Reutilización del requisito RD-02.** El requisito `RD-02` (almacenamiento en las dos bases de datos PostgreSQL 19) ya fue utilizado como *Decision Driver* en la Decisión 0001. Dado el criterio de no *utilizar un requisito en dos o más decisiones*, ¿no deberíamos retirar `RD-02` de esta decisión (PD-04) o de los *Decision Drivers* de la 0001?

3. **Complejidad del procesado por lotes frente a Decorator.** La Alternativa 1 (Decorator) se descarta por realizar escrituras síncronas que penalizarían la latencia. Pero, si retomamos la primera pregunta, es precisamente lo que queremos. Además, el Observer asíncrono y el procesamiento por lotes añade una complejidad innecesaria, en comparación con un contador en memoria o un registro síncrono optimizado.

4. **Representación UML.** Para que Álvaro y Alejandro puedan modelar esto en los diagramas de clases y paquetes del servidor, sin cambiar demasiado la estructura básica de Cliente-Servidor, ¿dónde se ubicaría el búfer temporal y qué componente ejecuta ese proceso planificado?

5. **Exclusividad de requisitos funcionales.** El driver `PD-04 (RF-03.1)` exige calcular la tasa de rebote mediante la fórmula `(Visitas - Conversión) / Visitas * 100`. ¿Este cálculo queda confinado exclusivamente a este componente del servidor, o interviene algún otro módulo del Front-end?

## Borrador de posibles respuestas de los ASS

1. Queremos mantener Observer asíncrono con procesamiento por lotes. Cliente-servidor no exige que todas las operaciones internas sean síncronas: el cliente puede consultar noticias mediante HTTP/REST mientras una tarea del mismo servidor procesa estadísticas. Defendemos la alternativa por separar el procesamiento estadístico de la respuesta de lectura.

2. La decisión 4 utiliza la base de datos destinada a administradores y redactores, respetando la restricción de persistencia. Hemos hecho los cambios convenientes ya.

3. La comunicación síncrona entre cliente y servidor no obliga a escribir las estadísticas antes de responder cada consulta. La ventaja que buscamos es que la lectura de una noticia no tenga que esperar a la consolidación y escritura de sus métricas. Queremos mantener la alternativa elegida.

4. Como propuesta de modelado, los servicios de noticias y suscripciones notifican las interacciones relevantes a un `ObservadorEstadisticas` interno, que registra datos ligeros en un `BufferMetricas`. Una `TareaConsolidacion`, activada periódicamente en el servidor, entrega los datos a un `AgregadorEstadisticas`, encargado de calcular y guardar los resultados mediante un repositorio de estadísticas en la base de datos de administradores y redactores. Los nombres son orientativos y no hay diagrama sobre esto.

5. Proponemos centralizar la definición y el cálculo de las métricas en el módulo estadístico del servidor. El frontend puede comunicar interacciones y datos de permanencia necesarios para medirlas y mostrar los resultados, sin mantener una segunda definición de las fórmulas. La regla de exclusividad se refiere al uso documental del requisito entre decisiones, no a impedir que varios componentes colaboren en cumplirlo.

## Resultado 

Queremos mantener Observer asíncrono y el procesamiento por lotes. No se ha modificado el MADR.

## Referencias

- [Decisión 0001, selección del estilo arquitectónico principal](../0001-selección-del-estilo-arquitectónico-principal-para-el-periódico-digital.md).
- [Requisitos y problemas de diseño](../../Requisitos/requisitos.md).
