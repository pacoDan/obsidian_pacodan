Apellido y Nombre: ............................................................................Legajo:............................................... Plan:......................

Arquitectura (35)

Modelo del Dominio (30)

Persistencia (35)

Total

A (12.5)

B (12.5)

C (10)

A (20)

B (10)

A (25)

B (10)

Fecha: 28/02/2026

Condiciones de aprobación: Para aprobar debe sumar como mínimo el 60% de los puntos rendidos y no menos del 50 % en cada
sección.

Contexto

 TurnoÁgil

TurnoÁgil es una plataforma centralizada orientada a la gestión de turnos médicos bajo demanda, utilizada por diversas clínicas privadas
adheridas. Funciona dentro de un modelo de cobertura unificado, una única red de salud, en el que los pacientes cuentan con distintos
niveles de plan (por ejemplo, Básico y Premium).

Su propósito es permitir la reserva de turnos en tiempo real, incorporando el cálculo automático de copagos según el plan del paciente,
así como el registro de eventos clínicos relevantes.

El sistema se organiza en los siguientes servicios:

Servicio I - Gestión de Profesionales y Agendas

Administra los profesionales y sus agendas disponibles. De cada profesional se registra: matrícula, nombre, especialidades y consultorios.
Cada agenda define franjas horarias disponibles por día. Además, cada especialidad tiene definido un valor base del turno administrable
por clínica.

Debe exponer un endpoint para consultar disponibilidad por profesional y especialidad.

Servicio II - Motor de Cobertura y Copagos

Calcula el copago del turno según reglas configurables definidas a nivel plataforma (globales para todas las clínicas).
Las clínicas no pueden modificar estas reglas, solo informar el plan del paciente.

Actualmente existen 3 reglas que se pueden aplicar de manera conjunta (no excluyentes entre sí), pero podrían agregarse nuevas en un
futuro. Las 3 reglas actuales son:

●  Cobertura Total: si el plan del paciente es “Premium”, copago = 0.
●  Plan Básico: copago = 30% del valor base del turno.
●  Recargo Urgente: si el turno se solicita con menos de 2 horas de anticipación, +20% sobre el valor base.

Servicio III - Validación Clínica

Antes de confirmar el turno, el paciente debe completar un breve cuestionario según la especialidad escogida. Las preguntas que se le
realizan deben poder ser configurables por un administrador de la plataforma.

El Sistema envía las respuestas a un motor externo de validación médica, el cual expone un endpoint REST para dicho propósito. Si el
motor  detecta  un  riesgo  alto,  el  turno  se  marca  como  “Requiere Revisión Médica”. Esto significa que un profesional médico deberá
revisar el cuestionario con el fin de determinar si ese paciente debe atenderse con urgencias, cancelando su turno.

Servicio IV - Reservas

Gestiona  la  creación  y  el  ciclo  de  vida  de  los  turnos.  Toma como entrada: paciente, profesional, especialidad, fecha/hora, cobertura y
cuestionario.

Debe  registrar  una  bitácora  completa  de  eventos  (creación,  validación,  cálculo  de  copago,  revisión  médica,  cancelación,  asistido,
ausente).

1

El pago y la facturación están completamente fuera del alcance del sistema, pero el sistema expone el código único de reserva para ser
utilizado por sistemas externos.

Alcance y Requerimientos

El sistema deberá permitir:

1.  Gestionar profesionales de la salud, sus especialidades y agendas de disponibilidad horaria.
2.  Consultar disponibilidad de turnos por profesional y especialidad.
3.  Calcular automáticamente el copago de un turno en función del plan de cobertura y reglas globales de la plataforma.
4.  Registrar cuestionarios clínicos previos a la reserva y enviar dicha información a un servicio externo de validación médica.
5.  Que los administradores agreguen/modifiquen/eliminen las preguntas de los cuestionarios por especialidades.
6.  Administrar el ciclo de vida completo de las reservas de turnos, contemplando su creación, validación, confirmación, cancelación,

asistencia y ausentismo.

7.  Mantener una bitácora completa y auditable de los eventos relevantes asociados a cada reserva (creación, validación, cálculo de

copago, revisión clínica, cancelación).

8.  Exponer un identificador único de reserva para su integración con sistemas externos de pago y facturación (fuera del alcance del

sistema).

Punto 1 - Arquitectura (35 puntos)

A.

(12.5 puntos) El servicio externo de revisión médica (el que consume el servicio III) expone
únicamente una API REST y puede demorar varios minutos en responder. Cabe aclarar que
este servicio no puede ser modificado pues no es de nuestra propiedad.

Analice el diagrama propuesto e indique si la arquitectura es correcta o si presenta problemas.
En  caso  de  detectar  problemas,  explique  qué  componente(s)  o  responsabilidad(es)
falta(n)/sobra(n) y por qué.

B.

(12.5 puntos) Continuando con el punto anterior, proponga una alternativa arquitectónica más
simple para integrar las validaciones pendientes, justificando claramente sus decisiones.

C.

(10 puntos) Teniendo en cuenta que actualmente no se cuenta con un equipo especializado
en  desarrollo  mobile,  pero  se  prevé  en  un  futuro  incorporar  una  aplicación  mobile  para
usuarios  recurrentes,  y  que  actualmente  los  inversores  aceptan  el  uso  del  Sistema  vía  web,  proponga  una  arquitectura  de
frontend  que  permita  desacoplar  las  interfaces  del  backend,  facilitar  la  reutilización  de  lógica y soportar múltiples canales de
acceso. Considere que los contratos de los microservicios no deben cambiar por necesidades de las interfaces gráficas.

Punto 2 - Modelado de Dominio (30 puntos)

A.

(20 Puntos) Documentar la solución de los Servicios II, III y IV, únicamente de la capa de dominio:

a.  Plan 08 - Documentar la solución utilizando diagramas UML (diagrama de clases obligatorio).

b.  Plan 23 - Documentar la solución utilizando diagramas UML (diagrama de clases obligatorio) separados por Servicio.

B.

(10  Puntos)  Justificar  las  decisiones  de  diseño  que  se  tomen,  por ejemplo, haciendo referencia a los principios que guían al
diseño o las consecuencias de aplicar un determinado patrón. También puede optar por justificar mediante código, pseudocódigo
o algún otro diagrama complementario.

Punto 3 - Modelo de Datos (35 puntos)

A.

(25 Puntos) Diseñar el modelo de datos para poder persistir en una base de datos relacional indicando las entidades con sus
respectivos campos, claves primarias, las foráneas, cardinalidad, modalidad y las restricciones según corresponda.

a.  Plan 08 - Los requerimientos que se puedan identificar bajo los títulos "Servicio I", "Servicio III" y "Servicio IV".

b.  Plan 23 - Los Servicios: I, III y IV. Cada servicio debe comunicarse en un diagrama separado.

B.

(10  Puntos)  a)  Qué  elementos  del  modelo  es  necesario  persistir;  b)  Cómo  resolvió  los  impedance  mismatches;  c)  Las
estructuras de datos que deban ser desnormalizadas, si corresponde.

2

