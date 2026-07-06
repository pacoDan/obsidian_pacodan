### 1. Componentes y Arquitectura de Power BI

Power BI es una herramienta de Microsoft líder a nivel mundial para la explotación de datos, enmarcada dentro de **Fabric**, una arquitectura integral en la nube orientada al uso y transformación de grandes volúmenes de datos.

**Sus componentes principales son:**

- **Power BI Service (El corazón):** Es el servicio alojado en la nube y funciona como el centro de la herramienta para consumidores finales, donde se administra la seguridad, las configuraciones de desarrollo y el acceso a los tableros.
- **Power BI Desktop:** Es la versión clásica instalable en computadora para desarrollo local. Aunque hoy la herramienta está migrando agresivamente para que todas las funciones de desarrollo estén directamente en la web, el Desktop se mantiene para casos puntuales.
- **Aplicaciones Móviles:** Aplicaciones nativas para iOS, Android y Windows que permiten visualizar la información interactiva desde teléfonos o tablets.
- **Power BI Gateway:** Es una herramienta puente o "puerta de enlace".

**¿Qué hace Power BI Gateway?** En entornos corporativos, las bases de datos o archivos (Excel, CSV) suelen estar protegidos en servidores locales (on-premise), detrás de un firewall o una VPN privada. El **Power BI Gateway actúa como un vínculo seguro entre la red privada de la organización y Power BI Service en la nube**, permitiendo que los datos de los tableros se actualicen (refresh) sin exponer la red corporativa a vulnerabilidades. Si los datos ya se encuentran en la nube (como Azure o Google Sheets), no se necesita pasar por el Gateway.

### 2. Roles en Power BI

**Roles para Usuarios Finales (Consumidores):**

- **Acceso y Consumo:** Acceden a Power BI Service mediante un usuario y clave para interactuar con reportes (reportes detallados tipo sábana) o dashboards (tableros resumidos de la situación actual).
- **Análisis y Decisiones:** Su objetivo principal es monitorear los indicadores y tomar decisiones de negocio.
- **Seguridad y Segmentación:** Interactúan en un ambiente seguro donde la información está filtrada. Por ejemplo, se puede restringir para que un grupo de usuarios solo vea los datos de una provincia específica, mientras otro grupo ve otra.

**Roles para Desarrolladores / Arquitectos:**

- **Construcción del modelo:** Se encargan de conectarse a las fuentes de datos (archivos, bases de datos, APIs), realizar limpiezas complejas y transformar la información.
- **Lógica de negocio:** Crean el modelo de datos (hoy llamado modelo semántico) estableciendo el relacionamiento entre tablas y creando funciones matemáticas complejas.
- **Distribución:** Son los responsables de diseñar visualizaciones, publicar los tableros y gestionar la administración de accesos.

### 3. La Arquitectura Interna: Power Query, Power Pivot y Power View

La arquitectura de transformación de Power BI, que actúa una vez que la información entra por los "conectores", se divide en tres fases fundamentales:

**A. Power Query (Limpieza y Transformación)** Es el entorno inicial que permite extraer, filtrar, combinar y modificar los datos provenientes de múltiples orígenes (por ejemplo, mezclar un CSV con una base relacional y métricas de Instagram). Trabaja de fondo con el **lenguaje de programación "M"**, el cual automatiza todos los pasos aplicados.

- _Ejemplo 1:_ Usar la primera fila de un Excel como encabezado de columna, un paso que Power Query mapea automáticamente junto con los tipos de datos (texto, número, fecha).
- _Ejemplo 2:_ Reemplazar valores incorrectos. Si en una columna aparece "mediana", hacer clic derecho y reemplazar por "media".
- _Ejemplo 3:_ Dividir columnas. Una columna de "región" se puede dividir usando un delimitador (como un punto y coma) para separar la celda en tres columnas nuevas: "provincia", "región" y "país".
- _Ejemplo 4:_ Crear columnas condicionales. Programar una regla que diga: si la columna "cantidad" es menor a 25, escribir "bajo"; si no, escribir "alto".
- _Ejemplo 5:_ Columnas calculadas básicas, como tomar la columna "Venta final", multiplicarla por el "Margen" y generar una nueva columna decimal llamada "Ganancia bruta".

**B. Power Pivot (Modelado de Datos)** Una vez que los datos están limpios, pasan a Power Pivot. Aquí el desarrollador actúa como arquitecto, buscando relacionar las diferentes tablas (generalmente bajo un modelo estrella) y creando medidas o métricas de análisis. Utiliza el **lenguaje de expresiones DAX** para generar lógicas matemáticas o de texto avanzadas. El resultado final de esta etapa se denomina **Modelo Semántico**.

- _Ejemplo DAX:_ Crear una función propia que tome un valor en dólares y una fecha, y lo convierta a pesos argentinos; o una función que tome el DNI y el sexo de una persona y devuelva automáticamente el número de CUIL.

