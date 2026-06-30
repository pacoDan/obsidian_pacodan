Aquí tienes la transcripción estructurada al detalle, pensada como una guía de estudio integral para exámenes, con lecciones aprendidas, conceptos clave, ejemplos y recomendaciones del profesor.

### 1. Conceptos Clave de Modelado de Datos

**Granularidad**

- **Definición práctica:** Es el nivel de detalle mínimo con el que se van a guardar los datos.
- **Ejemplo de la clase:** Si originalmente analizas los datos a nivel de "sucursal", pero decides agregar la "terminal" (caja), estás bajando la granularidad. Esto es un cambio drástico en el modelo porque ahora el nivel mínimo de detalle ya no es la sucursal, sino cada terminal individual.

**Hechos (Facts)**

- Los hechos representan los eventos de negocio que se quieren medir.
- **Ejemplos:** La "Facturación" o las "Ventas" son hechos. Los "Pedidos" también son hechos, pero ocurren en un momento diferente al de la facturación, por lo que van en tablas separadas.
- Al definir un hecho (ej. Facturación), siempre hay que preguntar al negocio las métricas: ¿Se medirá en importes o unidades? ¿En pesos o en dólares? ¿Con o sin impuestos?.

**Dimensiones**

- Son los ejes por los cuales se va a analizar la información del hecho (el _quién, qué, dónde, cuándo_).
- **Ejemplos de la clase:**
    - _Geográfica:_ Región -> Provincia -> Sucursal.
    - _Producto:_ No basta con el nombre; hay que preguntar si se necesita rubro, tamaño, color, material o marca.
    - _Actores de venta:_ En la clase se diferenció entre el **Vendedor** (quien emite la factura), el **Oficial de Cuenta** (quien consigue al cliente y se asocia como "padre" del cliente) y el **Responsable de Venta** (que puede ser el gerente de sucursal o región).

### 2. Diferencias entre Esquema Estrella y Copo de Nieve

**Modelo Estrella (Star Schema)**

- **Estructura:** Tienes la tabla de hechos (Fact Table) en el centro y las dimensiones alrededor, pareciendo una estrella.
- **Física:** Se hace **una tabla física por cada dimensión completa**.
- **Ventajas/Desventajas:** Genera mucha redundancia de datos (los datos se repiten), pero es muchísimo más rápido y fácil de consultar o llenar. Al profesor le gusta más para trabajar con herramientas como Power BI.
- **Ejemplo:** Un modelo estrella podría tener solo 6 tablas en total.

**Modelo Copo de Nieve (Snowflake Schema)**

- **Estructura:** Las dimensiones se normalizan y se ramifican, pareciendo un copo de nieve.
- **Física:** Se hace **una tabla física por cada atributo** o jerarquía.
- **Ventajas/Desventajas:** Elimina la redundancia de datos, pero requiere hacer actualizaciones (updates) mucho más complejas y genera muchas más tablas.
- **Ejemplo:** El mismo modelo que en estrella tenía 6 tablas, en copo de nieve pasa a tener 13 tablas.

### 3. Preguntas y Respuestas de Examen (Surgidas en Clase)

- **Pregunta:** ¿"Pedidos" no es un hecho que está dentro de facturación?
    - **Respuesta:** No. Pedidos y Facturación son dos hechos diferentes que ocurren en momentos distintos. No van en la misma tabla porque un pedido entra en un nivel genérico y luego se factura.
- **Pregunta:** ¿El "Oficial de Cuenta" y el "Responsable de Ventas" son lo mismo?
    - **Respuesta:** No. El oficial de cuenta está asociado al cliente (es quien lo consigue), mientras que el responsable de ventas o el vendedor están asociados a la venta/factura. Todos pueden ser dimensiones separadas o jerarquías dependiendo de la granularidad.
- **Pregunta:** ¿Por qué se llama Estrella y Copo de Nieve?
    - **Respuesta:** Es por la representación gráfica. Estrella tiene la tabla de hechos en el medio y las tablas de dimensiones directo alrededor. Copo de nieve tiene atributos que se ramifican hacia afuera, dándole la forma de un cristal de nieve.
- **Pregunta:** ¿Se deben usar bases de datos relacionales con Integridad Referencial (Foreign Keys) en el Data Warehouse?
    - **Respuesta:** El profesor **no lo recomienda**. Exigir integridad referencial a nivel de base de datos hace que las inserciones sean muy lentas. Es mejor hacer el control de calidad de datos (ETL) antes, e insertar los datos limpios de forma rápida.
