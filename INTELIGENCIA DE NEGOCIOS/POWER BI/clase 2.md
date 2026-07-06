### 1. Las Áreas de Trabajo (Workspaces) en Power BI

Las **áreas de trabajo** funcionan como carpetas o contenedores en la nube (Power BI Service) donde se guardan, organizan y publican los informes y modelos semánticos.

- **Requisitos:** Su creación está disponible con una suscripción gratuita de prueba o una licencia paga.
- **Administración y Seguridad:** Permiten configurar parámetros de seguridad y accesos específicos para diferentes usuarios.
- **Trabajo Colaborativo:** Un administrador puede invitar a otras personas (por ejemplo, colegas) a su área de trabajo, otorgándoles permisos para acceder, visualizar, editar, modificar, borrar o agregar nuevos tableros y modelos.
- **Trazabilidad (Linaje):** Dentro del área de trabajo, existe una vista de "linaje" que muestra visualmente de dónde proviene la información. Permite trazar cómo un informe específico está conectado a un Modelo Semántico particular, y cómo este, a su vez, está conectado a una fuente externa (como un documento de Google).

### 2. Ejemplos Prácticos de Usos de Power BI (Con Indicaciones Paso a Paso)

A continuación, se detallan múltiples ejemplos de operaciones fundamentales extraídos de la transcripción:

**Ejemplos de Transformación en Power Query:**

- **1. Conexión a Google Sheets:** Haz clic en _Obtener datos_ -> Selecciona _Hoja de cálculo de Google_ -> Pega la URL del archivo -> Inicia sesión con las credenciales corporativas o educativas.
- **2. Promover la primera fila como encabezado:** Si la primera fila de tu archivo tiene los nombres de las columnas en lugar de datos, ve a la pestaña _Transformación_ y selecciona la opción _Usar primera fila como encabezado_.
- **3. Corregir errores tipográficos (Reemplazar valores):** Haz clic derecho sobre una columna (por ejemplo, "Prioridad"), elige _Reemplazar los valores_ y escribe el valor erróneo (ej. "mediana") para cambiarlo por el correcto (ej. "media").
- **4. Dividir datos en múltiples columnas:** Selecciona una columna que contenga muchos datos juntos (ej. "Región"), ve a _Dividir la columna por un delimitador_, elige el carácter que los separa (como un punto y coma), y Power BI la dividirá en tres nuevas columnas (ej. provincia, región y país).
- **5. Crear Columnas Condicionales (Reglas If/Else):** Utilizando el botón _Columna condicional_, puedes programar reglas sobre otra columna. Por ejemplo, en una columna de "Cantidad": Si el valor es menor a 15, escribir "bajo"; si es menor o igual a 30, escribir "medio"; y de lo contrario, "alto".
- **6. Crear Columnas Calculadas Matemáticas:** Agrega una columna personalizada para calcular la "Ganancia bruta". La fórmula consiste en seleccionar la columna "Venta final", aplicar un signo de multiplicación y seleccionar la columna "Margen". Luego, ajusta el tipo de dato a "número decimal".

**Ejemplos de Visualización y Explotación de Datos:**

- **7. Implementar Segmentadores (Filtros Interactivos):** Agrega un gráfico tipo "segmentador de datos", arrastra la dimensión "Año", configúralo como una "lista vertical" y establécelo como de uso obligatorio para que los usuarios filtren el resto del tablero.
- **8. Configurar vistas de detalle (Tooltips / Información sobre herramientas):** Permite que un usuario posicione el ratón sobre una barra específica del gráfico (ej. ventas de "prioridad baja") y automáticamente aparezca un mini-gráfico incrustado revelando información detallada, como el tipo de "embalaje" utilizado.
- **9. Crear una vista móvil nativa (Mobile Layout):** Ve a la sección _Editar_, activa la vista de _diseño móvil_ (ícono de teléfono) en la parte inferior, y simplemente arrastra los gráficos que ya creaste en la PC para apilarlos verticalmente. Conservan toda la interactividad.
- **10. Exportar a PowerPoint en directo:** Elige la opción _Exportar_ -> _PowerPoint en directo_. Esto incrusta el tablero dentro de una diapositiva creando un vínculo real con la nube. En una reunión de ventas, puedes filtrar e interactuar con los datos directamente desde la presentación. Power BI también permite exportar tableros a PDF estático o como imagen simple.
- **11. Analizar directamente en Excel:** Con la opción _Analizar en Excel_, Power BI crea una conexión viva que permite abrir el Modelo Semántico desde Microsoft Excel. Los analistas pueden usar tablas dinámicas nativas de Excel (ej. ventas por región en filas y fechas en columnas) conectadas a los datos de Power BI.

