Si tienes una tabla lookup (LK) en un modelo **estrella** (donde toda la información de la jerarquía de la dimensión está agrupada en una sola tabla con alta redundancia), el proceso de normalizarla significa transformarla hacia un esquema **copo de nieve (Snowflake)**,. A esta técnica específica de extraer partes de una dimensión para normalizarlas se la conoce conceptualmente como la creación de una **subdimensión**,.

El proceso paso a paso para normalizar esta tabla sería el siguiente:

1. **Identificar la redundancia de atributos:** Debes analizar tu dimensión en estrella para detectar qué datos se repiten excesivamente y tienen una baja frecuencia de cambio. El profesor da el ejemplo de una dimensión de "Clientes" de 10 millones de registros que incluye los datos del país (nombre del país, superficie, habitantes, presidente). Si 9 millones de esos clientes son de Argentina, los datos del país estarán repetidos 9 millones de veces, generando una gran redundancia.
2. **Dividir y crear la nueva tabla (Subdimensión):** Extraes esos atributos específicos de la tabla original y generas una nueva dimensión física separada (por ejemplo, una nueva tabla _LK País_).
3. **Vincular jerárquicamente mediante Claves:** A la nueva tabla le asignas una Clave Subrogada (SK) propia, y en tu dimensión original de Clientes eliminas los textos descriptivos para dejar únicamente el ID (el puntero o clave foránea) que la vincula con su nueva tabla padre,.

**Impactos y consecuencias de aplicar esta normalización:**

- **Facilita el mantenimiento y ahorra almacenamiento:** Al eliminar la redundancia, evitas realizar actualizaciones masivas y costosas. Si la cantidad de habitantes del país cambia, ahora solo actualizas **un** registro en tu nueva subdimensión, en lugar de ejecutar un _update_ sobre 9 millones de filas en la dimensión de clientes,.
- **Mantiene la granularidad intacta:** Un punto clave de este proceso es que **no altera la Tabla de Hechos (Fact Table)**. El nivel de granularidad original del modelo se mantiene exactamente igual, ya que la tabla de hechos sigue apuntando a la dimensión de nivel inferior.
- **Aumenta la necesidad de realizar _Joins_:** La principal penalización de este proceso es el rendimiento de las lecturas. Para que un usuario pueda listar las ventas junto con el presidente del país, el motor de la base de datos ahora está obligado a ejecutar un salto o _join_ adicional entre las tablas dimensionales,.

Finalmente, dependiendo de cuántas veces apliques este proceso hacia arriba en la jerarquía, el diseño se clasificará como **completamente normalizado** (similar a una cuarta forma normal tradicional, con la mínima redundancia pero máxima cantidad de _joins_ necesarios) o **moderadamente normalizado** (donde las tablas hijas conservan los IDs de sus ancestros para evitar que el motor de base de datos deba dar tantos saltos),.





