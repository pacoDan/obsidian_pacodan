Para profundizar en la **arquitectura y el diseño de sistemas y software** de tu proyecto (FarmaTracker), y orientarlo especialmente a la audiencia técnica (los arquitectos), podemos desglosar esta sección utilizando los lineamientos del documento de arquitectura y la Especificación de Requerimientos (ERS IEEE 830) exigidos por la cátedra.

Aquí tienes una ampliación detallada de cómo estructurar y explicar la arquitectura de tu solución:

### 1. Patrón Arquitectónico y Distribución en Capas

Según los estándares de la cátedra, el diagrama de arquitectura debe mostrar cómo los componentes se distribuyen entre las capas y cómo se comunican entre sí. Para FarmaTracker, se ha definido una **arquitectura web clásica de tres capas**:

- **Capa de Presentación (Frontend):** Desarrollada con **React**. Se eligió por su rapidez de desarrollo y su amplio ecosistema de librerías para la construcción de interfaces y visualización de datos (Dashboards).
- **Capa de Lógica de Negocio (Backend):** Desarrollada con **Spring Boot** (Java). Esta capa es fundamental ya que aloja el "Motor de Reglas" automatizado y las APIs que procesan la criticidad de los vencimientos de forma robusta y estable.
- **Capa de Datos:** Utiliza **PostgreSQL** como motor de base de datos relacional para garantizar la integridad y trazabilidad del historial de lotes.

### 2. Infraestructura Tecnológica y Estrategia de Despliegue

El enfoque de infraestructura debe justificar las decisiones de diseño en base al modelo de negocio:

- **Despliegue On-Premise:** El sistema se aloja en un único servidor físico. Esta decisión está alineada con su modelo de negocio B2B (Software as a Service para farmacias), donde la concurrencia masiva de usuarios no es un problema crítico, permitiendo así reducir la complejidad operativa y los costos recurrentes que implicaría un entorno 100% Cloud escalable.
- **Proxy Inverso y Seguridad:** Se implementa **Cloudflare** como capa intermedia frente al servidor. Esto resuelve la gestión de DNS y proporciona protección esencial contra ataques (DDoS) y tráfico de bots maliciosos, a un bajo costo.

### 3. Modelado de Diseño (Estático y Dinámico)

El diseño del software se sostiene sobre artefactos y modelos UML clave que los arquitectos evaluarán:

- **Modelo de Datos (DER):** Diagrama Entidad-Relación y diccionario de datos que soportan la estructura de información de lotes, medicamentos y proveedores.
- **Modelo Estático:** Diagrama de Clases que estructura los objetos centrales del sistema (Sucursal, Medicamento, Lote, Motor de Reglas, Incidencia y Acción Correctiva).
- **Modelo Dinámico:** Se destacan los **Diagramas de Estado** (cruciales para modelar el ciclo de vida del medicamento y la transición de la _Incidencia_ de "Pendiente" a "Resuelta") y los **Diagramas de Secuencia** para ilustrar los flujos de auditoría.

### 4. Atributos del Software y Restricciones (Requerimientos No Funcionales)

Basado en el estándar IEEE 830, el diseño de la arquitectura debe satisfacer requerimientos críticos de calidad:

- **Seguridad y Accesos:** La arquitectura incorpora un control estricto de roles. El rol `DUENIO` (Director Técnico) tiene permisos de ejecución estratégica sobre las incidencias, mientras que el rol `EMPLEADO` tiene su alcance limitado a la gestión de auditoría en la góndola.
- **Rendimiento (Performance):** El motor de reglas está diseñado para procesar el universo de lotes y sugerir las resoluciones en un tiempo de respuesta de pocos segundos.
- **Integridad y Trazabilidad:** La base de datos garantiza que no existan lotes "huérfanos" (sin medicamento/laboratorio asociado) y el backend asegura que toda modificación de estado quede registrada en un _log de auditoría_ para cumplir con las normativas (como las de la ANMAT).

**Consejo para la presentación:** Cuando hables con los arquitectos, enfócate en el **"Trade-off" (costo/beneficio)** de tus decisiones. Por ejemplo, mencionar que _sacrificaron elasticidad extrema en la nube a cambio de un costo operativo predecible y bajo (on-premise + Cloudflare)_ demuestra un alto nivel de madurez en el diseño de arquitectura y visión de negocio.