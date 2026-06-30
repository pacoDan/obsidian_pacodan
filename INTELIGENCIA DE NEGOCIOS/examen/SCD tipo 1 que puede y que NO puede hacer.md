En el método de **SCD Tipo 1 (Dimensión de Cambio Lento Tipo 1)** no se realiza una operación de eliminación física (`DELETE`) de la fila por razones que abarcan tanto la definición propia de la técnica como los principios de rendimiento de un Data Warehouse:

- **La técnica se basa en actualizar (UPDATE), no en borrar:** La regla fundamental del SCD Tipo 1 es que, ante una modificación, simplemente se debe **reemplazar o reescribir** el registro existente. A nivel técnico en la base de datos, si el registro ya existe, la instrucción que se ejecuta es un `UPDATE` sobre el campo específico que cambió, sobrescribiendo el valor anterior para siempre.
- **El borrado es la operación más costosa:** Desde el punto de vista de la ingeniería de datos y el rendimiento (tuning), la ejecución de borrados (`DELETE`) es el proceso más pesado, lento y costoso para un motor de base de datos relacional. Le sigue el `UPDATE`, siendo el `INSERT` la operación más rápida. Intentar limpiar o manejar cambios haciendo borrados masivos degradaría severamente la performance del sistema.
- **Naturaleza "No Volátil":** Por definición arquitectónica, un Data Warehouse es un entorno **no volátil**, lo que significa que los datos se almacenan para perdurar en el tiempo y no se deben borrar físicamente, ya que a menudo sirven como historial o auditoría.
- **Pérdida de Integridad Referencial:** Si se eliminara un registro de una dimensión mediante un `DELETE`, se rompería la relación con todos los millones de registros históricos en la tabla de hechos (Fact Table) que ya estaban apuntando a la Clave Subrogada (SK) de esa dimensión eliminada. Mantener o controlar esa integridad referencial ante borrados en volúmenes masivos de datos es inviable.

Por lo tanto, en un escenario de Tipo 1, **el registro original se mantiene vivo y conserva su Clave Subrogada**, aplicándose únicamente una actualización (`UPDATE`) sobre el texto o atributo que cambió.

El método de **Dimensión de Cambio Lento Tipo 1 (SCD Tipo 1)** tiene un enfoque muy directo sobre cómo manejar las modificaciones en los atributos, lo cual define claramente sus capacidades y limitaciones dentro de la arquitectura de un Data Warehouse.

**Lo que PUEDE hacer:**

- **Mantener el dato más reciente o actual:** Cuando se detecta un cambio (por ejemplo, si una marca de bebida cambia de nombre o un cliente cambia de estado civil), el método simplemente reemplaza el dato viejo por el nuevo.
- **Simplificar la administración y el proceso de carga:** Es la forma técnica más fácil de manejar los cambios. A nivel de base de datos, solo requiere ejecutar una actualización (_update_) directa sobre el registro que ya existe.
- **Evitar el crecimiento de las tablas:** Al no generar nuevas filas ni nuevas Claves Subrogadas (SK) por cada modificación, mantiene el volumen de datos reducido y no requiere lógicas de control complejas.

**Lo que NO PUEDE hacer:**

- **No puede conservar el historial de los datos:** Su principal característica es que **reescribe (pisa) el registro**, por lo que el valor anterior se elimina y se pierde para siempre.
- **No permite realizar análisis históricos precisos:** Al sobrescribir la información, todas las transacciones del pasado quedan automáticamente atadas al nuevo estado. Por ejemplo, si un cliente realizó compras siendo "soltero" y hoy su estado civil se actualiza a "casado" usando el Tipo 1, los listados y reportes mostrarán que todas esas compras pasadas fueron realizadas por alguien "casado". Esto vuelve imposible analizar el comportamiento real que tenía el cliente en el momento exacto de la transacción.

Por esta razón, la decisión de utilizar el SCD Tipo 1 recae en los usuarios del negocio. Las fuentes recomiendan aplicarlo únicamente a **datos anecdóticos** o atributos donde mantener la historia sea irrelevante y solo interese conocer la última versión de la información.


