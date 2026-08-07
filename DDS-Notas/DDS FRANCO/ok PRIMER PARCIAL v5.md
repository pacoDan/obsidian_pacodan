

### 1. Niveles de Colectivos y Cuotas de Proyectos (Patrón _State / Strategy_)

- **Contexto similar:** _Firmadocs_ (Planes de facturación mensual) y _Copia.me_ (Calidades de servicio).
- **Requerimiento:** Al igual que en las plataformas profesionales, los colectivos pueden registrarse en diferentes categorías (por ejemplo: _Asociación Barrial_, _ONG Internacional_, _Fundación_). Según su categoría y su "reputación" en la plataforma, tendrán un límite de proyectos activos simultáneos que pueden publicar por mes (ej. una Asociación Barrial puede tener hasta 2 activos, mientras que una Fundación hasta 10). Una vez agotado el cupo, el sistema debe impedir la creación de nuevos proyectos hasta el mes siguiente, a menos que realicen una solicitud de ampliación que requiera aprobación administrativa.

### 2. "Mi Tipo de Colaboración" y Filtros Dinámicos (Patrón _Composite_ + _Strategy_)

- **Contexto similar:** _Findr_ (Filtros de grilla y Mi Tipo).
- **Requerimiento:** Las personas colaboradoras quieren poder crear "Perfiles de Búsqueda" o _"Mis Tipos"_ para recibir alertas o filtrar la lista general de proyectos. Un _Tipo de Búsqueda_ se compone de uno o más filtros combinables (por ejemplo: que requiera la habilidad "Desarrollo Frontend React" **Y** tenga modalidad "Completamente Gratuita", o que requiera "Diseño UI" **Y** pida un compromiso de "menos de 10 horas semanales"). Al aplicar el patrón _Composite_, el sistema debe evaluar polimórficamente si un proyecto encaja (_matchea_) con el conjunto de condiciones definidas por la colaboradora.

### 3. Propuestas de Cambio en Proyectos (Patrón _Command_ / Versión de Historial)

- **Contexto similar:** _Hitbug_ (Hits y modificaciones atómicas sobre Bags).
- **Requerimiento:** Los proyectos no siempre son estáticos. Las colaboradoras o el equipo del colectivo pueden proponer modificaciones al proyecto (añadir una habilidad requerida, cambiar el tipo de compromiso horario, o editar el título). Estas modificaciones deben ser atómicas y se denominan _"Propuestas de Cambio"_ (que pueden estar pendientes, rechazadas o aprobadas). Deben modelarse como objetos para poder aplicarse de forma diferida tras la aprobación del colectivo, registrar un historial completo de cambios y, eventualmente, permitir "deshacer" los efectos de las últimas modificaciones.

### 4. Flujo de Notificaciones y Alertas por Múltiples Canales (Patrón _Observer_ + _Adapter_)

- **Contexto similar:** _Noodle_ (Tareas de cambio configurables por administrador) y _Firmadocs_ (Notificaciones diarias y de eventos).
- **Requerimiento:** Cuando ocurren ciertos eventos clave en la plataforma (por ejemplo, una colaboradora se anota en un proyecto, o un colectivo aprueba una propuesta de colaboración), se debe notificar a los interesados por los medios que hayan elegido (Email, WhatsApp, o Slack). Estas tareas de notificación deben ser dinámicamente configurables por el administrador de la plataforma para cada evento. Para interactuar con las APIs de estos proveedores externos sin acoplar nuestro dominio, se deben desarrollar adaptadores (_Adapters_) que traduzcan la interfaz de nuestro sistema a las librerías externas correspondientes.

### 5. Integración y Verificación de Habilidades vía GitHub/LinkedIn (Patrón _Adapter_)

- **Contexto similar:** _LivreStream_ (Plataforma de mensajería externa) y _Copia.me_ (Detección automática de clones).
- **Requerimiento:** Para evitar que las colaboradoras declaren habilidades que no poseen, el sistema debe permitir "verificar" de forma automática ciertas habilidades de desarrollo de software utilizando servicios de terceros (por ejemplo, analizar los repositorios públicos de su cuenta de GitHub mediante la API de GitHub). Si la API externa indica que la usuaria tiene un alto porcentaje de contribuciones en un lenguaje específico (ej. Java), la habilidad "Desarrollo Web Java" se marca automáticamente como _Verificada_. El sistema debe abstraer estas llamadas HTTP a través de un adaptador para facilitar las pruebas unitarias usando _mocks_.

