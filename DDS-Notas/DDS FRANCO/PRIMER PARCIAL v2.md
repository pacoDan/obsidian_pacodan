Para enriquecer el diseño y la arquitectura de la plataforma **Código a Voluntad**, podemos derivar requerimientos avanzados basados directamente en las prácticas, criterios de evaluación de parciales, tips de diseño y metodologías expuestas en las clases.

A continuación, se presentan los **10 requerimientos adicionales más importantes**, estructurados como **Historias de Usuario (historias con el rol, acción y propósito)**, acompañados de sus respectivos criterios de diseño, justificaciones académicas y tips para el examen.

---

### 1. Robustez y Validación en la Creación (Principio Fail Fast)

- **Requerimiento**: Como **administrador** de Código a Voluntad, quiero evitar que se registren **Proyectos** o **Colectivos** con datos inconsistentes u obligatorios vacíos (como la descripción del proyecto o la falta de habilidades requeridas) para asegurar que la información en la plataforma sea profesional y operable.
- **Criterio de Diseño (Tip de examen)**: **Falla Rápida (Fail Fast)**. Las validaciones de consistencia de datos deben ejecutarse inmediatamente dentro del **constructor** de las entidades de dominio (como `Proyecto` o `Colectivo`) en lugar de permitir que el objeto se instancie en un estado inválido y tire errores tardíos en el flujo. No uses validaciones diferidas en "clases validadoras divinas" ajenas a la entidad.
- **Implementación**: Lanzar excepciones de negocio específicas (que hereden de `RuntimeException`) y evitar capturar tus propias excepciones dentro del dominio.

### 2. Atributos Opcionales y Evitación de Nulos (Manejo de la Opcionalidad)

- **Requerimiento**: Como **colectivo**, quiero que el compromiso horario de un **Proyecto** sea opcional (puede no especificarse) para no restringir la carga de iniciativas flexibles.
- **Criterio de Diseño**: **Evitar el uso de `null` en el flujo de negocio**. Para representar la opcionalidad de un atributo (como el compromiso horario), se desaconseja dejar el campo como `null` debido al riesgo de lanzar un `NullPointerException` al enviarle mensajes. En su lugar, se deben modelar constructores sobrecargados o aplicar un patrón _Null Object_ (por ejemplo, una estrategia de compromiso llamada `CompromisoFlexible` que responda consistentemente a las consultas de horas).

### 3. Inmutabilidad en Entidades Clave (Diseño no Endoble)

- **Requerimiento**: Como **administrador**, quiero asegurar que el título y código de una **Habilidad** creada no puedan modificarse una vez registrados para garantizar la consistencia en el historial de las colaboradoras y evitar la alteración de datos maestros.
- **Criterio de Diseño**: **Inmutabilidad**. El software que no cambia su estado es mucho más fácil de razonar, testear y es menos propenso a errores. Se debe evitar la creación sistemática de métodos _setter_ para todos los atributos. La asignación de dependencias debe realizarse únicamente en el constructor, manteniendo los atributos privados y sin métodos de modificación si el dominio no requiere explícitamente mutar ese dato.

### 4. Integración de Ubicaciones con Servicios Externos (Patrón Adapter)

- **Requerimiento**: Como **colectivo**, quiero que la ubicación de mi organización (actualmente texto libre) sea validada y normalizada geográficamente al registrarla, para asegurar que las búsquedas de proyectos por cercanía sean precisas.
- **Criterio de Diseño**: **Adaptación de Interfaces (Adapter)**. Como la normalización de direcciones requiere integrarse con una API externa (por ejemplo, Google Maps o OpenStreetMap), nuestro dominio no debe acoplarse a las estructuras del proveedor externo (`maps`, `strings` o tipos complejos de su SDK). Se debe definir una interfaz interna en nuestro dominio (ej: `ProveedorDeLocalizacion` con el método `normalizar(Direccion)`) y construir un **Adapter** que traduzca nuestra interfaz a los llamados específicos del SDK del tercero.

### 5. Notificación Dinámica ante Nuevas Colaboraciones (Patrón Observer)

- **Requerimiento**: Como **colectivo**, quiero que el sistema dispare múltiples acciones configurables cuando una persona **Colaboradora** se anote en mi **Proyecto** (por ejemplo: enviar un correo automático de notificación al colectivo, registrar la postulación en un log de auditoría del administrador y generar un mensaje interno).
- **Criterio de Diseño**: **Eventos y Suscripción (Observer)**. Las acciones que se desencadenan son variables y pueden cambiar o extenderse en el tiempo (por ejemplo, añadir notificaciones por Telegram a futuro). El `Proyecto` (sujeto) no debe conocer de forma acoplada las clases concretas de envío de mails o logs. Debe conocer una lista polimórfica de observadores (`InteresadoEnColaboracion`) que entiendan un mensaje unificado y asincrónico (método con retorno `void`).

