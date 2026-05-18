Esta estructura organiza los conceptos clave de la clase de Business Intelligence, diseñada para que analistas e ingenieros puedan aplicar estos principios en la resolución de problemas de modelado de datos.

### 1. Claves Subrogadas (Surrogate Keys - SK)

Las **claves subrogadas** son identificadores únicos internos del Data Warehouse que se despegan de los sistemas de origen.

- **Propósito:** Solucionan problemas de **reutilización de códigos** en el negocio, donde un mismo ID puede representar productos distintos a lo largo del tiempo.
- **Performance:** Es recomendable que sean de tipo **entero (numeric)** para mejorar el rendimiento de los índices y búsquedas en bases de datos con millones de registros, evitando el uso de claves largas o alfanuméricas de los sistemas operacionales.
- **Unificación:** Permiten crear una dimensión única cuando existen múltiples sistemas de origen con codificaciones diferentes para el mismo concepto.

### 2. Subdimensiones y Normalización

Se aplica la técnica de **subdimensión** para reducir la redundancia y mejorar el mantenimiento de los datos.

- **Proceso:** Consiste en extraer atributos de una dimensión con mucha redundancia (ej. datos de país en una tabla de 10 millones de clientes) y crear una tabla relacionada.
- **Impacto:** Este proceso convierte un modelo de **estrella en uno de copo de nieve** sin cambiar la granularidad de la tabla de hechos (Fact Table).

### 3. Dimensiones de Cambio Lento (Slowly Changing Dimensions - SCD)

Define cómo el modelo reacciona ante cambios en los atributos descriptivos para preservar o no la historia.

- **Tipo 0:** No se realiza ninguna acción; se mantiene el dato tal cual se cargó originalmente.
- **Tipo 1:** Se sobrescribe el registro; se mantiene solo el último estado y **se pierde la historia** anterior.
- **Tipo 2:** Es la más utilizada; se genera un **nuevo registro con una nueva SK** para cada cambio. Se utilizan campos de **vigencia (desde/hasta)**, versiones o banderas de "actual" para identificar el registro vigente.
- **Tipo 3:** Se añaden columnas adicionales en el mismo registro para guardar el "valor anterior" y la fecha del cambio, permitiendo ver una historia parcial.

### 4. Dimensiones Especiales y Optimizaciones

- **Monster Dimensions:** Son dimensiones que cambian muy rápido (ej. puntajes de crédito diarios), lo que genera un crecimiento exponencial de registros si se usa SCD Tipo 2. Una solución es separar los atributos volátiles de los estáticos.
- **Dimensiones Degeneradas:** Atributos que se dejan directamente en la Fact Table sin una tabla de dimensión asociada, como números de comprobante o de transacción, para evitar joins innecesarios con tablas de gran volumen.
- **Junk Dimensions:** Agrupación de múltiples atributos de baja cardinalidad (indicadores boleanoss, flags de encuestas) en una única dimensión de "varios" para simplificar el modelo.
- **Factless Fact Tables (Tablas de hechos sin hechos):** Se utilizan para modelar eventos o ocurrencias (ej. asistencia a clase) donde no hay una medida numérica clara. Se recomienda añadir una columna con valor constante "1" (ej. "asistió") para facilitar cálculos de suma y promedio.

---

### Recopilación de Preguntas y Desafíos

A continuación, se listan las preguntas planteadas durante la clase para fomentar el análisis:

**Preguntas de Diagnóstico y Concepto:**

1. **Performance:** ¿Qué sucede si utilizas un código de 22 dígitos como clave primaria en una base de datos con millones de transacciones?.
2. **Integridad:** Ante un cambio de producto (ej. de Pepsi a Manaos) con el mismo código original, ¿cómo afecta esto a los reportes históricos si no hay una clave subrogada?.
3. **Arquitectura:** En una subdimensión, ¿cambia la granularidad del modelo al pasar de estrella a copo de nieve?.
4. **Lógica de Negocio:** ¿Quién debe definir qué hacer con la historia cuando se detecta un cambio en un dato?.
5. **Diseño:** ¿En qué casos tiene sentido dejar una marca de "último registro" o un número de versión en lugar de solo fechas de vigencia en SCD Tipo 2?.

**Desafíos para Resolver (Ejercicios):** 6. **Navegación:** ¿Cuál es el problema técnico de "navegar a través de la Fact Table" al separar una dimensión monstruosa en dos?. 7. **Cálculo:** ¿Por qué es recomendable que la columna auxiliar en una _Factless Fact Table_ sea de tipo decimal y no entero?.

**Preguntas de Investigación (Tarea):** 8. **Dimensión de Tiempo:** Si la fecha fuera autoincremental (1 para el 1 de enero, 2 para el 2 de enero, etc.), ¿qué beneficios o contras encontrarías?. 9. **Generación de IDs:** ¿Qué otros métodos existen para generar claves primarias o SK que no sean campos autoincrementales, especialmente en entornos de Big Data o Data Lakehouses donde estos no existen?.



---

#### **Preguntas de Diagnóstico y Concepto**

1. **Performance: ¿Qué sucede si utilizas un código de 22 dígitos como clave primaria en una base de datos con millones de transacciones?**
    - **Respuesta:** El uso de claves alfanuméricas largas (como códigos de 22 dígitos de sistemas origen) hace que los índices sean "pesadísimos" y ocupen mucho espacio en disco. Esto degrada significativamente el rendimiento de las búsquedas y el _tuning_ de la base de datos. En comparación, una clave subrogada (SK) de tipo entero puede ser hasta 100 veces más eficiente en términos de memoria y velocidad de procesamiento.
