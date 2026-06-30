La **calidad de datos (o Data Quality)** se refiere al conjunto de **reglas, chequeos y procesos de limpieza que se aplican a la información extraída** para garantizar que los datos sean confiables, coherentes y estén completos antes de ser analizados.

Según lo expuesto en las clases, el trabajo de calidad de datos involucra principalmente las siguientes acciones:

- **Data Profiling (Perfilado de datos):** Es un **diagnóstico estadístico y analítico de las fuentes originales** que permite conocer el estado real de la información. Sirve para entender la distribución de las columnas, detectar la cantidad de valores nulos o vacíos, identificar errores de tipeo y validar los tipos de datos.
- **Validación de coherencias y reglas de negocio:** Implica programar controles para **evitar que ingresen datos ilógicos o incongruentes** al Data Warehouse. Algunos ejemplos clásicos de estos controles son verificar que una edad sea mayor a cero, que una fecha de nacimiento no sea posterior a la fecha actual, o lidiar con fechas inexistentes en los sistemas de origen (como un 29 de febrero en un año no bisiesto).
- **Equilibrio entre limpieza y velocidad (Costo-Beneficio):** Existe una relación directa entre la profundidad del chequeo de calidad y el rendimiento del proceso de carga. **Cuantas más reglas de completitud y limpieza se apliquen en el medio, menor será la velocidad de carga**, lo que retrasará la disponibilidad de los datos para los usuarios. Por ende, siempre se debe buscar un balance óptimo.

Estos procesos de calidad y limpieza **nunca se ejecutan directamente sobre el sistema transaccional** productivo, sino que se aplican en áreas intermedias (como el área de Staging o la Capa Silver) para no afectar la operatoria diaria de la empresa.


