Aquí tienes una propuesta completa de _speech_ (guion) para la parte técnica, de análisis, diseño y arquitectura de **FarmaTracker**. Está redactado en un tono profesional, persuasivo y directo, ideal para que los miembros con roles técnicos (como el Analista de Sistemas, el Líder Técnico y el Arquitecto) lo utilicen en la presentación ante la cátedra y los inversores.

---

### Inicio del Speech Técnico

"Buenas tardes. Pasando a la dimensión técnica de FarmaTracker, queremos detallar cómo transformamos la necesidad del negocio en una solución de software robusta, escalable y segura.

**1. Metodología de Análisis y Diseño** Para sentar unas bases sólidas, abordamos la fase de análisis y diseño siguiendo los lineamientos del **Proceso Unificado**. Todo el relevamiento de los requerimientos funcionales y no funcionales fue documentado rigurosamente bajo el **estándar IEEE 830-1993**. A partir de esto, modelamos el comportamiento y la estructura del sistema utilizando **UML**, lo que nos permitió generar artefactos clave como el Diagrama de Clases, Diagramas de Secuencia para los flujos operativos, Diagramas de Estado para el ciclo de vida del medicamento y el Modelo de Datos (DER).

**2. Arquitectura de Dominio y Lógica Central (FEFO)** El corazón intelectual de nuestro sistema reside en su **Arquitectura de Dominio**. Separamos conceptualmente los datos estáticos, como las sucursales y los medicamentos, de la evaluación dinámica del riesgo. Para esto construimos un **Motor de Reglas** automatizado que implementa el **algoritmo FEFO** _(First Expired, First Out - Primero en Vencer, Primero en Salir)_.

Este motor interactúa constantemente con el universo de lotes en góndola. Cuando detecta que un lote atraviesa una ventana de tiempo crítica según los convenios del laboratorio, no toma una acción directa, sino que genera automáticamente una **Incidencia**. Esto es vital: el sistema calcula el riesgo, pero delega la decisión comercial o logística final (como una Devolución, un Traslado o una Destrucción) a la validación exclusiva del usuario con rol de **Director Técnico**.

**3. Atributos de Calidad: Usabilidad, Seguridad y Trazabilidad** Entendimos que el software se usaría en un entorno de alta dinámica como es el salón de una farmacia. Por ello, definimos Requerimientos No Funcionales muy estrictos:

- **Usabilidad Extrema:** Diseñamos la "Hoja de Ruta" logística para dispositivos móviles bajo la estricta **'regla de los 3 clics'**. El empleado que recorre la góndola no debe realizar más de tres interacciones en la pantalla para confirmar el estado de un lote, garantizando así la agilidad operativa.
- **Rendimiento:** Nuestro motor de reglas está diseñado para evaluar el inventario y sugerir una resolución en apenas segundos.
- **Seguridad y Trazabilidad (Compliance legal):** Implementamos un control de accesos estricto separando los roles de `Director Técnico` y `Empleado`. Además, toda acción crítica ejecutada sobre un medicamento genera un **log de auditoría** inmutable y un manifiesto en PDF para cumplir con las rigurosas normativas de trazabilidad de la ANMAT.

**4. Arquitectura de Software y Estrategia de Despliegue** Yendo a la arquitectura de software pura, implementamos un **modelo web clásico de tres capas**.

- En la **Capa de Presentación (Frontend)** optamos por **React** debido a su agilidad de desarrollo y su potente ecosistema para construir componentes dinámicos y Dashboards.
- En la **Capa de Lógica de Negocio (Backend)** utilizamos **Spring Boot (Java)**. Necesitábamos un framework maduro, estable y robusto para soportar la pesada carga de procesamiento matemático de nuestro Motor de Reglas.
- En la **Capa de Datos**, la persistencia y la integridad referencial recaen sobre una base de datos relacional **PostgreSQL**.

Finalmente, **una de las decisiones arquitectónicas más importantes que tomamos fue nuestra estrategia de infraestructura**. En lugar de ir hacia un modelo 100% Cloud altamente elástico, decidimos realizar el **despliegue en un servidor on-premise**.

¿Por qué? Porque asumimos un **Trade-Off (costo/beneficio)** consciente y alineado al negocio. FarmaTracker es un producto SaaS B2B orientado a farmacias, lo que significa que tenemos un volumen de usuarios concurrentes limitado y sumamente predecible. Al alojarlo on-premise evitamos la complejidad operativa y los costos recurrentes y variables de la nube. Para mitigar la falta de herramientas nativas de la nube y proteger esta infraestructura, implementamos **Cloudflare** como proxy inverso. Esta capa intermedia resuelve de manera económica la gestión de los DNS y nos brinda un escudo de ciberseguridad fundamental contra ataques de denegación de servicio (DDoS) y tráfico de bots maliciosos.

En conclusión, entregamos una arquitectura que es financieramente viable, técnicamente robusta en el procesamiento de reglas, y logísticamente ágil para el usuario final."