### 6. Sistema de Postulación y Aprobaciones por Terceros (Patrón _State_)

- **Contexto similar:** _Noodle_ (Aprobación de cambios en grupos cerrados por otro docente).
- **Requerimiento:** El proceso de inscripción de una colaboradora en un proyecto (la "Colaboración") debe pasar por un flujo de estados: _Borrador/Iniciada_, _Pendiente de Validación_ (cuando el colectivo evalúa si el perfil aplica), _Aprobada_ (los datos de contacto se vuelven visibles), o _Finalizada/Rechazada_. Si el proyecto tiene incentivo económico, la aprobación de la postulación no la puede hacer el mismo creador del proyecto por cuestiones de transparencia; debe ser aprobada por un "Coordinador de Alianza" de un colectivo par (segundo validador). Los cambios de estado deben controlar qué acciones están habilitadas en cada momento.

### 7. Procesamiento Diario de Recordatorios de Impacto (Planificación Externa / _Cron_)

- **Contexto similar:** _Copia.me_ (Planificador nocturno) y _LivreStream_ (Terminador de transmisiones).
- **Requerimiento:** Las colaboradoras que se anotaron en un proyecto pero llevan más de 15 días sin registrar avances deben recibir un recordatorio de compromiso. Asimismo, los proyectos inactivos por más de 30 días deben cerrarse automáticamente. Ejecutar estas verificaciones en tiempo real en cada request web es inviable. Se requiere diseñar un proceso ejecutable de forma asincrónica (un componente CLI/JAR independiente) que se dispare mediante un planificador externo (como _crontab_ del sistema operativo) cada noche a la madrugada para auditar los estados y enviar notificaciones masivas.

### 8. Ranking de Colectivos de Mayor Impacto (Performance y Desnormalización)

- **Contexto similar:** _Juego de Tronos_ (Cuello de botella en casas importantes) y _Ginpass_ (Ranking de bebidas lento).
- **Requerimiento:** Se necesita mostrar en la página principal de la plataforma un "Ranking de Colectivos con Mayor Impacto Social", calculado en base a la sumatoria de horas de colaboración efectivamente realizadas en todos sus proyectos finalizados. En escenarios de alta concurrencia, realizar las consultas relacionales y recorrer todos los grafos de objetos para calcular este valor en tiempo real genera un grave cuello de botella en la base de datos. Se debe proponer una estrategia de **desnormalización** para almacenar los totales de horas de impacto de forma incremental o utilizar almacenamiento rápido en memoria (como _Redis_) para caching.

### 9. Asignación Equitativa de Asesorías de Emergencia (Algoritmos de Reparto)

- **Contexto similar:** _Copia.me_ (Reparto de documentos entre freelancers).
- **Requerimiento:** Cuando un colectivo reporta un problema crítico de infraestructura o seguridad en producción, se genera un "Ticket de Asistencia de Emergencia". La plataforma debe repartir de forma equitativa esta solicitud entre las colaboradoras registradas como asesores de infraestructura disponibles. El algoritmo debe filtrar únicamente a aquellas colaboradoras que no hayan superado su cupo máximo de horas de voluntariado mensual acordado, mezclarlas para asegurar un reparto equitativo y asignar el ticket notificando por correo electrónico.

### 10. Alertas de Nuevos Proyectos Cercanos en Tiempo Real (Arquitectura y Protocolos)

- **Contexto similar:** _Findr_ (Alertas de cercanía) y _SwordGo_ (Notificaciones de ítems cercanos y Websockets).
- **Requerimiento:** Las colaboradoras pueden configurar alertas para tipos específicos de proyectos dentro de un radio de distancia configurable (ej. proyectos de colectivos a menos de 5 km de su ubicación). Cada vez que un colectivo da de alta un proyecto de interés, la plataforma debe notificar de inmediato a los dispositivos móviles de las colaboradoras cercanas que estén conectadas. Dado que HTTP tradicional no permite comunicaciones iniciadas desde el servidor, se debe analizar y justificar el cambio de arquitectura hacia el uso de **Websockets** o el uso de una **Cola de Mensajes** para resolver la comunicación bidireccional en tiempo real.

---

📊 ¿Te gustaría que elijamos uno o dos de estos requerimientos complejos para diseñar su diagrama de clases UML detallado, analizando cómo interactuarían los objetos y patrones en el código?