El método de **Dimensión de Cambio Lento Tipo 2 (SCD Tipo 2)** es la técnica que se aplica cuando el negocio decide que necesita **guardar toda la historia** de las modificaciones en los atributos de una dimensión para poder realizar análisis históricos precisos, campañas de marketing o alimentar modelos predictivos (como _machine learning_).

Como repasamos anteriormente en nuestra conversación, cuando se detecta un cambio (por ejemplo, el estado civil de un cliente que pasa de "soltero" a "casado"), el **SCD Tipo 2** realiza las siguientes acciones estructurales en la base de datos:

- **Generación de una nueva Clave Subrogada (SK):** A diferencia del Tipo 1 que sobrescribe el dato, el Tipo 2 **no borra nada, sino que inserta una nueva fila** con el nuevo valor (ej. "casado") y le asigna una nueva Clave Subrogada. Esto asegura que las transacciones pasadas queden atadas al SK antiguo (soltero) y las transacciones nuevas se aten al SK nuevo (casado).
- **Campos de fechas de vigencia (Desde y Hasta):** Para administrar este historial y saber en qué momento exacto fue válido cada registro, se deben agregar columnas de fechas. El registro original recibe una fecha de fin de vigencia (el día anterior al cambio), mientras que el nuevo registro se marca como válido desde la fecha del cambio hasta el "fin de los tiempos" (un valor que suele dejarse vacío o completarse con un año lejano, como el 2900 o 3000).
- **Marcas de estado y versiones (Opcionales):** Para facilitar las consultas de los usuarios y no obligarlos a buscar por fechas, es muy común y recomendable agregar un campo con el **número de versión** del registro (para contar cuántas veces cambió) y una **marca o _flag_ booleano (0 o 1)** que indique directa y visualmente cuál es el registro activo o vigente.

Aplicando esta estructura, el modelo garantiza que nunca se pierda el contexto en el que ocurrieron las transacciones originales.


