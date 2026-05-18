**I. Introducción y Aspectos Administrativos**

- **Complejidad de la materia:** Se enfoca más en lo **funcional y conceptual** que en lo netamente técnico.
- **Objetivo principal:** Comprender y aplicar el **modelo multidimensional** y la arquitectura de proyectos de BI.
- **Cronograma y novedades:**
    - Aviso de inasistencia del profesor el 21 de abril y propuesta de recuperación.
    - Primera clase presencial práctica el 28 de abril.
    - Disponibilidad de material (guía de ejercicios y clases grabadas) en el aula virtual.

**II. Fundamentos del Modelado Dimensional**

- **Definición:** Técnica de desarrollo basada en estándares definidos por **Kimball e Inmon** en los años 90.
- **Propósito:** Crear modelos **intuitivos, escalables y performantes** para el acceso a datos en bases relacionales.
- **El proceso de BI:**
    - **Primera etapa (Relevamiento):** Generación del modelo multidimensional lógico (análogo a un DFD o DER) para plasmar las entidades de negocio.
    - **Segunda etapa (Físico):** Transformación del modelo lógico a un modelo físico en un **Data Warehouse**.

**III. Componentes del Modelo Dimensional**

- **Hechos (Facts / Indicadores):**
    - Columnas **agregables** de tipo numérico (sumas, promedios, máximos, mínimos).
    - Representan los valores **cuantitativos** de una transacción.
    - Ejemplos: Monto de venta, unidades vendidas, minutos hablados, temperatura.
- **Atributos:**
    - Columnas **agrupables** que dan contexto a los hechos (transforman datos en información).
    - Representan valores **cualitativos**.
    - Poseen elementos o instancias (ej: nombres de páginas web, años específicos).
    - Ejemplos: ítem de venta, Fecha, provincia, producto, cliente, género.
- **Dimensiones:**
    - **Agrupamientos lógicos** de atributos (ej: Dimensión Tiempo, Geografía, Producto).
    - A menudo presentan estructuras jerárquicas (ej: Ciudad -> Provincia -> Región).

**IV. Ventajas del modelo dimensional y Metodología (La "Receta")**

- **Ventajas del modelo:** Predecible, estándar, simétrico (todas las dimensiones están a la misma distancia de los hechos) y fácilmente escalable.
- **Pasos para modelar:**
    1. **Definir el proceso de negocio:** Entender el "dolor" o necesidad del usuario antes de escribir código.
    2. **Definir el nivel de granularidad:** Determinar el detalle más bajo de los datos (ej: nivel de fecha vs. nivel de hora).
    3. **Identificar dimensiones y atributos:** Listar cómo se agrupará la información.
    4. **Identificar los hechos:** Determinar qué se va a medir.

**V. Caso Práctico: Modelo de Ventas y Stock**

- **Análisis de requerimientos:** Necesidad de cruzar ventas (diarias) con stock (mensual valorizado).
- **Definición de granularidad:**
    - Ventas: Fecha, sucursal, ítem/producto, cliente.
    - Stock: Mes, sucursal, ítem/producto.
- **Construcción del modelo lógico:**
    - Identificación de dimensiones compartidas (Geografía, Producto, Tiempo).
    - Diseño de puntos de acceso únicos a las dimensiones para evitar ambigüedades.

**VI. Conceptos Avanzados y Cierre**

- ==**Dimensiones Conformadas:** Dimensiones compartidas por más de un modelo (ej: Producto y Geografía(Dim geografica) compartidos por Ventas y Stock) que permiten cruzar información.==
- **Método de la Matriz:** Herramienta para identificar estas dimensiones conformadas entre distintos procesos.
- **Data Marts:** El resultado físico de los modelos lógicos dentro de un Data Warehouse.
- **Rol de la IA en BI:** Útil para generar código o tablas, pero incapaz de reemplazar la interpretación del negocio y la interacción con el usuario.

---

### **Recopilación de Preguntas**

- ¿Qué es el modelado dimensional?
- ¿Alguien me puede dar un ejemplo de lo que podría ser un hecho?
- ¿Qué más? [en referencia a ejemplos de hechos]
- ¿Alguien se ocurre algo distinto, algún hecho?
- ¿Cuáles serían los dos procesos de negocio que obviamente les dije que qué necesitamos?
- ¿Qué modelos podemos tener acá?
- ¿Cuál es el nivel de granularidad en cada uno [Ventas y Stock]?
- En ventas, ¿cuál es el nivel el más bajo que tengo yo en ventas?
- ¿A qué nivel lo tenés? [una transacción]
- ¿Y a nivel de stock, ¿qué tenemos?
- ¿Se entiende, chicos?
- ¿Qué otro atributo tenemos dando vuelta?
- ¿Cuáles van a ser mis columnas de base que van a estar formando los hechos?
- ¿Cuáles son los agrupamientos lógicos que vamos a tener?
- ¿Qué más? ¿Qué otra dimensión?
- ¿Y esa qué sería? ¿Cómo le podríamos poner esa dimensión?
- ¿Alguna dimensión más tiene el modelo de ventas?
- ¿Qué tiene [el modelo de stock]?
- ¿Cómo se dibuja un modelo dimensional ya con todos los atributos y las dimensiones?
- ¿Cuál sería el ítem?
- ¿Qué me faltaría de venta?
- ¿Qué es una dimensión conformada?
- ¿Dudas hasta acá?
- ¿Les pareció fácil, complejo?

