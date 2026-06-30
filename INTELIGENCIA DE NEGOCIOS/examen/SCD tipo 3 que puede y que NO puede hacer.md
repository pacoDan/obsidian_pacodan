El método de **Dimensión de Cambio Lento Tipo 3 (SCD Tipo 3)** es una técnica diseñada para cuando se desea **guardar solo un pedacito de la historia** de las modificaciones.

**Lo que PUEDE hacer:**

- **Conservar un historial parcial y acotado:** Permite mantener al mismo tiempo el valor actual del atributo y su valor inmediatamente anterior.
- **Evitar el crecimiento de registros:** En lugar de crear nuevas filas y Claves Subrogadas (SK) por cada cambio (como hace el Tipo 2), **el Tipo 3 mantiene todo en el mismo registro único**. Para lograrlo, agrega nuevas columnas físicas en la tabla de la dimensión: un campo para el "registro actual", otro para el "registro anterior" y un campo de "fecha" para saber cuándo ocurrió la modificación.
- **Facilitar comparaciones de estado directo:** Es muy útil para negocios que necesitan ver rápidamente el salto de un estado a otro. Por ejemplo, lo utilizan los proveedores de cable para comparar el plan actual contra el anterior y saber si el cliente hizo un _upgrade_ o _downgrade_ de su servicio. También lo usan las telefónicas para ver de qué operador venía el cliente, e incluso pueden agregar más columnas para guardar hasta dos o tres versiones anteriores en la misma fila.

**Lo que NO PUEDE hacer:**

- **No puede guardar TODA la historia completa:** Dado que depende de una cantidad finita de columnas creadas para albergar el pasado, su capacidad de memoria es limitada.
- **No retiene los estados iniciales ante cambios múltiples:** Si un atributo sufre varias modificaciones a lo largo del tiempo, los datos más viejos se sobrescriben y se pierden. Por ejemplo, si un cliente pasa de "soltero" a "casado", el campo anterior guardará "soltero". Pero si luego se divorcia, el campo actual dirá "divorciado", el campo anterior dirá "casado", y **el dato inicial de que alguna vez fue soltero se perderá para siempre**.


