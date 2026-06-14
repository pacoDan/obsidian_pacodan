### en el servicio  Donaciones - Asignación de las donaciones
no uso State por que necesito cambiar el comportamiento, no estados, cada vez que se ejecuta el proceso de matchmaking, ni tampoco hago uso intensivo de condiciones 
El código de los algoritmos las puedo encapsular de otra manera en otro patrón de diseño aparte , no en el servicio de donaciones exclusivamente
Debido al proceso de mathmaking para las asignaciones de donaciones hacia las entidades y obtener la trazabilidad, necesito poder tener cada asignación como si fuese objeto, lo cual Command es ideal

debe mostrar a los beneficiarios y a los donantes la ubicación en tiempo real de los camiones

###  Eventos e integración con medios de notificación

El patrón observer me permite poder ejecutar acciones ante una determinada acción, si hay una nueva acción llamada por ejemplo "notificación de Donación vencida", entonces se crea una nueva instancia de Observador/ConcreteSubscriter  que implemente la interfaz


### Logística - Entrega y planificación de rutas

pequeña logica que se terceariza  su calculo de ruta
debe estar listo al principio del dia, ASINCRÓNICO

---


hacer crontask con el servicio hecho de donaciones, deberian de podeer asignar a los beneficiarios  segun los donantes 


### Logística - Monitoreo de camiones en tiempo real

asincronico, en horario de baja carga
