Ambos conceptos son atributos de calidad fundamentales y están fuertemente contemplados tanto en el estándar IEEE 830 para la Especificación de Requerimientos de Software (ERS) como en los requerimientos específicos de tu proyecto **FarmaTracker**.

Aquí te detallo cómo se abordan ambos aspectos basándonos en la documentación:

### 1. Seguridad

Según el estándar de la ERS, la seguridad especifica los elementos de protección contra accesos accidentales o maliciosos para usar, modificar, destruir o revelar información. Esto abarca desde el uso de técnicas criptográficas y el mantenimiento de un historial de transacciones, hasta la verificación de la integridad de los datos críticos y la asignación de funciones a diferentes módulos.

En el diseño específico de **FarmaTracker**, la seguridad se aborda en distintos frentes para garantizar el cumplimiento normativo (RNF04):

- **Control de Accesos por Roles:** El sistema contempla niveles de acceso estrictamente diferenciados. El rol de "Director Técnico" tiene privilegios administrativos para la gestión estratégica (aprobar devoluciones, traslados o destrucciones), mientras que el "Empleado" tiene un acceso limitado a la ejecución operativa de las auditorías en el salón.
- **Log de Auditoría (Trazabilidad):** Como medida de seguridad e integridad documental, cualquier cambio de estado en una incidencia (quién, cuándo y qué acción se tomó) debe quedar registrado obligatoriamente en un log de auditoría. Además, el sistema bloquea transacciones simultáneas si dos usuarios intentan resolver la misma incidencia al mismo tiempo.
- **Seguridad a nivel Infraestructura:** La arquitectura del sistema delega el acceso externo a través de **Cloudflare**, que actúa como un proxy inverso. Esta decisión no solo simplifica el DNS, sino que añade una capa de protección esencial contra bots y ataques a nivel de infraestructura.

### 2. Usabilidad (Experiencia de Usuario)

La usabilidad determina la facilidad con la que las personas interactúan con el software. En la estructura de la carpeta final del proyecto exigida por la cátedra, este atributo cuenta con una sección propia ("Experiencia de Usuario"), en la cual se deben incluir prototipos de navegación y _wireframes_ o _mockups_ visuales.

Para **FarmaTracker**, la usabilidad es un requerimiento no funcional crítico, dado que el sistema se usará dinámicamente durante el horario de atención al público. Se establecieron las siguientes restricciones para garantizarla:

- **Adaptabilidad Móvil (Diseño Responsive):** La interfaz de la "Hoja de Ruta" debe poder ejecutarse de forma legible y cómoda en las pantallas de dispositivos móviles (como teléfonos de 5 pulgadas), ya que el empleado la utilizará mientras camina por los pasillos.
- **Regla de Interacciones Mínimas:** Para no ralentizar el trabajo físico de auditoría en la góndola, el diseño exige que confirmar el estado de un lote no requiera más de tres interacciones (clics o _taps_) en la pantalla.

En resumen, la **seguridad** en tu proyecto blinda el sistema contra ataques externos y asegura que las decisiones legales/comerciales recaigan exclusivamente sobre el profesional a cargo, mientras que la **usabilidad** está enfocada en la agilidad operativa, garantizando que el empleado de mostrador pueda realizar sus controles de inventario de forma rápida y sin fricciones tecnológicas.


