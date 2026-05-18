
### **II. Conceptos Fundamentales de Inteligencia de Negocios**

- **Gestión del Conocimiento:**
    - **Dato:** Factores discretos sin contexto (ej. un número suelto).
    - **Información:** Datos procesados con significado, propósito y relevancia mediante contextualización, categorización o cálculo.
    - **Conocimiento:** Información aplicada para comparaciones, conexiones y toma de decisiones.
- **Definiciones de BI:**
    - **Enfoque tecnológico (TDWI):** Procesos y herramientas para transformar datos en planes de negocio rentables.
    - **Enfoque de gestión:** Conjunto de procesos orientados al análisis para la ayuda en la toma de decisiones.
- **Desafíos de las Organizaciones:**
    1. **Única verdad:** Lograr que todas las áreas manejen los mismos criterios y definiciones de datos.
    2. **Visión integral:** Entender el origen y flujo del dato en toda la empresa (profiling).
    3. **Tiempo adecuado:** Acceder a la información en el momento necesario (tiempo real, diario o mensual según la necesidad).

### **III. Arquitectura de Business Intelligence**

- **Capas de la Arquitectura:**
    - **Capa de Datos:** Orígenes (ERP, CRM, archivos, sensores) y transformación.
    - **Capa de Entendimiento (Metadatos):** Aplicación de reglas de negocio.
    - **Capa de Acción (Explotación):** Visualización y toma de decisiones.
- **Componentes Técnicos:**
    - **Área de Staging (Bronze/Raw):** Repositorio temporal de datos crudos para no afectar los sistemas transaccionales.
    - **Data Warehouse (DW):** Repositorio integrado, no volátil, variable en el tiempo y orientado al negocio.
    - **Data Marts:** Áreas temáticas específicas dentro del DW (ventas, compras, etc.).
    - **ODS (Operational Data Store):** Área intermedia con datos más detallados que el DW.
    - **Exploration Warehouse:** Copia del DW para que ingenieros de datos realicen pruebas sin afectar el rendimiento general.
- **Nuevas Tecnologías:**
    - **Bases de Datos No Estructuradas:** Almacenamiento de imágenes, videos y documentos (asociado a Big Data).
    - **Streaming de Datos:** Captura de información en tiempo real (ej. sensores industriales).

### **IV. Explotación y Visualización**

- **Estilos de Entrega:**
    - **Reporting:** Grillas de texto plano.
    - **Documentos:** Mezcla de gráficos, tendencias y colores.
    - **Dashboards (Tableros):** Indicadores clave (KPIs) comparados contra objetivos.
- **Análisis Avanzado:**
    - **Cubos OLAP:** Análisis multidimensional (drill-down) por categorías como tiempo, producto o cliente.
    - **Data Mining:** Modelos matemáticos para predecir el futuro o encontrar relaciones ocultas entre productos.
    - **Data Discovery:** Herramientas que dan independencia al usuario final para explorar datos sin depender de IT.
- **Funcionalidades Adicionales:**
    - **Mobile:** Acceso desde dispositivos móviles.
    - **Writeback:** Capacidad de ingresar datos nuevos (como cotizaciones de dólar) desde el propio tablero.
    - **Alertas y Distribución:** Notificaciones automáticas y envío masivo de informes.

---

### **I. Introducción y Logística de la Cursada**

- **Presentación del Docente:** Ariel, especialista en MicroStrategy, Power BI y arquitecto de datos en Fabric.
- **Estructura de la Materia:**
    - **Tres Ejes:** Conceptos iniciales y modelado multidimensional, modelado físico y desarrollo práctico en Power BI.
    - **Metodología:** Clases virtuales vía Zoom y encuentros presenciales mensuales para ejercicios prácticos.
    - **Evaluación:** Un parcial (promoción con 8, o 7 si el TP es sobresaliente) y un Trabajo Práctico (TP) cuatrimestral grupal.
    - **Uso de IA:** Se enseñará a aprovechar la Inteligencia Artificial para el desarrollo en Power BI.

