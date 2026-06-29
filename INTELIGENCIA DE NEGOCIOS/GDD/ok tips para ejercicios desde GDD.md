recopila todas las preguntas y respuestas de las transcripciones, definí dominio, tabla de hechos, el modelo copo de nieve el modelo estrella, recomendaciones

definí dominio, da el instructivo para poder enlistarlos, para poder identificar los dominios, como armar el modelo dimensional
como listar las dimensiones y como poder ejemplificar, dar recomendaciones de armado
definir la tabla de hechos, dar ejemplos, como identificar la tabla de hechos
En que procesos de negocios englobaría los hechos detectados
que es la granularidad de un modelo, para que sirve, como lo uso, da ejemplos

----


**Recopilación de Preguntas y Respuestas de las clases:**

- **P:** ¿La tabla de información OLAP se actualiza automáticamente con la de los sistemas transaccionales? **R:** No es automático ni instantáneo. Se actualiza mediante procesos "batch" en ventanas de tiempo (ej. mensual, diario), porque si se permitiera la actualización automática en tiempo real se castigaría la operativa del sistema y los datos perderían consistencia durante consultas analíticas extensas.
- **P:** ¿Por qué se dice que el sistema OLTP es volátil? ¿Se borra la base de datos al apagar? **R:** No se refiere a la memoria RAM. Es volátil porque la información operativa va cambiando día a día y, si bien se pueden hacer respaldos, no hay obligación de tener 15 años de historia operativa en línea. En cambio, OLAP es no volátil y persistente porque guarda exclusivamente el histórico de años anteriores para analizar tendencias.
- **P:** ¿El "cubito" (intersección) en las herramientas OLAP es físicamente como una tabla normal de base de datos? **R:** Sí. En las implementaciones sobre Bases de Datos Relacionales (RDBMS), el modelo se almacena en tablas normales (tablas de hechos y dimensiones) donde cada registro es la intersección de varias claves.
- **P:** En la dimensión "Tiempo", ¿por qué usar una Primary Key (PK) de código autonumérico en lugar de la fecha misma? **R:** Se hace para desnormalizar y optimizar las consultas. Al usar un código autonumérico y guardar campos precalculados (ej. cuatrimestre, día hábil, feriado, hora pico), el motor de base de datos no tiene que calcular funciones de rango de fechas en tiempo real, lo que hace el cruce muchísimo más rápido.
- **P:** Al armar la tabla de hechos, ¿es conveniente poner "litros despachados" (combustible) y "cantidad de unidades" (repuestos) en la misma tabla? **R:** Presenta problemas. Las unidades de medida son distintas (los repuestos son enteros discretos, los litros tienen decimales/fracciones) y, dependiendo de lo que se venda, uno de los dos campos siempre quedará en cero, generando una tabla ineficiente. Es mejor separarlo en dos tablas de hechos distintas.
- **P:** ¿Cómo se le llama al modelo donde se enfoca en una sola tabla de hechos con sus dimensiones a los costados? **R:** Se le conoce como "Data Mart", que es un subconjunto del "Data Warehouse" enfocado en un solo sector o área particular del negocio.
- **P:** En un trabajo práctico, ¿cómo extraigo dimensiones como "edad" o "sexo" si no los tengo en la tabla transaccional origen? **R:** La edad se puede inferir calculándola a partir de la "fecha de nacimiento". Si un dato no existe y no hay forma de derivarlo lógicamente ni cruzando fuentes de información externas, no se puede incorporar a la dimensión.

---

**1. Dominios (Patrones de Interés / Dimensiones)**

- **Definición:** El dominio de análisis representa los **patrones de interés** (sujetos u objetos) sobre los cuales la organización desea guardar, agrupar y analizar información. En la terminología técnica se los denomina **Dimensiones**.
- **Instructivo para identificarlos y enlistarlos:**
    1. Ubicar los actores principales del negocio analizando a los **sujetos/objetos** (ej. en una universidad: profesores, alumnos, materias; en retail: clientes, sucursales, autos, repuestos).
    2. Analizar qué perspectivas requiere la gerencia para visualizar los datos (¿Quién compra? ¿Qué compra? ¿Dónde? ¿Cuándo?).
