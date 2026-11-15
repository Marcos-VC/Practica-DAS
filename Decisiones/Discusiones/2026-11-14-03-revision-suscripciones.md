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

5. **Nuesta propuesta.** Creemos que el patrón Observer se adecúa más a este caso. Si diferís, explicad el porqué.

## Respuestas de los ASS y resultado

<<<<<<< HEAD
1. Al publicarse una noticia, el servidor genera un evento con su identificador y categoría. El sistema utiliza ese evento para poner un aviso de la noticia a disposición de los lectores suscritos a esa categoría. El lector ve los avisos cuando consulta el periódico y accede a la noticia correspondiente. Para registrar altas y bajas bastaría con guardar la relación entre lector y categoría; elegimos Publish-Subscribe para distribuir también los avisos. Esa distribución se plantea aquí como un supuesto de diseño, no como un requisito expresamente confirmado por RF-01.1 y RF-01.2.

2. El servidor genera un identificador aleatorio para el lector y guarda en la base de datos de suscriptores las categorías asociadas a ese identificador, sin solicitar su nombre ni su correo para esta función. El navegador conserva una credencial que permite al servidor reconocer sus suscripciones y tramitar la baja de las categorías solicitadas. Si el lector pierde la credencial, no podrá recuperar desde ese navegador las suscripciones anteriores sin un mecanismo adicional de recuperación. Este identificador es seudónimo: no garantiza por sí solo el anonimato, que también depende de los datos y registros que conserve el sistema.

3. El cliente pide al servidor mediante HTTP/REST suscribirse a Política. El servidor registra la relación en la base de datos de suscriptores. Cuando un redactor publica una noticia de Política, el componente de noticias del servidor publica un evento en el canal correspondiente. Un intermediario lo distribuye al componente que prepara los avisos para los lectores de esa categoría. Los avisos se guardan para que el lector los consulte aunque estuviera desconectado cuando se publicó la noticia. En UML se representan el cliente, los componentes de suscripciones, noticias y avisos, el intermediario y el almacenamiento, junto con sus relaciones. En esta propuesta, el diagrama de clases actual debe revisarse: muestra que la categoría notifica directamente al suscriptor y no representa el intermediario.

4. Elegimos Publish-Subscribe porque, en el flujo propuesto, la publicación de una noticia y la distribución de avisos quedan separadas mediante un intermediario: el componente que publica no necesita conocer directamente a cada destinatario. Observer resolvería la notificación con una relación más directa entre el elemento observado y sus observadores; Mediator centralizaría la coordinación entre componentes y serviría para gestionar altas y bajas, pero no aporta por sí mismo la distribución por categorías planteada. Publish-Subscribe introduce a cambio un intermediario y complica el seguimiento del flujo. Su capacidad de crecimiento dependerá de cómo se diseñe y dimensione ese mecanismo.

5. Observer es una alternativa válida: la categoría mantiene una relación directa con sus suscriptores y les notifica cuando se publica una noticia. Es una solución sencilla y se corresponde con el diagrama de clases actual. En la propuesta de Publish-Subscribe, en cambio, el componente de noticias publica el evento en un canal y un intermediario se encarga de distribuirlo, sin que el publicador mantenga directamente la lista de destinatarios. Para el flujo de avisos que planteamos, preferimos esta separación, aunque añade un componente y más complejidad.
=======
(vuestras respuestas)
>>>>>>> e4ec103ce9a929974366c05015cb1b3e7eddbc43

## Referencias

- [Requisitos y problemas de diseño](../../Requisitos/requisitos.md).
