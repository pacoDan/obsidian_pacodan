No, no siempre estará estrictamente limitada a claves foráneas e indicadores. Si bien la estructura fundamental de una tabla de hechos (o _Fact Table_) está compuesta por **una columna por cada hecho o indicador** (los valores cuantitativos a medir) y **una columna de clave foránea por cada atributo** que define su nivel de granularidad, existe una excepción muy común: las **dimensiones degeneradas**.

Las **dimensiones degeneradas** son atributos que se almacenan **directamente como una columna dentro de la tabla de hechos** sin contar con una tabla física de dimensión separada (tabla _lookup_). Esto se hace cuando un dato es útil para el análisis o para listar transacciones, pero no justifica crear una dimensión independiente porque no tiene jerarquías, no tiene atributos descriptivos adicionales, o porque generaría una tabla innecesariamente gigante.

Por lo tanto, dentro de tu tabla de hechos también puedes encontrar directamente:

- **Número y tipo de comprobante:** como guardar la letra de una factura (A, B, C, remito) y su número exacto.
- **Identificadores operativos:** como el número de transacción, el número de trámite o el número de terminal POS donde se realizó la operación.
- **Fechas adicionales:** además de la clave de fecha principal, la tabla de hechos puede guardar columnas directas para la fecha de entrega, fecha de envío o de recepción del producto.
- **Campos de texto libre:** como una columna para guardar las observaciones de la transacción.
- **Atributos volátiles (Monster Dimension):** valores que cambian todo el tiempo, como el puntaje de crédito o puntaje de compras de un cliente al momento exacto de la transacción. Para no saturar la dimensión de clientes con millones de cambios, a veces se opta por guardar el valor atómico de esos puntos como una columna más en la tabla de hechos.
- 