- **Cómo armar el modelo dimensional y listarlo (Ejemplificación):** Debes enlistar las dimensiones separando conceptualmente cada actor. Por ejemplo:
    - _Dimensión Tiempo:_ Define en qué momentos sucedieron los eventos.
    - _Dimensión Cliente / Segmento de Cliente:_ Perfil de quién generó la acción.
    - _Dimensión Geográfica:_ Provincias, localidades o estaciones de servicio donde ocurrió el evento.
- **Recomendaciones de armado:**
    - Las dimensiones son el espacio ideal para agregar **toda la información posible (interna y externa)** que no importaba en el sistema operativo (ej. si el alumno vive con sus padres, estado civil, ingresos, club del que es hincha), porque estos datos "ocultos" son los que después cruzará el algoritmo de _Data Mining_ para descubrir por qué ocurren las cosas.
    - Utilizar **Claves Subrogadas** (claves sintéticas autonuméricas ocultas al usuario) en lugar de las claves primarias reales de los sistemas operativos, porque la información proviene de múltiples orígenes y los códigos originales podrían chocar.

---

**2. Tabla de Hechos (Fact Table)**

- **Definición:** Es la tabla central del modelo dimensional. Su característica principal es que **su Clave Primaria (PK) es compuesta**, formada por la sumatoria de las claves foráneas (FK) de todas las dimensiones que la rodean. Adicionalmente, contiene los atributos matemáticos o **métricas** dependientes de esa intersección.
- **Cómo identificarla:** Se detecta ubicando la transacción central, el evento o "el hecho" del negocio que se desea medir. Los atributos de esta tabla siempre cambian si se altera al menos una dimensión (si cambia el año, o cambia el alumno, cambian las notas o las ventas).
- **Ejemplos:**
    - _Facultad:_ Una tabla de hechos de "Evaluaciones" cuyas PK sean las dimensiones Cuatrimestre, Materia y Profesor; y sus métricas sean "cantidad de aprobados", "cantidad de inscriptos" o "desertores".
    - _Concesionaria/Estación:_ Una tabla de hechos de "Ventas de Combustible" que almacene la intersección de Tiempo, Producto, Estación y Tipo de Pago, donde las métricas sean "Litros vendidos" y "Cantidad de clientes".

---

**3. Modelos Arquitectónicos de Implementación**

- **Modelo Estrella (Star Schema):** Es la topología más pura y utilizada. En este modelo, **cada dimensión se representa con una sola tabla fuertemente desnormalizada**. Gráficamente, se ve la tabla de hechos en el núcleo conectándose directamente con un único anillo de tablas de dimensiones (como si fueran los rayos de un sol).
- **Modelo Copo de Nieve (Snowflake Schema):** Es una variación donde **alguna de las dimensiones está compuesta por más de una tabla** (tienen un nivel de normalización). Por ejemplo, si en lugar de poner Provincia y País en la misma "Dimensión Geográfica", se crean tablas separadas unidas entre sí antes de llegar a la tabla de hechos.

---

**4. Procesos de Negocios de los Hechos Detectados**

- Los hechos detectados no se engloban en la operatoria diaria (como emitir una factura o inscribir un alumno), sino en **procesos gerenciales para la toma de decisiones estratégicas o tácticas**.
- Se engloban en el área de **Business Intelligence (BI)** y sirven para:
    - Ejecutar análisis de Marketing (campañas de promociones por tipo de cliente o estación).
    - Generar predicciones meteorológicas o de riesgos.
    - Realizar _Data Mining_ exploratorio para entender desvíos de desempeño, descubrir correlaciones ocultas ("¿Por qué aprueban ciertos alumnos?" o "¿Qué patrón une a los clientes que abandonan un servicio?").

---

**5. Granularidad de un Modelo**

