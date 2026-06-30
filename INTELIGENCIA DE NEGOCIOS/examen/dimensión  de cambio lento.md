Para mantener toda la historia de los datos en una Dimensión de Cambio Lento (Slowly Changing Dimension o SCD), se debe aplicar el método de **SCD Tipo 2**.

Al aplicar una dimensión de **Tipo 2**, cada vez que se detecta una modificación en un atributo (por ejemplo, si un cliente cambia de estado civil o se muda), se deben realizar las siguientes acciones:

- **Generar una nueva Clave Subrogada (SK):** En lugar de sobrescribir el registro existente, se inserta una nueva fila en la base de datos con el valor actualizado, asignándole una nueva Clave Subrogada.
- **Agregar campos de vigencia:** Para administrar correctamente la historia y saber qué registro usar en qué momento del tiempo, es indispensable agregar columnas adicionales en la tabla, siendo la forma más recomendada el uso de **fechas de vigencia (desde y hasta)**. El registro original tendrá una fecha de cierre (el día anterior al cambio), y el nuevo registro tendrá una fecha de inicio desde el momento del cambio hasta el "fin de los tiempos" (un campo que suele dejarse vacío o con una fecha lejana como el año 2900 o 3000).
- **Marcas opcionales:** También es común complementar estas fechas agregando el número de **versión** del registro, o una marca (como un campo booleano o un flag) que indique fácilmente cuál es el registro **último o actual/vigente**.

De esta manera, la historia queda guardada y permite que los análisis o campañas posteriores (o incluso modelos de _machine learning_) utilicen el dato correcto asociado al momento exacto en el que ocurrió la transacción.

A diferencia de este método, si utilizaras el **Tipo 0** no harías nada ante un cambio (quedando el dato desactualizado), y si aplicaras el **Tipo 1** simplemente reemplazarías (sobrescribirías) el dato, perdiendo para siempre el historial anterior.