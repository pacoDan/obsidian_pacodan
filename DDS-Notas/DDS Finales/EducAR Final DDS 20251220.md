Fecha: 20/12/2025

Arquitectura (40)
B (15)

C (10)

Modelo de Dominio (30)
A (20)

B (10)

Persistencia (30)
B  (10)

A (20)

A (15)

Final

Apellido y Nombre: .............................................................................Legajo:.........................................     Plan:..................

Contexto

EducAR+

Una  institución  nacional  ha  impulsado  una  iniciativa  orientada  a  fortalecer  la  educación
inclusiva  y  accesible  mediante el uso de tecnologías digitales. Este proyecto surge como
respuesta a la necesidad de reducir la brecha educativa y digital que persiste en distintos
sectores  de  la  sociedad,  afectando  especialmente  a comunidades con acceso limitado a
recursos formativos y tecnológicos.

En  este  marco,  se  propone  el  diseño  y  desarrollo  de  EducAR+  un  Sistema  Educativo  Digital  Inclusivo,  basado  en  una
plataforma  de  cursos  en  línea  abierta  a  personas  de  todas  las  edades  y  niveles  educativos,  compuesta  por  diversos
servicios independientes que colaboran entre sí.

Servicio I - Gestión de Cursos y sus contenidos

Será el servicio central de la plataforma, responsable de administrar la información de los cursos y de permitir la creación,
organización y publicación de los contenidos educativos.

Un  curso  representa  una  unidad  de  contenido  educativo  dentro  de  la  plataforma.  De  cada  uno  se  conoce  su  título,
descripción  general, categoría y subcategoría a la que pertenece (por ejemplo, Historia → Antigua, Contemporánea, etc.),
idioma y nivel de dificultad, el cual permite clasificar los cursos según los conocimientos previos requeridos.

Cada  curso  podrá  ser  creado  y  administrado  por  un  profesor,  quien  será  responsable  de la correcta organización de los
contenidos. Una vez creado, un administrador de la plataforma deberá revisarlo y aprobarlo para que pueda ser publicado y
puesto a disposición de los alumnos. Los cursos se pueden organizar en uno o más módulos que estructuran el contenido
de forma lógica y progresiva. Cada módulo está compuesto por una o más lecciones, que contienen el contenido específico
a ser aprendido.

Las  lecciones  representan  la  unidad  mínima  de  aprendizaje  dentro  de  un  módulo.  En  ellas  se  presenta  el  contenido
específico  que  el  alumno  debe  estudiar,  pudiendo  incluir  diferentes  contenidos  como  texto,  video,  presentaciones  o
actividades prácticas. Es necesario poder identificar qué tipo de contenido contiene, la URL del mismo, cuál es su duración
(de ser un texto será un aproximado de cuántos minutos lleva leerlo)  y un título.

Servicio II - Gestión de Usuarios

Este servicio será el encargado de administrar la información de las personas que utilizan la plataforma. Toda persona que
desee  utilizar  el  Sistema  deberá  registrarse,  almacenando  información  básica  como  nombre,  apellido,  número  de
documento, correo electrónico, número de teléfono y edad. Estos datos permitirán identificar correctamente a los usuarios y
personalizar su experiencia dentro de la plataforma.

Adicionalmente,  los  usuarios  deberán  indicar  sus  categorías de interés, las cuales serán utilizadas como insumo para los
mecanismos de recomendación de cursos.

Existen dos tipos principales de usuarios:

●  Alumnos: podrán inscribirse en cursos, acceder a los contenidos y realizar el seguimiento de su progreso.
●  Profesores: serán responsables de la creación, organización y mantenimiento de los cursos y sus contenidos.

1

El  proceso  de  registro  deberá  incluir  una  validación  de  identidad. Para ello, el Sistema deberá integrarse con un servicio
externo provisto por el RENAPER, con el objetivo de verificar que el número de documento, nombre y apellido ingresados
por el usuario correspondan a una persona real registrada en dicha entidad.