### **II. La Gestión del Dato: Del Dato a la Sabiduría**

- **Dato:** Conjunto discreto de factores sin contexto ni relevancia por sí mismos (ej. "80%" o un mapa sin leyenda).
- **Información:** Datos procesados con significado, propósito y relevancia. Se obtiene mediante:
    - **Contextualización:** Para qué sirve el dato.
    - **Categorización y Condensación:** Agrupar ventas por día, mes o cliente.
    - **Cálculo:** Precio x Cantidad = Venta.
    - **Corrección:** Normalizar nombres (ej. "Bs As" a "Buenos Aires").
- **Conocimiento:** Información aplicada a la acción y la experiencia.

### **III. El Corazón de Business Intelligence (BI)**

- **Definiciones:**
    - **TDWI:** Procesos y herramientas para transformar datos en planes de negocio rentables.
    - **Enfoque de Gestión:** Conjunto de procesos y herramientas para el análisis que ayuda a la toma de decisiones.
- **Los Tres Desafíos Organizacionales:**
    1. **Única Verdad:** Lograr que todos los departamentos manejen la misma definición de un dato (ej. ¿Qué es una venta?).
    2. **Visión Integral:** Entender el origen y flujo del dato en toda la empresa (profiling).
    3. **Tiempo Adecuado:** Acceder a la información cuando se necesita (tiempo real, diario o mensual según el área).

### **IV. Arquitectura y Componentes Técnicos**

- **Capas de la Arquitectura:**
    - **Capa de Datos:** Orígenes (ERP, CRM, logs, sensores) y transformación.
    - **Capa de Entendimiento (Metadatos):** Donde residen las reglas de negocio.
    - **Capa de Acción (Explotación):** Visualización y toma de decisiones.
- **Componentes de Almacenamiento:**
    - **Área de Staging (Bronze/Raw):** Repositorio temporal para lectura rápida sin afectar los sistemas transaccionales.
    - **Data Warehouse (DW):** Repositorio integrado, no volátil, variable en el tiempo y orientado al negocio.
    - **Data Marts:** Áreas temáticas específicas del DW (ventas, compras, etc.).
    - **ODS (Operational Data Store):** Área intermedia con datos más detallados que el DW para análisis específicos.
    - **Exploration Warehouse:** Copia del DW para que ingenieros de datos realicen pruebas sin afectar el rendimiento general.

---

### **Recopilación de Preguntas y Respuestas para Exámenes**

#### **Conceptos sobre el Conocimiento y el Dato**

- **¿Cuándo se transforma el dato en conocimiento?** Se transforma cuando se utiliza para realizar **comparaciones** (ventas vs. objetivos), encontrar **conexiones** (relación entre sucursales o productos) y cuando se le suma la **experiencia y valores** del usuario.
- **¿Por qué se transforma en conocimiento?** Porque permite atribuir significado a la información para que esta genere consecuencias o acciones de negocio concretas.

#### **Escenario de Ejemplo: El Problema de las Ventas**

- **¿Me puede decir cuánto dieron las ventas? (Pregunta retórica)** Es el ejemplo del conflicto de la "Única Verdad". En una reunión, Ventas puede decir 210, Marketing 250 y Finanzas 290.
- **¿Cuál es la venta? / ¿Qué son las ventas?** La respuesta depende del criterio: Ventas puede ver el dato crudo, Marketing puede incluir beneficios comerciales y Finanzas puede sumar impuestos y retenciones. **La lección aprendida es definir una métrica única (ej. importe bruto sin impuestos) para que todos discutan la acción y no el dato**.

#### **Arquitectura y Procesamiento**

