Los casos problemáticos **no se descartan sin dejar rastro ni se omite su logueo**.

En primer lugar, los datos extraídos se guardan exactamente tal cual vienen de los sistemas de origen en la capa temporal inicial (área de _Staging_ o capa de Bronce), copiando el archivo o dato original para no perder su estado inicial. Esto se hace específicamente para mantener una auditoría y permitir que, si ocurre una falla o error, se pueda volver a recargar la información y hacer una "ingeniería inversa" del problema.

En segundo lugar, cuando los datos pasan a la etapa de procesamiento y limpieza (conocida en arquitecturas modernas como capa de Plata o _Silver_), es allí donde se aplican las reglas de negocio y se generan los datos adicionales de auditoría. El sistema debe registrar y loguear todo lo que ocurre: qué día se realizó la carga, **si un registro dio error**, cuántos registros fueron insertados en total y si hubo registros modificados.

Además, el profesor resalta que durante el proceso de carga de datos (ETL), especialmente cuando hay redundancia de información (como ocurre al alimentar tablas agregadas), es fundamental prestar mucha atención a los **procesos de control de datos y de logueo** para identificar que no haya problemas o diferencias en la carga.

Por lo tanto, la arquitectura exige mantener siempre la información cruda por seguridad y generar logs (auditorías) de los errores y transformaciones aplicadas, garantizando que un dato que no cumple las reglas no se pierda simplemente en el vacío.


----


Basándome en las transcripciones, el profesor hizo una aclaración explícita sobre este tema: **decidió no profundizar en las reglas específicas para solucionar problemas de calidad de datos** (como qué hacer exactamente si viene una fecha inexistente, como un 29 de febrero en un año no bisiesto), indicando que es algo que aprenderán en la vida real y no consideró importante enseñar en clase todas las reglas posibles de corrección.

Sin embargo, basándose en los principios de arquitectura, rendimiento y modelado que el profesor remarcó fuertemente a lo largo de las clases, **las siguientes son soluciones que absolutamente NO deben seguirse** ante registros problemáticos:

- **NO se deben aplicar las reglas de negocio y limpieza filtrando directamente sobre el sistema de origen (OLTP):** Si se intenta procesar, limpiar o rechazar registros incompatibles consultando directamente a la base de datos productiva durante la extracción, se afectará el rendimiento del sistema transaccional, ralentizando la operatoria de la empresa. Los datos problemáticos (y los correctos) siempre deben llevarse primero, lo más rápido y "crudos" posible, a un área temporal (_Staging_ o capa de Bronce) y desde nuestro lado recién aplicar las transformaciones.
- **NO se deben eliminar físicamente los datos crudos ni descartarlos sin dejar rastro:** La arquitectura moderna (como la Medallion Architecture) establece que los datos siempre deben guardarse tal cual vienen en la capa de Bronce (_Staging_ o _Raw_). Esto se hace para mantener una auditoría estricta y permitir que, si un proceso de transformación falla o hay registros erróneos, se pueda recargar la información y hacer ingeniería inversa ante el fallo. Además, a nivel de base de datos, el profesor recalcó que realizar operaciones de borrado (_Deletes_) para limpiar datos es el proceso más costoso y que peor rendimiento genera.
- **NO se debe rechazar la transacción ni frenar todo el proceso por falta de un dato dimensional:** El proceso de ETL usualmente cuenta con una ventana de tiempo muy acotada durante la madrugada (por ejemplo, de 2 a 6 de la mañana) para terminar la carga. Abortar un proceso por errores de integridad detendría la disponibilidad de datos. En lugar de descartar un registro de venta porque no cumple la regla de encontrar su dimensión padre, se recomienda tener en la tabla de dimensiones un elemento genérico con el valor **"Desconocido"** para asignar esos registros que vienen vacíos o con errores de cruce.

Si estás ante una pregunta de examen tipo _múltiple choice_ con opciones específicas, la respuesta correcta sobre qué **NO** hacer generalmente apunta a **no detener la carga completa** o a **no aplicar la validación sobre el sistema transaccional de origen**.