Servicio III - Inscripciones y Proceso de Admisión

Este servicio será responsable de gestionar la inscripción de los alumnos a los cursos y de coordinar el proceso de admisión
asociado a cada inscripción.

Los  alumnos  podrán  solicitar  la  inscripción  a  los  cursos  que  deseen.  Al  realizarse  una  inscripción,  se  dará  inicio  a  un
proceso de evaluación automática, en el cual el Sistema validará si el alumno cumple con los requisitos mínimos del curso
en función de sus intereses declarados, sus conocimientos previos y experiencia educativa registrada en la plataforma.

Esta  evaluación  será  realizada  por  un  componente  de  Inteligencia  Artificial,  encargado  de  analizar  el  perfil del alumno y
determinar si la inscripción es aprobada o rechazada. En caso de aprobación, el alumno quedará formalmente inscripto en
el curso y podrá comenzar a acceder a sus contenidos. En caso de rechazo, el Sistema deberá enviar automáticamente una
notificación  al  alumno  por  su  medio  preferido  de  notificación  (a  través  del  Servicio  de  Notificaciones),  detallando  los
requisitos mínimos que no fueron cumplidos y las recomendaciones correspondientes.

Servicio IV - Gestión de Trayectoria de Aprendizaje

Este  servicio  será  responsable  de  registrar  y  calcular  el  progreso  de  los  alumnos  dentro  de  los  cursos  en  los  que  se
encuentren inscriptos.

Para cada curso al que esté inscripto un alumno, el servicio deberá permitir conocer:

●  Estado actual del curso (en progreso, abandonado, finalizado)
●  Fecha del último acceso al contenido
●  Porcentaje total de progreso del curso

Este servicio deberá permitir que el alumno consulte en todo momento qué lecciones ya fueron completadas, qué módulos
se encuentran finalizados, cuál es el porcentaje de avance dentro de un módulo y cuál es su progreso total dentro del curso
(en porcentaje).

Para calcular correctamente el progreso, el servicio deberá integrarse con el Servicio de Gestión de Cursos, obteniendo la
estructura completa del curso (módulos y lecciones) para poder realizar los cálculos correspondientes.

Servicio V - Notificaciones

Este  servicio  será  responsable  de  ejecutar  el  envío  de notificaciones a los alumnos, utilizando el medio de comunicación
preferido por cada uno.

El Sistema deberá enviar diferentes tipos de notificaciones automáticamente en los siguientes casos:

●  Cuando el progreso de un alumno en un curso alcance el 50%.
●  Cuando el progreso de un alumno en un curso alcance el 100%, enviando un mensaje de felicitación.
●  Cuando  un  alumno  no  registre  interacción con un curso durante más de 20 días, con el objetivo de incentivarlo a

retomar la actividad.

●  Envío de recomendaciones semanales de cursos, basadas en los intereses declarados por el alumno y su historial

de actividad.

Este servicio se integrará con servicios externos para el envío de notificaciones, tales como:

●  APIs de correo electrónico (por ejemplo, SendGrid)
●  Servicios de mensajería SMS y WhatsApp (por ejemplo, Twilio)
●  Calendarios externos (por ejemplo, Google Calendar)

2

Alcance y Requerimientos

El sistema deberá permitir:

1.  El alta, baja y modificación de cursos con sus módulos y lecciones.
2.  La trazabilidad de los estados en los que puede estar un curso (“proceso de aprobación” por parte de administradores).
3.  La gestión de los usuarios y la integración con RENAPER para su validación.
4.  La inscripción de alumnos a cursos junto con su proceso de admisión.
5.  La trazabilidad de los módulos y lecciones completadas, así como también el progreso de los alumnos en los cursos que están

inscriptos.

6.  El envío de notificaciones para todos los casos planteados.

