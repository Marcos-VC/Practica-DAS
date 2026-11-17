# Revisión de la decisión sobre modalidades de suscripción y facturación mediante Stripe

- Semana: 3.
- Decisión revisada: [0003, implementación de las modalidades de suscripción y su ciclo de facturación mediante Stripe](../0003-implementación-de-las-modalidades-de-suscripción-y-su-ciclo-de-facturación-mediante-stripe.md).
- Revisión preparada por: Fran (ASC).
- Estado: preguntas preparadas para discutir con los ASS; respuestas y resultado pendientes.

## Cuestiones para los ASS

1. **¿Es necesario el patrón Strategy para tres tarifas fijas?** El patrón Strategy es muy útil cuando tenemos una clase que hace algo específico, pero de muchas formas diferentes. La idea del patrón es extraer todos esos algoritmos para colocarlos en clases separadas, haciendo que el patrón Strtegy encaje bien cuando existen algoritmos complejos. Sin embargo, aquí contamos solamente con tres planes con precios y periodos cerrados (Trimestral 70€, Semestral 130€ y Anual 190€). ¿No introduce demasiada complejidad estructural un patrón de diseño para tres tarifas fijas? ¿No podrían modelarse de otra manera más sencilla, como un enumerado?

2. **Gestor de reintentos y arquitectura Cliente-Servidor.** En las consecuencias negativas decís que los reintentos obligan a implementar una planificación en el backend para no bloquear las peticiones HTTP/REST del cliente, razón parecida a la que aparece en la Alternativa 2. ¿Cómo se resuelve exactamente este flujo sin introducir infraestructura asíncrona (colas o eventos) que descartamos en la Decisión 0001? ¿Qué ocurre cuando falla el primer intento, y cuándo se ejecutan los siguientes?

3. **Anonimato vs. trazabilidad.** Indicáis que la solución permite *"llevar un control estricto y auditable en la base de datos PostgreSQL 19 de suscriptores del número exacto de reintentos"*. Si tenemos en cuenta las restricciones de privacidad y anonimato de los usuarios, ¿qué tenéis pensado almacenar en la base de datos para asociar los reintentos y las renovaciones recurrentes con el suscriptor sin comprometer sus datos personales?

4. **Representación UML.** Para que Álvaro y Alejandro puedan trasladar esta decisión a los diagramas de clases y paquetes del servidor, ¿qué interfaz tiene el `StripeAdapter`, cómo se relaciona el gestor de cobros con las estrategias de plan, y dónde queda encapsulada la lógica de los 3 reintentos?

5. Como indica el enunciado de la práctica, no podemos *utilizar un requisito funcional o de dominio en dos o más decisiones*. Por lo tanto, como el `RD-01` aparece en esta decisión (PD-03), ¿no deberíamos quitarlo de los *Decision Drivers* de la decisión 0001? 

## Respuestas de los ASS y resultado

1. 

2. 

3. 

4. 

5. 

## Referencias

- [Decisión 0001, selección del estilo arquitectónico principal](../0001-selección-del-estilo-arquitectónico-principal-para-el-periódico-digital.md).
- [Requisitos y problemas de diseño](../../Requisitos/requisitos.md).