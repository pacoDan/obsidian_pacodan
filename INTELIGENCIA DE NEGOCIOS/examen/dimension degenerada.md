Una **dimensión degenerada** es aquella **dimensión que no cuenta con una tabla física separada** (tabla _lookup_) en el modelo de datos. En lugar de crear una tabla dimensional independiente, **estos datos se guardan directamente como una columna dentro de la tabla de hechos (FAC)**.

**¿Para qué sirve?** Sirve para **almacenar información que es necesaria para analizar o listar transacciones, pero que no justifica la creación de una dimensión propia**. Esto suele aplicarse a identificadores únicos de transacciones que generarían tablas dimensionales monstruosas y poco performantes (con millones de registros solo para guardar un número) o a campos aislados que no poseen jerarquías ni características descriptivas adicionales.

**Ejemplos de dimensiones degeneradas:**

- **Número y tipo de comprobante** (ej. Factura A o B y su número identificador).
- **Número de transacción** o número de trámite de una operación.
- **Número de POS** (Punto de venta o terminal de Postnet) donde se realizó la compra.
- **Fechas adicionales** vinculadas a una venta, como pueden ser la fecha de entrega, fecha de envío o fecha de recepción del producto.
- **Puntajes de crédito o de compra** asociados al cliente en el momento de la transacción.
- Campos de texto de ingreso libre, como una columna de **observaciones**.