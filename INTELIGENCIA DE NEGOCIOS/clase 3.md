
### 1. Definición del Esquema (Estructura del Modelo)

Al diseñar o resolver un ejercicio, lo primero es decidir cómo se organizarán las tablas de dimensiones (Lookups).

- **Modelo Estrella (Star Schema):**
    - **Estructura:** Posee una única tabla de dimensión (Lookup) por cada dimensión del negocio.
    - **Ventajas:** Menor cantidad de _joins_, consultas más rápidas y simetría en el acceso a los datos.
    - **Desventaja:** Genera redundancia de datos (desnormalización).
- **Modelo Copo de Nieve (Snowflake Schema):**
    - **Estructura:** Posee una tabla por cada atributo de la dimensión, normalizando las jerarquías.
    - **Ventajas:** Ahorro de espacio y mayor facilidad para cargar datos atómicos.
    - **Desventaja:** Requiere múltiples _joins_ para llegar a los niveles superiores de la jerarquía (ej. de ciudad a provincia), lo que puede afectar la performance.

### 2. Tipos de Tablas en el Modelo Físico

Para construir el modelo, debes identificar qué función cumple cada tabla:

- **Tablas Lookup (Maestras):** Almacenan los elementos de un atributo (instancias como "Nombre del Cliente"). Deben contener una columna por cada **Attribute Form** (ID, descripción) y, en modelos copo de nieve, una columna por cada padre para establecer la relación jerárquica.
- **Tablas de Hechos (Fact Tables):**
    - **Tabla Base:** Almacena los hechos al **mínimo nivel de detalle** definido (granularidad). Se alimenta directamente de los sistemas orígenes (OLTP).
    - **Tabla Agregada:** Son resúmenes creados a partir de la Tabla Base para reducir tiempos de respuesta en consultas frecuentes. Se calculan desde el Data Warehouse, no desde el origen, para garantizar consistencia.
- **Tablas de Relación:** Se utilizan exclusivamente para resolver relaciones de **muchos a muchos** (M:M) entre atributos que no tienen una jerarquía lineal (ej. vendedores que trabajan en múltiples sucursales).

### 3. Clasificación de Hechos (Métricas)

Al resolver ejercicios de cálculo o creación de medidas, es vital identificar el tipo de hecho para saber qué funciones matemáticas aplicar:

- **Aditivos:** Tienen sentido sumarse a través de todas las dimensiones (ej. Ventas, Unidades).
- **Semi-aditivos:** Tienen sentido sumarse para algunas dimensiones pero no para todas (ej. El balance de una cuenta tiene sentido por cuenta, pero no sumar los balances de todos los días del año).
- **No aditivos:** No tienen sentido sumarse; suelen ser porcentajes o fotos fijas (ej. margen de ganancia, avance de un proyecto).

### 4. Guía de Pasos para Resolver Ejercicios de Modelado

Si se te presenta un caso de negocio para modelar, sigue esta secuencia lógica:

1. **Determinar la Granularidad:** Identifica cuál es el nivel mínimo de detalle de la Tabla de Hechos (ej. venta por producto, por día, por empleado).
2. **Identificar Dimensiones y Jerarquías:** Define qué tablas Lookup necesitas y si existen relaciones jerárquicas (ej. Ciudad -> Provincia -> País).
3. **Seleccionar el Nivel de Normalización:**
    - **Completamente Normalizado:** Máxima división de tablas, mínima redundancia.
    - **Moderadamente Normalizado:** Los hijos arrastran IDs de los ancestros para evitar _joins_ excesivos.
    - **Completamente Desnormalizado:** Una sola tabla por dimensión con todos los atributos (estrella pura).
4. **Definir Claves:** Aunque el origen use IDs de sistema, considera el uso de **Claves Surrogadas** (claves internas del modelo) para mejorar la performance y el mantenimiento a largo plazo.
5. **Evaluar Necesidad de Agregadas:** Si el volumen de datos de la Tabla Base es masivo, diseña tablas agregadas para los reportes de alto nivel (ej. ventas mensuales).

**Nota importante fuera de las fuentes:** Para analistas que utilizan herramientas específicas, recuerda que la elección del modelo a veces depende del software: **Power BI** suele preferir modelos estrella, mientras que herramientas como **Microstrategy** o **Tableau** pueden manejar eficientemente copos de nieve.


Esta estructura organiza los contenidos de la clase sobre **Modelado Físico en Business Intelligence**, integrando las definiciones teóricas con las respuestas a las preguntas planteadas en la transcripción, ejemplos prácticos y lecciones aprendidas.

---

### 1. Fundamentos del Modelado Físico

**¿Qué es el modelado físico?** Es el proceso de pasar un modelo dimensional, conceptual o multidimensional a un esquema físico real compuesto por **tablas, filas y campos**. El objetivo final es obtener una estructura de tablas y columnas optimizada para bases de datos relacionales, que es donde funcionan la gran mayoría de los Data Warehouses actuales.

**Componentes Principales:**

- **Tablas Lookup (Maestras):** Almacenan los elementos de un atributo (ej. nombres de clientes).
- **Tablas de Hechos (Fact):** Se dividen en **Base** (mínimo nivel de detalle) y **Agregadas** (resúmenes).
- **Tablas de Relación:** Se usan en casos especiales de relaciones muchos a muchos (M:M).

---

### 2. Análisis de Esquemas: Estrella vs. Copo de Nieve

La elección del esquema determina la complejidad y performance del modelo.