### 6. Preferencias de Alertas por Colectivo (Observer por Instancia)

- **Requerimiento**: Como **colectivo**, quiero poder configurar dinámicamente cuáles de las acciones de notificación ante nuevas colaboraciones deseo que se ejecuten y cuáles no, para evitar la saturación de alertas en mi casilla de correo.
- **Criterio de Diseño**: **Suscripción por Instancia**. Siguiendo la resolución avanzada del Observer (como en las alertas meteorológicas de "Qué me Pongo"), la lista de observadores no debe ser global del sistema. Cada instancia de `Colectivo` o de `Proyecto` debe poseer su propia colección de suscripciones (`AccionesConfigurables`), permitiendo al usuario registrar (`register`) o dar de baja (`unregister`) observadores en tiempo de ejecución de acuerdo a sus preferencias individuales.

### 7. Procesamiento Diario Automatizado de Recomendaciones (Tareas Programadas)

- **Requerimiento**: Como **colectivo**, quiero que todas las mañanas el sistema busque automáticamente a las **Colaboradoras** que tengan habilidades afines a mis proyectos activos y les envíe por correo una sugerencia de postulación para acelerar el reclutamiento.
- **Criterio de Diseño**: **Planificación Externa (Scheduled Tasks con Cron)**. El diseño de esta tarea periódica debe resolverse creando una clase ejecutable con un método `main` independiente (ej: `MainRecomendador`). Este método consultará al repositorio de proyectos y colaboradoras para disparar el cálculo de forma atómica. La periodicidad de la ejecución debe ser delegada al sistema operativo (mediante un archivo de configuración de tareas periódicas como un _Cron_ o _Crontab_), manteniendo el servidor de dominio limpio de planificaciones internas o hilos bloqueantes.

### 8. Desacoplamiento de Algoritmos de Recomendación (Dependency Injection)

- **Requerimiento**: Como **administrador**, quiero poder cambiar o alternar la lógica/algoritmo con el que se seleccionan y recomiendan los proyectos a las colaboradoras (por ejemplo, priorizar por cercanía geográfica, priorizar por proyectos con incentivo económico, o combinar ambos criterios) sin tener que modificar el código de la clase `Colaboradora`.
- **Criterio de Diseño**: **Inversión de Control e Inyección de Dependencias**. El algoritmo de recomendación de proyectos no debe estar instanciado con un `new` ni con un `Singleton` dentro de la entidad `Colaboradora`. Debe ser tratado como una estrategia polimórfica (patrón _Strategy_) que la entidad recibe externamente a través de su constructor o como parámetro del método de cálculo, facilitando la extensibilidad y la mantenibilidad del diseño.

### 9. Aislamiento y Testeo de Colaboraciones (Uso de Impostores / Mocks)

- **Requerimiento**: Como **diseñador de software (stakeholder)**, quiero poder asegurar que los tests unitarios del sistema de postulaciones funcionen de manera rápida y aislada, sin enviar correos electrónicos reales ni consumir créditos del servicio externo de geolocalización en cada ejecución.
- **Criterio de Diseño**: **Uso de Impostores (Mocking)**. Para lograr independencia y evitar efectos colaterales en los tests, se deben reemplazar las implementaciones reales de los servicios externos (como el `EnviadorDeMails` o el `ServicioDeUbicacion`) por objetos de mentira (**Mocks** o **Stubs** creados a mano o usando frameworks como _Mockito_). Estos impostores deben respetar la misma interfaz y simular un comportamiento predecible y controlado para que las aserciones del test verifiquen la lógica del negocio de forma determinística.

### 10. Evitar Clases Vacías e Identificar Atributos Calculados (Criterio de Cohesión)

- **Requerimiento**: Como **colaboradora**, quiero poder consultar en mi perfil la cantidad de horas totales que he donado a la plataforma y el tipo de colectivo en el que más he participado para construir un historial profesional robusto.
- **Criterio de Diseño (Tip de examen)**: **No duplicar datos calculables y evitar clases sin comportamiento**.
    - La cantidad de horas donadas es un **atributo calculable** en base al historial de colaboraciones finalizadas; no debe guardarse como una variable mutable persistida para evitar inconsistencias en el estado de la colaboradora.
    - Cualquier concepto del dominio (como `Habilidad`) no debe ser diseñado como una "clase de datos vacía" (sin métodos ni comportamiento). Una clase sin comportamiento es un fuerte indicador de un diseño pobre de responsabilidades (un _code smell_). Si un objeto solo guarda datos y otra clase (un "Objeto Dios") procesa todo por él, se está destruyendo el encapsulamiento y la cohesión.

---

💬 **Sugerencia de siguiente paso**: ¿Te gustaría que armemos un **diagrama de clases inicial** (en base a los requerimientos anteriores) para estructurar el modelo de dominio utilizando estos criterios antes de pasar al pseudocódigo?