- ¿Una fecha puede ser como fecha de compra [un hecho]?
- ¿Se puede usar Power BI para monitoreo de programación o excepciones de código?
- ¿Lo hacemos ahora o nos juntamos los equipos?
- ¿La granularidad solamente la analizamos en nuestro modelo de negocio o analizamos la de todo el modelo completo?
- ¿Más bajo o más alto, profe? [referente a niveles jerárquicos]
- ¿Estamos suponiendo los atributos del tiempo o tenemos que ser fieles al modelo [DER original]?

---

- **Complejidad de la materia:** Se centra en lo **funcional y arquitectónico** más que en lo estrictamente técnico o de programación.

**II. Fundamentos del Modelado Dimensional**

- **Definición:** Técnica de desarrollo basada en estándares de los años 90 (Kimball e Inmon) para crear modelos intuitivos, escalables y de alto rendimiento.
- **Relación con el modelo relacional:** Se basa en él pero con restricciones de diseño específicas para facilitar el acceso a los datos.

**III. Componentes del Modelo (El "Trípode" de BI)**

- **Hechos (Facts):** Columnas numéricas agregables que representan métricas cuantitativas.
- **Atributos:** Columnas cualitativas que proporcionan contexto y permiten agrupar los datos.
- **Dimensiones:** Agrupamientos lógicos de atributos (ej: Tiempo, Geografía).

**IV. Metodología de Trabajo: "La Receta"**

1. **Definir el proceso de negocio:** Entender el "dolor" del usuario.
2. **Definir la granularidad:** Establecer el nivel mínimo de detalle.
3. **Identificar dimensiones y atributos:** Agrupaciones lógicas para el análisis.
4. **Identificar hechos:** Qué se va a medir o contar.

**V. Caso Práctico: Ventas y Stock**

- **Análisis del DER (Diagrama Entidad-Relación):** Interpretación de tablas de sucursales, productos, ventas y stock.
- **Construcción del modelo lógico:** Definición de dimensiones conformadas (compartidas) y puntos de acceso únicos.

**VI. Conceptos Avanzados y Tecnología**

- **Dimensiones Conformadas:** Clave para cruzar información entre distintos procesos de negocio.
- **Arquitectura:** Paso del modelo lógico al físico (Data Warehouse y Data Marts).
- **Rol de la IA:** Herramienta de apoyo para generar código, pero incapaz de interpretar el negocio de forma autónoma.

---

### **Recopilación de Preguntas y Respuestas para Exámenes**

Esta sección responde directamente a las inquietudes planteadas en la transcripción, organizadas para su estudio:

**1. ¿Qué es el modelado dimensional?** Es una técnica de diseño de datos que busca ser **intuitiva y escalable**, permitiendo un acceso rápido y performante a la información. Se utiliza para plasmar las entidades de negocio en una etapa lógica antes de llevarlas a un modelo físico en un Data Warehouse.

**2. ¿Qué es un hecho y cuáles son sus ejemplos?** Un hecho es un valor **cuantitativo** de una transacción, generalmente numérico y **agregable** (se le pueden aplicar funciones como SUM, AVG, MAX, MIN).

- **Ejemplos comunes:** Monto de venta, unidades vendidas, minutos hablados, cantidad de reclamos, notas de un examen.
- **Ejemplos no convencionales:** Temperatura de un sensor, altura, presión, o incluso la cantidad de posteos en redes sociales.

**3. ¿Una fecha puede considerarse un hecho?** Generalmente **no**. La fecha es un atributo que da contexto (cuándo ocurrió algo). Sin embargo, en casos muy específicos donde se necesite contar "días distintos" en los que hubo actividad, podría tratarse como tal, pero no es lo habitual.

**4. ¿Qué son los atributos y las dimensiones?**

- **Atributos:** Son las columnas **agrupables** (cualitativas). Ejemplo: Provincia, color de producto, género del cliente.
- **Dimensiones:** Son agrupaciones lógicas de esos atributos. La dimensión **Tiempo** es obligatoria en todo modelo de BI.