**C. Power View (Explotación Visual)** Es el motor embebido encargado de construir la visualización interactiva de datos y el análisis visual (forecast), integrándose actualmente con herramientas como Copilot, Python o R.

- _Ejemplo 1:_ Construir un gráfico combinado que muestre barras evolutivas de "Ventas" y una línea de tendencia de "Cantidades" segmentado mes a mes.
- _Ejemplo 2:_ Agregar segmentadores (filtros obligatorios con menús desplegables) para que el usuario deba elegir un año específico (ej. 2025) y ver cómo varían los productos tecnológicos.
- _Ejemplo 3:_ Tooltips o información sobre herramientas (vistas detalle), que permiten que al apoyar el ratón sobre una barra de "prioridad baja", aparezca automáticamente un mini gráfico explicando los tipos de embalaje que se usaron para esa venta.

### 4. Flujo de Trabajo en Power BI

El ecosistema completo sigue siempre estos 5 pasos inmutables:

1. **Conexión:** Extraer información usando los más de 50 conectores a fuentes externas.
2. **Limpieza:** Transformar la información en bruto con Power Query.
3. **Modelado:** Establecer relaciones y reglas de negocio creando el Modelo Semántico (Power Pivot).
4. **Visualización:** Generar los tableros gráficos de explotación (Power View).
5. **Compartir:** Publicar los tableros al Power BI Service para que la organización los consuma de forma colaborativa.

---

### 5. Preguntas y Respuestas para Exámenes (Q&A)

**P1: ¿Qué es el "Perfil de la Columna" (Column Profile) en Power Query y para qué sirve?** **R:** Es una herramienta analítica que evalúa los datos de una columna específica mostrando su distribución (ej. si tiene forma de campana de Gauss), calculando promedios, desviaciones estándar, y exponiendo anomalías como el porcentaje de celdas vacías (nulos) o errores de formato de datos. Sirve para entender los datos crudos antes de comenzar a limpiarlos.

**P2: ¿Cuáles son las diferencias conceptuales entre Power Query y Power Pivot?** **R:** Power Query es la herramienta enfocada en extraer, limpiar y transformar la estructura de los datos brutos, utilizando el lenguaje "M". Power Pivot toma esos datos limpios para modelarlos, relacionar tablas y crear métricas avanzadas bajo el lenguaje "DAX", dando origen al modelo semántico final.

**P3: ¿Es posible alimentar múltiples reportes diferentes a partir de la misma carga de datos?** **R:** Sí, un único "Modelo Semántico" (el repositorio central de tablas y métricas) puede estar vinculado a múltiples informes diferentes. Esto asegura que si el modelo semántico original se actualiza, todos los tableros que dependan de él reflejarán los nuevos cambios simultáneamente, garantizando la consistencia de los datos.

**P4: Si una empresa almacena la base de datos de recursos humanos en servidores físicos en su propia oficina, ¿cómo se conecta Power BI para actualizar los datos automáticamente?** **R:** A través de Power BI Gateway, que se debe instalar localmente para crear una puerta de enlace cifrada. Esto permite que el servicio web en la nube cruce los firewalls de manera segura y lea los servidores físicos locales de la empresa.

**P5: En la experiencia del usuario final, ¿qué alternativas ofrece Power BI más allá de navegar por la web?** **R:** Los usuarios pueden utilizar la vista móvil generada mediante la App nativa de teléfonos, exportar los tableros a formato PDF estático, o integrarlos en vivo dentro de diapositivas de PowerPoint, lo cual permite interactuar con los datos directamente en una presentación.

---

### 6. Lecciones Aprendidas de la Clase Práctica

- **Cada acción es código automatizable:** Una lección crucial al limpiar datos en Power BI es que el menú visual (pasos aplicados) es solo una interfaz para el código "M" subyacente. Esto significa que si agregas una columna y te equivocas, puedes borrar simplemente ese paso en el historial, o si sabes programar, puedes ir al _Editor Avanzado_, modificar el código y reutilizar ese fragmento (script) pasándoselo a otros colegas.
- **Anticipar el crecimiento de los datos:** Gracias al sistema de "pasos aplicados" en cascada, la próxima vez que te conectes al archivo y este contenga más registros, Power BI ejecutará secuencialmente cada uno de los pasos guardados (ej. limpieza de texto o conversión a decimales), procesando los nuevos datos de forma totalmente automática.
- **Separar la información de su representación:** Un reporte de ventas en el lienzo visual no almacena datos; solo los lee. Si alguien altera manualmente los datos subyacentes, el informe no cambiará hasta que se ordene una actualización del "Modelo Semántico". Es el modelo semántico el dueño de la información.
- **Fácil adaptabilidad móvil:** No es necesario reconstruir funciones visuales complejas para celulares. Desde el modo de diseño móvil, los objetos visuales interactivos ya creados simplemente se arrastran y se apilan en un diseño vertical, reteniendo por completo los filtros y comportamientos de la vista de computadora.