2. **Integridad: Ante un cambio de producto (ej. de Pepsi a Manaos) con el mismo código original, ¿cómo afecta esto a los reportes históricos si no hay una clave subrogada?**
    - **Respuesta:** Se produce una pérdida de integridad histórica. Al reutilizar el código (ej. 678), cualquier reporte actual mostrará las ventas antiguas de Pepsi como si fueran de Manaos, ya que el código ahora referencia la nueva descripción. Sin una SK que identifique de forma única cada versión del producto, el sistema no tiene forma de distinguir qué se vendió antes del cambio.
3. **Arquitectura: En una subdimensión, ¿cambia la granularidad del modelo al pasar de estrella a copo de nieve?**
    - **Respuesta:** No, la **granularidad no cambia**. Aunque se agregue una tabla adicional para reducir la redundancia (ej. extraer datos de país de una tabla de clientes), la tabla de hechos (_Fact Table_) sigue siendo la misma y no requiere ser recargada.
4. **Lógica de Negocio: ¿Quién debe definir qué hacer con la historia cuando se detecta un cambio en un dato?**
    - **Respuesta:** El **cliente o usuario de negocio** es quien define la lógica. Aunque el analista puede sugerir técnicas, el negocio es quien conoce la utilidad de la historia y debe hacerse cargo de la decisión, ya que ellos usarán los datos para reportes o modelos de _machine learning_.
5. **Diseño: ¿En qué casos tiene sentido dejar una marca de "último registro" o un número de versión en lugar de solo fechas de vigencia en SCD Tipo 2?**
    - **Respuesta:** Se utiliza para facilitar las consultas de "estado actual". Al tener un campo de versión o un booleano de "activo/último", se evita que el analista tenga que buscar el `MAX(fecha)` o filtrar por rangos de fechas complejos, permitiendo traer el registro vigente de forma directa y rápida.

#### **Desafíos para Resolver (Ejercicios)**

6. **Navegación: ¿Cuál es el problema técnico de "navegar a través de la Fact Table" al separar una dimensión monstruosa en dos?**
    - **Respuesta:** El problema principal es que se pierde la relación directa entre atributos si no hay una transacción de por medio. Si se separa la demografía del cliente de sus datos básicos, para saber qué nivel educativo tiene un cliente, hay que hacer un _join_ pasando por la tabla de hechos. Si ese cliente **no ha realizado compras**, la información demográfica se vuelve inalcanzable a través del modelo, perdiendo visibilidad sobre clientes potenciales.
7. **Cálculo: ¿Por qué es recomendable que la columna auxiliar en una _Factless Fact Table_ sea de tipo decimal y no entero?**
    - **Respuesta:** Debido a cómo operan los motores de SQL; si se realiza un promedio (`AVG`) sobre una columna de tipo entero, el resultado se truncará a un entero, perdiendo precisión (ej. 1.1 o 1.5 se mostrarían como 1). Al usar decimales, se aseguran cálculos exactos en operaciones de promedio y sumas complejas.

---

### II. Lecciones Aprendidas y Detalles Técnicos

- **Claves Subrogadas (SK):** Son claves internas del Data Warehouse que nos despegan de los sistemas de origen. Son vitales para manejar la **reutilización de códigos** y para unificar datos de múltiples sistemas que usan distintas codificaciones para un mismo producto (ej. "Coca", "Coca-Cola 600", "Coca-Cola").
- **Subdimensiones:** Se utilizan para evitar la **redundancia masiva** en dimensiones grandes. Ejemplo: En una tabla de 10 millones de clientes donde 9 millones son de Argentina, es mejor crear una subdimensión de "País" para no repetir datos estáticos (superficie, presidente) 9 millones de veces y evitar _updates_ masivos cuando cambie un dato nacional.
- **Slowly Changing Dimensions (SCD):**
    - **Tipo 0:** El dato es estático (ej. nombres de provincias).
    - **Tipo 1:** Sobrescribe el valor; es fácil de mantener pero **pierde la historia**.
    - **Tipo 2:** Crea un nuevo SK para el cambio, manteniendo fechas de vigencia (Desde/Hasta). Es la técnica estándar para guardar historia detallada.
    - **Tipo 3:** Guarda solo el valor actual y el anterior en el mismo registro, permitiendo una visión histórica limitada.
- **Dimensiones Degeneradas:** Son atributos (como números de factura o comprobante) que se dejan directamente en la _Fact Table_ porque crear una tabla de dimensión para ellos no tiene sentido debido a su alta cardinalidad (millones de registros únicos).
- **Junk Dimensions:** Agrupan múltiples indicadores pequeños (flags de Sí/No) en una sola tabla de combinatorias para evitar llenar el modelo de pequeñas tablas innecesarias.

---

### III. Tarea de Investigación

8. **Dimensión de Tiempo:** Se debe analizar si una fecha autoincremental (ID 1, 2, 3...) aporta beneficios o contras frente al uso de una clave inteligente basada en la fecha (AAAAMMDD).
9. **Generación de IDs en Entornos Modernos:** En sistemas de Big Data o _Lakehouses_ basados en archivos (como Parquet), el concepto de "campo autoincremental" tradicional de SQL no existe. Es necesario investigar otros métodos para generar Primary Keys únicas en estos entornos.