- **Definición:** Es el **nivel mínimo de detalle o la unidad de medida atómica** que define cómo se almacenará y agrupará la información en cada dimensión.
- **Importancia:** La granularidad determina exactamente qué se puede y qué no se puede consultar. Si tu dimensión de tiempo agrupa los hechos por "mes" (como un corte mensual), el Data Warehouse podrá dar reportes trimestrales y anuales, pero **nunca podrá entregar un reporte por semana o por día**. Si la granularidad geográfica es la "provincia", jamás se podrá analizar el comportamiento de una sola "ciudad".
- _Recomendación:_ Bajar la granularidad al mínimo posible (ej. hora del día o ticket por sucursal) otorga un inmenso poder de análisis, pero multiplica exponencialmente el tamaño y los recursos necesarios de la base de datos, por lo que requiere buscar el punto de equilibrio correcto para el negocio.


La **dimensión tiempo es fundamental** en los modelos de inteligencia de negocios (OLAP y Data Warehouse) porque estas bases de datos se dedican a guardar información histórica. Como los datos de análisis son persistentes y representan cosas que ya pasaron, incorporar el tiempo **permite proyectar, hacer análisis temporales y pronosticar el futuro** analizando tendencias y comportamientos del pasado.

**Ejemplos y características de la dimensión tiempo:**

- **Atributos precalculados:** Para optimizar y acelerar las consultas, no se guarda una simple fecha, sino que la tabla **se desnormaliza** incluyendo múltiples columnas precalculadas, como: año, semestre, cuatrimestre, mes, día de la semana, indicador de día hábil, indicador de feriado o incluso si es un "horario pico".
- **Granularidad (nivel de detalle):** El corte de tiempo depende de lo que el negocio necesite medir.
    - En una estación de servicio, la unidad mínima puede ser **la hora del día**, lo cual es útil para analizar cuándo hay más flujo de clientes y decidir si hace falta abrir más cajas o surtidores en horarios pico.
    - En un ámbito universitario, el corte lógico suele ser **el cuatrimestre**, ya que las materias y los resultados se evalúan en ese bloque de tiempo.
    - Para un supermercado, hacer un análisis diario puede generar información engañosa (por ejemplo, si un día llovió torrencialmente, las ventas caerán drásticamente). Por ello, suelen usar cortes **mensuales** que absorben esas anomalías y reflejan mejor si la gente tiene más capacidad de compra a principio o fin de mes.
- _Dato técnico:_ A diferencia de otras dimensiones, los datos de la dimensión tiempo **no se extraen ni se migran de los sistemas operativos (origen)**, sino que se generan de forma artificial y propia para el Data Warehouse, utilizando generalmente una clave primaria (PK) autonumérica.

**Sí, existen muchas más dimensiones.** Las dimensiones representan los **patrones de interés** (los sujetos u objetos) por los cuales una organización desea agrupar, filtrar y analizar la información. Dependiendo del área del negocio, algunas dimensiones comunes incluyen:

- **Dimensión Geográfica:** Permite agrupar los hechos por zonas espaciales, abarcando desde el país y la provincia, hasta la localidad o la sucursal/estación específica donde ocurrió el evento.
- **Dimensión Producto / Línea de Productos:** Categoriza lo que se está transaccionando. Puede incluir el código de catálogo del producto, familias de productos, el tipo de moneda utilizada y un historial de los precios.
- **Dimensión Segmento de Clientes:** En lugar de enfocarse en el nombre o apellido del individuo, agrupa a las personas mediante patrones y atributos poblacionales: rango de edad, localidad de residencia, género o estado civil, permitiendo descubrir tendencias de consumo basadas en el perfil de vida.
- **Dimensión Tipo de Pago:** Detalla cómo se abonó una transacción (efectivo, débito, tarjeta de crédito, banco emisor, cheques), siendo muy útil para evaluar el impacto de las campañas de marketing o promociones bancarias.
- **Dimensiones de un negocio específico (ej. Universidad):** Profesores, materias, alumnos y turnos (mañana, tarde, noche), creadas para evaluar comportamientos como, por ejemplo, en qué turnos o con qué profesores aprueban más los alumnos.