|Característica|Modelo Estrella (Star)|Modelo Copo de Nieve (Snowflake)|
|:--|:--|:--|
|**Estructura**|Una tabla por cada **dimensión**.|Una tabla por cada **atributo**.|
|**Cantidad de Tablas**|Menor (ej. 5 tablas para una dimensión frente a 16 en copo de nieve).|Mayor cantidad de tablas por la normalización de jerarquías.|
|**Joins**|Menos joins; acceso directo a la información.|Más joins necesarios para navegar la jerarquía (ej. Ciudad -> Provincia -> País).|
|**Redundancia**|Alta redundancia de datos (desnormalizado).|Mínima redundancia; ahorro de espacio.|
|**Performance**|Más rápido en lecturas generales; simétrico.|Puede ser más lento por los joins, pero más rápido para cargar datos atómicos.|

**¿Qué son los "Attribute Forms"?** Son los componentes o atributos descriptivos de un elemento. Por ejemplo, para el atributo "Cliente", sus _attribute forms_ serían el ID, la Razón Social, la Dirección y el Representante.

---

### 3. Lógica de Modelado y Estrategias de Desnormalización

**¿Qué impacto tiene un join menos?** Aunque parece poco, las bases de datos tienen límites finitos de joins (frecuentemente entre 16 y 32). En modelos complejos con 20 dimensiones y cálculos pesados, reducir la cantidad de joins mediante la desnormalización puede ser la diferencia entre un reporte instantáneo y uno que tarda minutos.

**Niveles de Normalización en Copo de Nieve:**

1. **Completamente Normalizado:** Mínima redundancia; requiere muchos joins para llegar de hijos a abuelos.
2. **Moderadamente Normalizado:** Los hijos arrastran los IDs de todos sus ancestros (padres, abuelos), reduciendo joins pero introduciendo algo de redundancia controlada.
3. **Completamente Desnormalizado:** Los hijos arrastran tanto los IDs como las descripciones de sus ancestros. Esto lo convierte funcionalmente en un **Modelo Estrella**.

---

### 4. Tablas de Hecho (Fact Tables) y Carga de Datos

**¿Qué es la Granularidad?** Es el **mínimo nivel de detalle** definido para los hechos. Por ejemplo, en una tabla de ventas, la granularidad podría ser "Venta por producto, por día, por sucursal y por empleado".

**Lógica de Carga (Base vs. Agregada):**

- **Tabla Base:** Se llena directamente desde los sistemas **OLTP (orígenes)**. Si el origen tiene facturas detalladas pero el modelo no requiere el número de factura, los datos se agrupan al nivel de granularidad definido durante la carga.
- **Tablas Agregadas:** Son resúmenes (ej. ventas por mes) creados para acelerar consultas de alto nivel.
    - **¿Por qué se cargan desde la Fact Base y no desde el origen?** Para garantizar que los totales coincidan (consistencia) y porque los datos en la tabla base ya pasaron por procesos de limpieza y filtrado.

---

### 5. Tipos de Hechos (Métricas para Examen)

Es vital clasificar los hechos para saber qué operaciones matemáticas son válidas:

- **Aditivos:** Se pueden sumar en todas las dimensiones (ej. Ventas, Unidades).
- **Semi-aditivos:** Se pueden sumar en algunas dimensiones pero no en otras (ej. El **Balance** de una cuenta se puede sumar por cuenta en un día, pero no sumar los balances de todos los días del año).
- **No aditivos:** No tiene sentido sumarlos; suelen ser porcentajes o estados (ej. Margen de ganancia, **Avance de un proyecto**).

---

### 6. Consultas Técnicas de Alumnos y Respuestas

- **Relaciones Jerárquicas:** Una tabla "hijo" (ej. Ciudad) debe tener una columna para el ID de su "padre" (ej. Provincia) para poder vincularlas en el modelo copo de nieve.
- **Hechos en Filas vs. Columnas:** Se pueden tener métricas (Ventas y Compras) como columnas separadas o usar una dimensión adicional para identificar el tipo de hecho en una sola columna de montos. Ambas son válidas; la elección depende de la facilidad para el usuario final y la performance de carga.
- **Performance de Agregadas:** Aunque crear una agregada consume tiempo de procesamiento durante la carga, este se realiza en horarios marginales (noche) para que el usuario final tenga una respuesta instantánea durante el día.

---

### 7. Lecciones Aprendidas (Resumen Crítico)

1. **Flexibilidad del Modelo Dimensional:** Es fácil agregar nuevos hechos o dimensiones siempre que sean compatibles con la granularidad definida.
2. **La "Guerra" de Herramientas:** La elección del modelo físico a veces viene impuesta por la herramienta de visualización (ej. **Power BI** prefiere Estrella, mientras que **Microstrategy** prefiere Copo de Nieve).
3. **Clave Surrogada:** Es una clave interna creada por el modelador (no viene del origen) que garantiza la performance y la integridad del modelo a largo plazo.
4. **Consistencia:** Nunca cargar agregadas desde el origen; siempre desde la Fact Base para evitar discrepancias por transacciones en tiempo real.

---

### 8. Logística de la Cursada

- **Equipos:** Máximo de 4 personas para asegurar la participación de todos.
- **Clases Prácticas:** Se recomienda llevar **hojas en blanco** en lugar de computadoras para los ejercicios de diseño manual.
- **Software:** Se utilizará principalmente **Power BI en la nube** para la parte práctica del tablero.
- **Examen Parcial:** Programado para el 23 de junio.