**5. ¿Qué es el nivel de granularidad?** Es el **nivel de detalle más bajo** que tienen los datos. Definirlo es crucial porque determina qué preguntas puede (y qué no) responder el modelo.

- **Ejemplo en Ventas:** El nivel más bajo puede ser la **transacción por ítem** (Fecha + Sucursal + Producto + Cliente).
- **Ejemplo en Stock:** En el caso visto, la granularidad era **mensual** (Mes + Sucursal + Producto).

La **granularidad** se define como el **nivel de detalle más bajo** que poseen los datos en su origen. Es un concepto fundamental en el modelado dimensional, siendo considerado el segundo nivel más importante después de la definición del proceso de negocio.

A continuación se detallan los puntos clave sobre la granularidad según las fuentes:

- **Determinación de respuestas:** El nivel de granularidad establece qué preguntas de negocio puede (y qué no puede) responder el modelo. Por ejemplo, si los datos están a nivel de **fecha**, no es posible extraer información sobre ventas por **hora**, ya que ese detalle no existe en el modelo.
- **Importancia en el diseño:** Definir correctamente el "grano" evita errores costosos en etapas avanzadas del proyecto. Si un usuario pide un reporte a un nivel más bajo del que se construyó originalmente, el desarrollador tendrá que rehacer el modelo desde etapas anteriores.
- **Relación con los requisitos:** Aunque las buenas prácticas sugieren construir sobre lo que el cliente solicita, es vital indagar si en el futuro se requerirá un mayor detalle (como pasar de un análisis mensual a uno diario) para prever la estructura necesaria.

### **Ejemplos de granularidad vistos en la clase:**

1. **Modelo de Ventas:** El nivel más bajo de detalle identificado está compuesto por la combinación de **fecha, sucursal, ítem/producto y cliente**.
2. **Modelo de Stock:** En el caso práctico, la granularidad es de nivel **mensual**, combinada con **sucursal e ítem/producto**.

En resumen, conocer la granularidad permite al analista establecer los límites máximos de información disponible y asegurar que el modelo coincida con las necesidades reales de explotación de datos.

**6. ¿Qué es una dimensión conformada?** Es una dimensión que es **compartida por más de un modelo** de negocio. Por ejemplo, si tanto el modelo de Ventas como el de Stock usan la dimensión "Producto", esta es una dimensión conformada que permite cruzar datos de ambos procesos en un mismo informe.

**7. ¿Se puede usar Power BI para monitoreo de programación o excepciones de código?** **Sí**, siempre que exista un log o traqueo de esa información (en bases de datos, archivos TXT o logs de servidor). Se puede medir, por ejemplo, el tiempo que un usuario pasa en una app o cuánto tarda un proceso de código en ejecutarse.

---

### **Lecciones Aprendidas**

1. **El Negocio manda sobre la Tecnología:** No se debe empezar a escribir código SQL o Python sin entender primero el "dolor" del usuario y el proceso de negocio.
2. **La "Receta" evita el retrabajo:** Saltarse la definición de la granularidad puede llevar a que el modelo final no pueda responder preguntas básicas (como ventas por hora si solo se guardó por día), lo que implica un alto costo de corrección.
3. **Simetría y Previsibilidad:** El modelo dimensional es ventajoso porque es estándar; todas las dimensiones están a la misma "distancia" de los hechos, lo que facilita su uso por parte de usuarios no técnicos.
4. **Escalabilidad:** Un modelo bien diseñado permite agregar nuevas dimensiones (como "Vendedor") o atributos (como "Categoría de Cliente") sin destruir la estructura existente.
5. **Límites de la IA:** Aunque la IA puede generar tablas y código, la **interpretación del negocio** y la interacción con los usuarios para definir requerimientos sigue siendo una tarea netamente humana.

---

### **Detalle de Ejemplos Prácticos**

- **El sensor de las heladeras:** En un evento (recital), se usaron sensores de volumen y apertura de puerta en heladeras para registrar datos en un archivo TXT. Esto demuestra que la fuente de datos para BI no siempre es una base de datos SQL; puede ser cualquier registro digital.
- **Atención al cliente:** El ejemplo de traquear desde que una persona saca un ticket en un tótem, es llamada por la recepción y finaliza su trámite permite calcular métricas de "tiempo de espera" y "tiempo de atención".
- **Modelo Ventas vs. Stock:**
    - **Ventas:** Granularidad diaria. Permite ver el "producto estrella" o la evolución mes a mes.
    - **Stock:** Granularidad mensual. Permite ver el capital frenado y compararlo con las ventas para decidir si mover mercadería entre sucursales.
- **El "punto de acceso único":** Al dibujar el modelo, una dimensión como "Geografía" debe tener un solo punto de entrada al hecho (por ejemplo, a través de la Sucursal), aunque internamente la dimensión tenga jerarquías de Ciudad, Provincia y Región.