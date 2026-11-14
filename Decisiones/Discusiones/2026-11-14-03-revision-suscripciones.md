# Revisión de la decisión sobre suscripciones a categorías

- Semana: 2.
- Decisión revisada: [0002, gestión de suscripciones mediante Publish-Subscribe](../../docs/decisions/0002-gestión-de-suscripciones-a-categorías-de-noticias-mediante-publish-subscribe.md).
- Revisión preparada por: Fernán (ASC).
- Estado: preguntas preparadas para discutir con los ASS; respuestas y resultado pendientes.

## Cuestiones para los ASS

1. **Problema que resuelve el patrón.** No nos parece mal el patrón Publish-subscribe para esto, pero necesitamos aclarar una cuestión. Los requisitos funcionales 1.1 1.2 hablan de suscribirse a categorías y darse de baja, pero el patrón Publish-Subscribe está más pensado en distribuir mensajes o eventos a suscriptores. ¿Qué publicaríamos y qué recibirían los lectores? ¿Hace falta para cumplir los requisitos, o solo gestionando la relación entre lector y categoría llega? 

2. **Sobre el anonimato y la baja de un lector.** ¿Cómo reconoce el sistema qué suscripciones tiene que cancelar un lector sin pedirle datos personales? ¿Dónde mantenemos esas suscripciones y cómo se recuperan luego? Podríamos poner un id o una credencial.

3. **Funcionamiento y representación.** Si un lector se suscribe a cosas de política y después se publica una noticia, ¿qué componentes intervienen en eso, quién publica, quién recibe y cómo se relacionan con el cliente y el servidor?

4. **Comparación de alternativas.** ¿Por qué escogisteis Publish-Subscribe? (por ejemplo antes que observer o mediator). ¿Cómo lo justificáis?

## Respuestas de los ASS y resultado

(vuestras respuestas)
## Referencias

- [Requisitos y problemas de diseño](../../Requisitos/requisitos.md).