- **¿A qué llamó su origen de datos?** A cualquier sistema que contenga un dato: ERP, CRM, archivos Excel, redes sociales, sensores industriales (streaming) o incluso logs de dispositivos.
- **¿Qué hago con todo eso (los datos crudos)?** Se llevan a un área de almacenamiento (Data Warehouse) tras pasar por un proceso de entendimiento donde se aplican **reglas de negocio** para transformarlos en activos de la compañía.
- **¿De qué se trata esto? (Sobre el procesamiento de datos)** De resolver necesidades de negocio mediante el uso de tecnología, garantizando que el usuario pueda responder preguntas de manera rápida y confiable.
- **¿Qué es un área de stage?** Es un **repositorio temporal y crudo** de información. Su objetivo es desconectarse rápido del sistema origen para no afectar su performance y tener los datos disponibles para el proceso de BI.
- **¿Qué es un Data Warehouse (DW)?** Según el profesor, es una estructura construida específicamente para **dar respuesta a situaciones de negocio**. Sus características clave son: **Integrado, No Volátil, Variable en el tiempo y Orientado al negocio**.
- **¿Por qué hablamos que es integrado?** Porque consolida datos de fuentes heterogéneas (bases de datos, videos, redes sociales, archivos) bajo una estructura unificada.
- **¿Qué son los ODS?** Son almacenes de datos intermedios que guardan un nivel de **detalle mayor** que el Data Warehouse, permitiendo análisis granulares si el negocio lo requiere.

#### **Técnicas de Análisis y Explotación**

- **¿Qué es el data mining?** Es el uso de modelos matemáticos (regresiones, redes neuronales) para **descubrir información oculta** o predecir comportamientos futuros.
- **¿Qué producto se vende mejor con tal otro producto?** Esta es una pregunta típica que resuelve el **Data Mining** a través de reglas de asociación y relacionamiento de datos.
- **¿Qué regla de negocio tengo que aplicar para que se cumpla tal cosa?** Se define mediante el entendimiento del negocio y se plasma en la capa de metadatos para transformar la información en conocimiento accionable.
- **¿Para qué necesito el writeback?** Para permitir que el usuario **ingrese datos directamente desde el tablero** (ej. cargar la cotización del dólar o un presupuesto) que luego impactan en el Data Warehouse para ser usados en cálculos.
- **¿Qué más hay como herramientas de explotación?** Alertas automáticas (vía mail, Teams, Slack), aplicaciones móviles, distribución masiva de informes (PDFs) y herramientas de **Data Discovery** que dan independencia al usuario frente a IT.

---

### **Preguntas de los Alumnos al Profesor**

- **¿Hay mucha diferencia entre Qlik y Power BI a nivel técnico?** A nivel de uso es un cambio rotundo, pero los conceptos de **modelado y resolución de problemas de negocio** son los mismos.
- **¿Dentro de explotación no tendríamos que poner los RAG (modelos de lenguaje)?** Sí, son componentes nuevos de explotación. Cualquier agente de IA que pueda leer los datos y responder preguntas sobre el modelo es una herramienta válida.
- **¿Los grupos son de 4 o 5 personas... cómo hacemos para mandárselos?** Se definirán durante el mes de abril. No se aceptarán trabajos individuales.

---

### **Lecciones Aprendidas Detalladas**

1. **Prioridad del Modelo sobre la Herramienta:** Aprender Power BI es sencillo (es como aprender a usar Word), pero lo difícil y valioso es aprender a **modelar y escribir la "carta"** (resolver el problema de negocio).
2. **El Sistema es Cerrado y Evolutivo:** Un tablero que no cambia en meses es un tablero que no se usa. El usuario siempre pedirá más datos (presupuestos, comisiones, gastos) una vez que ve el valor de la primera entrega.
3. **Calidad del Dato:** Si el dato está mal, el usuario desconfía del sistema. Un ejemplo fue el error en los mapas de ventas causado porque los operarios cargaban por defecto el código postal "1000".
4. **IA como Complemento, no Reemplazo:** La IA puede generar reportes si el modelo es bueno, pero aún no puede entender qué quiere el negocio ni diseñar la arquitectura de datos inicial.
5. **Indispensabilidad del Tiempo:** En el modelado multidimensional, la dimensión de **tiempo es obligatoria**. No incluirla es motivo de desaprobación automática.