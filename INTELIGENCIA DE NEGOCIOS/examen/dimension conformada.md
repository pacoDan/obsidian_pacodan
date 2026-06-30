Una **dimensión conformada** es aquella **dimensión que es compartida por dos o más Datamarts** (o modelos de negocio diferentes) dentro de la arquitectura de un Data Warehouse. Conceptualmente, funcionan como los **puntos de encuentro o enlaces entre varios submodelos**.

**¿Para qué sirve?** Su objetivo fundamental es **permitir el cruce y la integración de información proveniente de diferentes tablas de hechos en un único informe**. Al estar unificada y mantener el mismo nivel de significado para toda la organización, garantiza que los datos de distintos procesos de negocio se puedan comparar alineados bajo los mismos atributos.

- **Ejemplo práctico:** Si posees un Datamart de _Ventas_ y un Datamart de _Stock_, y ambos comparten las dimensiones "Geografía" y "Producto" (dimensiones conformadas), podrás construir un reporte consolidado. En este reporte, al filtrar por una sucursal específica (Geografía) y una categoría específica (Producto), podrás ver en columnas contiguas cuántas unidades vendiste y cuánto stock valorizado te queda disponible.
- Otro cruce típico habilitado por las dimensiones conformadas es comparar las _Ventas reales_ frente al _Presupuesto objetivo_, utilizando las dimensiones compartidas de Vendedor, Producto y Tiempo.

**¿Cómo se identifican? (El método de la matriz)** En modelos de Data Warehouse grandes donde existen muchos Datamarts, se utiliza una técnica visual conocida como el **método de la matriz** para identificar estas dimensiones:

1. Se dibuja una tabla donde las **filas** representan todos los Datamarts del negocio (ej. Ventas, Stock, Reclamos, Facturación).
2. En las **columnas** se listan todas las dimensiones relevadas en la compañía.
3. Se marca (con una cruz o tilde) qué dimensiones son utilizadas por cada Datamart.
4. Cualquier dimensión (columna) que **tenga marcas en dos o más Datamarts** es catalogada automáticamente como una **dimensión conformada**.

Las dimensiones que casi siempre resultan conformadas por naturaleza en cualquier empresa son el **Tiempo** (Fecha/Mes/Año), la **Geografía** y los **Productos/Servicios**. Por el contrario, aquellas dimensiones que solo reciben una marca (pertenecen a un solo Datamart) no se consideran conformadas.



