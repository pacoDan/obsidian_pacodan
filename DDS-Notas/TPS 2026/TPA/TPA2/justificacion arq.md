Dada la restricción de dividir el sistema en un máximo de **dos microservicios** para organizar a los dos equipos de desarrollo, la división más natural y cohesiva —siguiendo los lineamientos de "Base de Datos por Servicio" y "Separación de Responsabilidades"— sería la siguiente:

### 1. Microservicio de Gestión de Donaciones

Este servicio concentraría toda la lógica de negocio "núcleo" relacionada con el procesamiento de las donaciones y la vinculación con los beneficiarios.

- **Gestión de Actores:** Operaciones CRUD sobre personas donantes (humanas y jurídicas) y entidades beneficiarias.
- **Gestión de Bienes y Necesidades:** Administración de las necesidades materiales (recurrentes y extraordinarias) y de las donaciones recibidas.
- **Ciclo de Vida de la Donación:** Registro y cambio de estados de las donaciones (ej. "En Depósito"), garantizando la **trazabilidad y auditoría** de cada movimiento mediante una bitácora de eventos.
- **Motor de Matchmaking:** Ejecución asincrónica de los algoritmos de asignación (Compatibilidad Semántica y Prioridad) para generar rankings de beneficiarios.
- **Sistema de Notificaciones:** Responsable de disparar avisos por inactividad de donantes o asignación de donaciones (aplicando el patrón **Observer**) [Chat History].

### 2. Microservicio de Logística y Distribución

Este servicio se encargaría exclusivamente del movimiento físico de los bienes desde el depósito hasta su destino final, manejando la complejidad técnica de la ruta.

- **Gestión de Recursos Físicos:** Administración de la flota de camiones y conductores.
- **Planificación de Rutas:** Integración con el **proveedor externo**. Esto incluye realizar solicitudes en lotes de hasta 100 donaciones y exponer la **URL de callback** para registrar los planes de ruta una vez procesados por el externo [Chat History, Requerimientos].
- **Operación en Campo:** Permitir a los choferes informar el inicio de sus rutas y gestionar la recepción de la entrega por parte de las entidades.
- **Seguimiento en Tiempo Real:** Lógica para mostrar la localización de los camiones tanto a donantes como a beneficiarios.
- **Re-planificación:** Responsabilidad de gestionar las donaciones que el planificador externo no pudo asignar en una primera instancia para volver a procesarlas.

### Interacción entre los Microservicios

Siguiendo los requerimientos técnicos y las convenciones REST:

- **Comunicación Sincrónica (REST/HTTP):** Logística puede consultar a Donaciones la información detallada de los bienes y direcciones de los beneficiarios para completar el plan de carga.
- **Comunicación Asincrónica (Broker de Mensajes):** Cuando Donaciones marca una donación como "Asignada", emite un evento que el microservicio de Logística consume para poner esa entrega en la cola de planificación nocturna. Asimismo, cuando Logística confirma una "Entrega Realizada", notifica a Donaciones para que este actualice el estado final de la donación y registre el evento en la auditoría [Chat History].

Esta estructura permite que cada equipo trabaje de forma independiente sobre un dominio claro (Negocio vs. Operación), cumpliendo con el principio **Open/Closed** y facilitando la **escalabilidad** de componentes pesados como el ruteador o el asignador.