---

### 3. Preguntas y Respuestas para Exámenes (Q&A)

**P1: ¿Qué ocurre si me equivoco al aplicar una regla de limpieza (por ejemplo, un filtro incorrecto) en Power Query? ¿Tengo que empezar de cero?** **R:** No. Power Query mantiene un panel lateral llamado "pasos aplicados" que funciona como un historial de acciones. Cada clic es una línea de código. Puedes eliminar un paso haciendo clic en él o editarlo directamente (por ejemplo, cambiar el límite de una condición de 15 a 25) y el modelo se recalculará automáticamente.

**P2: Si otra persona añade nuevos registros al archivo de Excel original, ¿el tablero gráfico se actualiza en tiempo real frente al usuario?** **R:** No, el tablero (informe visual) no controla ni almacena los datos; solo actúa como un lector. Para ver los nuevos datos, primero hay que dar la orden de "actualizar" (refresh) al _Modelo Semántico_. Una vez que el modelo procesa las novedades de la base de datos, los tableros que dependan de él reflejarán los cambios. Además, esta tarea se puede programar automáticamente (ej. todos los días a las 10:00 PM).

**P3: ¿Qué es el "Perfil de la Columna" y qué información brinda a un analista?** **R:** Es una herramienta analítica en Power Query que evalúa una columna. Permite entender la distribución de los datos (ej. curva de Gauss), cuántas filas existen, cuenta la cantidad de valores únicos, determina promedios y desviaciones estándar, y detecta anomalías, como el porcentaje de campos vacíos (nulos) o textos en celdas numéricas.

**P4: ¿Se pueden construir múltiples tableros visuales totalmente diferentes partiendo del mismo origen de datos?** **R:** Sí. Un solo _Modelo Semántico_ puede alimentar a múltiples informes a la vez (por ejemplo, un tablero resumen y otro tablero enfocado a evoluciones históricas diarias). Al conectarse al mismo modelo central, se garantiza que todos los usuarios de la empresa consuman exactamente los mismos valores sin discrepancias.

---

### 4. Lecciones Aprendidas Clave

- **Cada interfaz gráfica esconde Código "M":** Una lección vital es comprender que la interfaz de botones de Power Query genera el lenguaje estructurado "M" de fondo. Si haces clic en el "Editor avanzado", puedes copiar todo ese texto, pegarlo y enviárselo a un colega para que replique tus transformaciones en otra máquina, haciendo todo el trabajo completamente reutilizable y paramétrico.
- **La automatización es permanente (Escalabilidad):** Todos los pasos de transformación (como cambiar textos, agrupar columnas o limpiar nulos) se graban como un procedimiento. La próxima vez que Power BI se conecte a la base de datos, aunque ahora tenga miles de registros nuevos, ejecutará secuencialmente cada instrucción preprogramada sin que tengas que intervenir manualmente.
- **Integración ilimitada (Flexibilidad total):** Power BI está diseñado para destruir los silos de datos. Dentro de un mismo proyecto, el sistema es lo suficientemente potente para tomar un archivo plano (CSV) viejo, unirlo con una base de datos relacional moderna corporativa y mezclarlo, todo simultáneamente, con interacciones sociales obtenidas de la web de Instagram, consolidando todo en un único Modelo Semántico.