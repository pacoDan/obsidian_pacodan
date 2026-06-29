A partir de todas las transcripciones, el profesor ha sido muy explícito a lo largo de las clases sobre qué preguntas conceptuales y ejercicios prácticos entran en los parciales (y en finales orales). A continuación, se detalla la recopilación absoluta de todo lo que se evaluará, así como lo que está excluido.

### 📌 Preguntas Teóricas y Conceptuales de Parcial (Multiple Choice)

Estas son las preguntas teóricas que el profesor señaló textualmente como "pregunta de parcial":

- **Definición de Sistema de Información:** Se evalúa recordar el concepto base. Es un conjunto de subsistemas o elementos organizados para recopilar, procesar, almacenar y distribuir información, con el objetivo de control y apoyo a la toma de decisiones.
- **Características del Data Warehouse (Clásica pregunta multiple choice):** Deben identificarse las cuatro características fundamentales definidas por Inmon, que son: **integrado, no volátil, variable en el tiempo y orientado al negocio**.
- **Nivel de conocimiento de los hechos:** Se pregunta explícitamente "¿Qué significa el nivel al cual se conocen los hechos?". La respuesta correcta es el **nivel de granularidad**.
- **Conjunto de hechos:** Se pregunta "¿Qué son los conjuntos de hechos que se conocen al mismo nivel?". La respuesta correcta es un **Datamart**.
- **Diferencia entre Tablas Base y Agregadas:** Es una pregunta muy frecuente (incluso para promocionar en el oral) saber identificar la diferencia entre una tabla base de hechos y una tabla agregada, y especialmente **cómo se llena cada una**. La tabla base se llena desde el sistema transaccional (OLTP), mientras que la agregada se llena obligatoriamente a partir de la tabla base (nunca directo del origen) para garantizar que los datos crucen correctamente y no penalizar al sistema origen.
- **Tipos de Hechos (Pregunta engañosa):** Se evaluará la definición de hechos aditivos, semiaditivos y no aditivos. El profesor remarcó que la "trampa" de esta pregunta es que no se trata solo de que se le pueda aplicar una función matemática a la columna, sino de que **tenga sentido de negocio** sumarizarlo.
- **Dimensión obligatoria:** Una regla que no se perdona en la corrección es la ausencia de la **dimensión tiempo/fecha**. Todo modelo dimensional debe tenerla obligatoriamente para cumplir con la característica de ser variable/perdurable en el tiempo.

### 🛠️ Ejercicios Prácticos del Parcial

En cuanto a la práctica, el profesor aclaró qué deben hacer y qué no:

- **Corregir modelos con errores:** Es un ejercicio típico donde entregan un modelo dimensional y los alumnos no deben desarrollarlo de cero, sino **detectar y marcar errores típicos**.
- **Ejemplos de errores a detectar:** Marcar si a uno de los Datamarts le falta la dimensión de tiempo, o si existen relaciones inventadas entre atributos que no deberían estar.
- **Identificación de Modelado Físico:** En la parte de modelado físico, no se pide crear un modelo físico complejo desde cero, sino que el profesor presentará dimensiones o tablas en el examen y preguntará: "¿Qué dimensión es esta?". El alumno deberá saber identificar, por ejemplo, si se trata de una tabla de hechos (FAC) o si el diagrama muestra una "Slowly Changing Dimension Tipo 2" (SCD2).

### 🧠 Preguntas adicionales "para pensar"

El profesor dejó planteadas dos preguntas reflexivas para los alumnos relacionadas al modelado físico y las claves subrogadas (SK), que son excelentes candidatos a preguntas teóricas sobre rendimiento:

- ¿Qué beneficios y contras tendría usar una clave autoincremental directamente sobre la dimensión de Tiempo (Fecha)?
- ¿Qué otros métodos o técnicas de bases de datos se pueden utilizar para generar una Clave Subrogada (SK) cuando el motor de base de datos no soporta campos "autoincrementales" nativos (como pasa en un Datalake o base de datos de archivos)?

### 🚫 Lo que NO entra en el parcial

El profesor ha sido muy claro con los temas que se explican para la vida profesional pero que **están completamente excluidos de la evaluación del examen**:

- **Herramienta Power BI y DAX:** No se toma nada del uso de Power BI, visualizaciones, ni cómo crear medidas o fórmulas con código DAX. Esto es exclusivo del trabajo práctico final.
- **Desarrollo de Modelos Físicos desde cero:** No se pide que el alumno desarrolle o dibuje un modelo físico desde cero en la prueba.
- **Monster Dimension:** Se explica por su relevancia en grandes bases de datos (como puntajes volátiles que cambian varias veces al día), pero aclaró explícitamente: "No la tomo en el parcial, pero es para que entiendan".
- **Subdimensiones:** También advirtió que no suelen tomar el concepto de subdimensión (romper una estrella a copo de nieve) en los parciales.