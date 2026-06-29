De acuerdo con los estándares de la cátedra (basados en la norma IEEE 830) y el Acta de tu proyecto **FarmaTracker**, los requerimientos se dividen claramente en funcionales (lo que el sistema debe _hacer_) y no funcionales (cómo debe _comportarse_ el sistema).

A continuación, te detallo las definiciones formales y cómo aplican específicamente a tu proyecto:

### 1. Requerimientos Funcionales

Según el estándar IEEE 830, los requerimientos funcionales **definen las acciones fundamentales que se deben realizar en el software** para aceptar entradas, procesarlas y generar las salidas correspondientes.

En el caso de **FarmaTracker**, se relevaron 13 Requerimientos Funcionales (RF) específicos para cumplir con el alcance del negocio:

- **Accesos y Roles (RF01, RF02, RF03):** El sistema exige autenticación obligatoria y clasifica a los usuarios estrictamente en dos roles (`Director Técnico` y `EMPLEADO`), ofreciendo un menú principal distinto según los permisos de cada uno.
- **Gestión del Motor y Alertas (RF04, RF06, RF08, RF13):** Permite configurar el motor de reglas paramétricas (ej. días para vencer). El sistema debe evaluar continuamente estas reglas, **generar alertas de vencimiento automáticamente**, despachar notificaciones a los usuarios afectados y permitir la gestión de aquellas alertas que el Director Técnico decida "postergar".
- **Control Físico y Logístico (RF05, RF10, RF11, RF12):** Facilita la visualización de los lotes organizados por su ubicación física en la sucursal, la gestión unificada de medicamentos y la posibilidad de marcar un lote como "agotado".
- **Reportes y Trazabilidad Legal (RF07, RF09):** Exige la existencia de un **dashboard de gestión logística gerencial** (para visualizar KPIs como ahorro operativo y criticidad por laboratorio) y la **generación de documentos en PDF** (manifiestos de traslado o actas de destrucción) cuando se resuelve una incidencia.

### 2. Requerimientos No Funcionales (Atributos de Calidad y Rendimiento)

El estándar indica que los requerimientos no funcionales definen los atributos estáticos y dinámicos del software, imponiendo **restricciones de diseño** para garantizar características vitales como la confiabilidad, el rendimiento, la disponibilidad, la seguridad y la portabilidad.

Para que **FarmaTracker** sea exitoso en un entorno real de farmacia, estableciste las siguientes restricciones (RNF):

- **Usabilidad y Adaptabilidad:** Como el empleado utilizará la plataforma recorriendo la góndola, la interfaz de la "Hoja de Ruta" debe ser completamente adaptable a dispositivos móviles. Además, **no debe requerir más de tres interacciones (clics o taps)** para confirmar el estado de un lote, asegurando agilidad operativa.
- **Rendimiento (Performance):** El motor de reglas está exigido a procesar todo el universo de lotes y emitir la resolución sugerida en **pocos segundos**, sin bloquear la operatoria de la sucursal.
- **Integridad de Datos:** Es obligatorio garantizar la trazabilidad total, impidiendo lógicamente la existencia de lotes "huérfanos" (que no estén vinculados previamente a un medicamento y un laboratorio válido).
- **Seguridad de Acceso:** El diseño de la arquitectura blinda las operaciones al separar los niveles de acceso. Las decisiones críticas (estrategia y resolución de mermas) solo pueden ser ejecutadas por el Director Técnico, mientras que el empleado de salón está restringido únicamente a tareas de auditoría física.