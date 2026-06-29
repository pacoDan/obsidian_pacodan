### Recopilación de Preguntas y Respuestas Conceptuales Destacadas

Debido a la extensa cantidad de interacciones, se recopilan las preguntas y respuestas que definen la teoría y práctica de la materia:

- **Alumno:** ¿Cuál es el nivel al cual se conocen los hechos? **Profesor:** Es el nivel de **granularidad**, el mínimo nivel de detalle que tiene el modelo.
- **Alumno:** En el modelo Estrella, ¿por qué la tabla tiene todos los IDs repetidos si genera redundancia? **Profesor:** Porque las herramientas de BI lo resuelven mejor y permite hacer menos _joins_. Al tener todo en un solo lugar, el acceso es más rápido, aunque ocupa más espacio y tiene redundancia de datos.
- **Alumno:** ¿Por qué la tabla agregada se llena a partir de la tabla base de hechos y no directamente de los sistemas de origen? **Profesor:** Para no afectar la operatoria productiva del sistema transaccional y para garantizar que los datos sean congruentes y sumen lo mismo, evitando que se lean transacciones de más o de menos por diferencias de tiempo.
- **Alumno:** Si una dimensión no cuenta con una tabla física, ¿qué es? **Profesor:** Se la llama **dimensión degenerada**, como ocurre a menudo con el número de comprobante o ticket que se guarda directamente en la tabla de hechos.
- **Alumno:** ¿Qué pasa si quiero controlar el historial de cambios de un cliente (ej. cambio de sucursal)? **Profesor:** Debes usar una **Slowly Changing Dimension (SCD) Tipo 2**, creando una nueva clave subrogada (SK) con fechas de vigencia (desde/hasta) para no perder la historia del dato al momento de la venta.

---

### Dominio (Proceso de Negocio) y Armado del Modelo Dimensional

En la jerga de las transcripciones, el "dominio" de análisis se denomina **proceso de negocio** o **área temática**, los cuales se materializan dentro del Data Warehouse en estructuras llamadas **Datamarts**.

**Instructivo paso a paso para armar el Modelo Dimensional:**

1. **Definir el proceso de negocio a modelar:** Se debe identificar qué se busca resolver, comprendiendo las fuentes de datos y las métricas necesarias (ej. ventas, stock, logística). Es crucial escuchar al negocio antes de programar.
2. **Elegir la granularidad:** Determinar el nivel de detalle máximo al que se llegará en el modelo de datos (ej. por día, por sucursal, por producto).
3. **Definir los atributos y armar las dimensiones:** Listar todas las columnas agrupables y organizarlas jerárquicamente en dimensiones, verificando que respeten la granularidad elegida.
4. **Seleccionar los hechos:** Identificar los valores numéricos cuantitativos que se van a medir y aplicarles funciones matemáticas.

---

### Dimensiones y Atributos

- **Atributos:** Son las **columnas agrupables** que dan contexto a una transacción (ej. mes, ciudad, categoría). Proveen el nivel de detalle para las métricas.
- **Dimensiones:** Son los **agrupamientos lógicos de los atributos** organizados jerárquicamente.
- **Cómo listarlas y ejemplificar:** Se listan agrupando atributos relacionados de menor a mayor jerarquía. Por ejemplo, la dimensión _Geografía_ agrupa los atributos: sucursal $\rightarrow$ ciudad $\rightarrow$ provincia $\rightarrow$ región. La dimensión _Tiempo_ agrupa: día $\rightarrow$ mes $\rightarrow$ trimestre $\rightarrow$ año.
- **Recomendaciones de armado:**
    - **Único punto de acceso:** Toda dimensión debe relacionarse con la tabla de hechos a través de un único punto de entrada, correspondiente a su nivel más bajo (ej. sucursal, no región).
    - **Dimensión Tiempo obligatoria:** Todo modelo debe incluir obligatoriamente una dimensión de tiempo para permitir el análisis histórico.
    - **Uso de Claves Subrogadas (SK):** Es altamente recomendable generar claves propias numéricas (SK) en lugar de usar los códigos de los sistemas de origen. Esto mejora radicalmente la performance de los índices, ahorra espacio y permite manejar la historia y los cambios de códigos de los sistemas productivos.
    - **Dimensiones Conformadas:** Identificar dimensiones que se comparten entre varios Datamarts (ej. Geografía compartida entre Ventas y Stock) para permitir cruces de información.

---

### Tabla de Hechos (Fact Table)

- **Definición:** Es la tabla central que **almacena los hechos al mínimo nivel de detalle**. Son las tablas que mayor cantidad de filas tienen y ocupan más del 90% del Data Warehouse.
- **Cómo identificarla:** Se identifica porque contiene **una columna por cada hecho** a contabilizar y **una columna de clave (ID/SK) por cada atributo** que defina el nivel de granularidad del modelo.
- **Ejemplos de Hechos:** Monto de venta, unidades vendidas, saldo en pesos, minutos de llamadas, cantidad de reclamos, temperatura de un sensor.
- **Procesos de negocio que los engloban:** Los hechos se agrupan en distintos procesos de negocio representados lógicamente como **Datamarts**. Por ejemplo, los hechos _monto_ y _unidades vendidas_ se engloban en el **proceso de Ventas**; mientras que el _saldo valorizado_ y las _unidades en depósito_ se engloban en el **proceso de Stock / Inventario**.

---

### Granularidad de un Modelo

- **Qué es:** Es el **mínimo nivel de detalle** (la máxima profundidad) al que se almacenan y conocen los hechos en el modelo.
- **Para qué sirve:** Sirve para establecer los límites del análisis. Define exactamente qué preguntas de negocio el modelo es capaz de responder y determina el volumen de datos que tendrá la tabla de hechos.
- **Cómo lo uso:** Se utiliza para evitar prometer reportes imposibles de generar. Si el negocio pide un análisis de ventas por hora y la granularidad se definió por día, se usa esta definición para justificar que el modelo no contiene dicho dato.
- **Ejemplos:**
    - Granularidad del modelo de **Ventas**: A nivel de _Fecha, Sucursal, Ítem (Producto) y Cliente_.
    - Granularidad del modelo de **Stock**: A nivel de _Mes, Sucursal e Ítem_.

---

### Modelo Estrella vs. Modelo Copo de Nieve (Físico)

Una vez creado el modelo lógico dimensional, se traslada a la base de datos física utilizando principalmente dos sabores:

- **Modelo Estrella (Star Schema):**
    - Consiste en tener **una sola tabla (Lookup) por dimensión**.
    - **Ventajas:** Contiene menos tablas involucradas (ej. 5 tablas en total), lo que requiere menos _joins_ y hace que las consultas sean más veloces y fáciles de entender para el usuario.
    - **Desventajas:** Genera una alta redundancia de datos (los nombres de categorías y rubros se repiten miles de veces) y ocupa más espacio de almacenamiento.
- **Modelo Copo de Nieve (Snowflake Schema):**
    - Consiste en tener **una tabla por cada atributo jerárquico** de la dimensión (completamente normalizado).
    - **Ventajas:** Minimiza la redundancia de datos y ahorra espacio de almacenamiento, asemejándose a una cuarta forma normal.
    - **Desventajas:** Genera un gran número de tablas. Obliga al motor de base de datos a realizar múltiples _joins_ (saltos entre tablas) para consultar un dato jerárquico alto (ej. llegar desde la venta hasta la categoría del producto), lo que puede degradar el rendimiento y dificultar la comprensión del usuario final.