- **Pregunta:** ¿Qué pasa si hago distintos análisis sobre los mismos datos, los pongo todos en un solo lugar?
    - **Respuesta:** Tendrás diferentes modelos (ej. modelo de ventas, modelo de stock) que comparten **dimensiones conformadas** (ej. la dimensión de Tiempo o Cliente), lo que permite cruzar y comparar la información entre modelos.

### 4. Errores Críticos: Lo que NO se debe cometer

1. **Guardar fechas como texto:** Esto es un "asesino de rendimiento" (performance killer). Además, trae problemas de casteo (conversión) porque en países como Estados Unidos o México el mes va primero, lo que rompe los sistemas al intentar hacer cálculos de diferencias de días.
2. **Usar claves subrogadas (auto-incrementales) puras para la Dimensión Tiempo:** Si le pones "1, 2, 3..." a las fechas, hacer una simple consulta para "saber cuánto se vendió hoy" obliga a cruzar tablas. Es mejor usar una clave numérica inteligente como `20260101` (YYYYMMDD) que a nivel de base de datos pesa lo mismo que un entero, pero humanamente es legible y rápida.
3. **Confiar ciegamente en la Inteligencia Artificial:** El profesor cuenta que le pidió a Gemini generar un modelo (DER) y la IA inventó entidades que no estaban en los requerimientos u omitió otras. La IA sirve para programar rápido, pero **el humano debe entender el negocio** para saber qué pedirle y qué auditar.
4. **No validar suposiciones:** Nunca asumas nada. Si te dicen que van a medir por "mes y año", pregunta si no querrán ver también el detalle por "día" o "mañana/tarde" para el futuro, porque reconstruir eso después es costoso.

### 5. La Dimensión Tiempo: Ejemplos de su extrema importancia

La dimensión Tiempo no es solo "Día, Mes y Año". Suele tener hasta 20 atributos o más, y es vital entender la lógica del negocio:

- **Días Laborales vs Días Calendario:** Si comparas ventas, no puedes comparar el día 1 de este mes (domingo) contra el día 1 del mes pasado (lunes). Debes comparar el _Día Laboral 1_ contra el _Día Laboral 1_.
- **El negocio de la salud (Enfermería):** Es clave tener atributos de fin de semana y feriados, ya que hay enfermeras "sadófenas" (que solo trabajan sábados, domingos y feriados).
- **Sistemas Bancarios:** Los días no cortan a las 12 PM. El "día bancario" puede empezar a las 17:00 de hoy y terminar a las 16:59 de mañana (o a las 16:00 en redes internacionales como Cirrus). Esto debe estar reflejado en la dimensión tiempo.
- **Año Fiscal:** No siempre va de Enero a Diciembre. Muchas empresas van de Julio a Junio o de Abril a Marzo.
- **Tarjetas de Crédito:** Manejan cortes por ciclos (ej. Ciclo 22, Ciclo 30) y no por meses calendario perfectos.

### 6. Lecciones Aprendidas y Tips del Profesor

- **"Conozca el negocio, conozca el negocio, conozca el negocio"**: Esta fue la regla de oro repetida por el profesor. Hoy la IA puede escribir código (DAX, Python, SQL), pero si no caminas la fábrica o no entiendes funcionalmente cómo se paga o quién cobra comisiones, tu modelo no servirá de nada.
- **Dibuja el proceso:** Aunque el usuario te pida medir solo la "facturación", pregúntale cómo nace el pedido y cómo termina en la cobranza. Dibujar el ciclo completo en papel te da una visión global y te evita errores futuros.
- **El modelo físico (para Power BI):** Power BI funciona muchísimo mejor y está optimizado para procesar modelos Estrella.

### 7. Tips y Recomendaciones para el Examen / Práctica

- **Sobre el parcial:** Ante la duda de un alumno ("cómo es el parcial"), el profesor indicó que no se preocupen. La parte práctica del examen involucrará la creación del modelo físico. Una consigna típica será: _"Liste las dimensiones o sus jerarquías, anímense a dibujarlas"_ (no necesariamente los atributos, pero sí el esqueleto dimensional).
- **Material de Práctica:** El profesor subirá ejercicios resueltos (llamados "pagaré uno" y "pagaré dos") que incluyen la resolución en Modelo Estrella y Copo de Nieve paso a paso para que puedan comparar las diferencias y practicar en casa.
- **Herramientas a usar:** Para las clases prácticas y probablemente para las evaluaciones futuras, no necesitarán instalar Power BI Desktop; usarán Power BI en la nube vinculado a un Google Drive/Google Sheets proporcionado por la cátedra a través de la cuenta de la UTN.