Punto 1 - Arquitectura (40 puntos)

A.

B.

(10 puntos) Cuando un alumno se inscribe a un curso, además de recibir la notificación (por su medio preferido) respecto a si su
inscripción  fue  aceptada  o  no,  interesa  también  mostrarle  una  burbuja  emergente  en  la  plataforma  web  que  contenga
exactamente  el  mismo  detalle.  Esta  burbuja  emergente  solamente  deberá  aparecer  si  el  alumno  está  navegando  por  la
plataforma  (logueado)  cuando  el  resultado  de  su  inscripción  esté  listo. ¿Cómo implementaría esta funcionalidad? Describa el
proceso completo.

(10 puntos) Actualmente, los contenidos multimedia de las lecciones se almacenan en el servidor del propio sistema. Durante
los  períodos  de  alta  demanda,  se  observa  un  incremento  significativo  en  los  tiempos  de  carga. La transferencia de archivos
grandes desde un único servidor genera latencia, saturación de ancho de banda y una experiencia deficiente para los alumnos.
No contamos con la capacidad de añadir otro servidor, por lo tanto, ¿Qué solución sugiere implementar para reducir dicho tiempo
de respuesta? Justifique su respuesta.

C.

(10 puntos)

a.

(5 puntos) Para cada una de las notificaciones que se deben enviar desde el Servicio de Notificaciones, detalle cómo
realizaría el envío de cada una de ellas teniendo en cuenta que no se debe enviar 2 o más veces seguidas la misma
notificación.

b.

(5 puntos) ¿Cómo implementaría un nuevo tipo de notificación en el Servicio de Notificaciones? Por ejemplo: Cuando
un alumno apruebe una evaluación, notificando el resultado y sugiriendo el siguiente contenido.

D.

(10 puntos)

a.  Plan 08 - Algunas veces cuando los usuarios están intentando registrarse en el Sistema, luego de completar todos los
datos  solicitados,  obtienen  un  timeout  como  resultado,  sin  entender  qué  está  pasando.  ¿Qué  considera  que  está
sucediendo? ¿Qué propone para solucionarlo? Detalle de forma completa y clara su propuesta.

b.  Plan  23  -  Considerando  una  arquitectura  de  microservicios,  elabore  un  diagrama  de  despliegue  demostrando
claramente  qué  servicios  y  componentes  involucraría.  ¿Qué  cambios  arquitectónicos  se  producirían  al  utilizar  un
enfoque de coreografía en lugar de uno de orquestación en una arquitectura de microservicios?

Punto 2 - Modelado de Dominio (30 puntos)

A.

(20 Puntos) Documentar la solución de los Servicios: III, IV y V.

a.  Plan 08 - Documentar la solución utilizando diagramas UML (diagrama de clases obligatorio).

b.  Plan 23 - Documentar la solución utilizando diagramas UML (diagrama de clases obligatorio) separados por Servicio.

B.

(10  Puntos)  Justificar  las  decisiones  de  diseño  que  se  tomen,  por ejemplo, haciendo referencia a los principios que guían al
diseño o las consecuencias de aplicar un determinado patrón. También puede optar por justificar mediante código, pseudocódigo
o algún otro diagrama complementario.

Punto 3 - Persistencia (30 puntos)

A.

(20 Puntos) Diseñar el modelo de datos para poder persistir en una base de datos relacional indicando las entidades con sus
respectivos campos, claves primarias, las foráneas, cardinalidad, modalidad y las restricciones según corresponda.

a.  Plan 08 - Los requerimientos n° 1, 2, 4 y 5.

b.  Plan 23 -  Los Servicios: I, III y IV. Cada servicio debe comunicarse en un diagrama separado.

B.

(10 Puntos) Justificar: a) Qué elementos del modelo es necesario persistir; b) Cómo resolvió los impedance mismatches; c) Las
estructuras de datos que deban ser desnormalizadas, si corresponde.

3

