
En el diseño de tu sistema **FarmaTracker**, la auditoría guiada se materializa a través de lo que la documentación define como la **"Auditoría de Góndola"** y la **"Hoja de Ruta"**. Este proceso digitaliza la verificación técnica y física de los medicamentos en las estanterías de la sucursal.

A continuación, te detallo cómo está estructurada esta auditoría guiada según la especificación de tu proyecto:

### 1. El Concepto Central: La Hoja de Ruta de Auditoría

El sistema cumple con el Requerimiento Funcional de "Gestión de Auditoría Guiada" mediante la creación de la **Hoja de Ruta**. Se trata de un documento logístico digital que agrupa y organiza el traslado o control de múltiples lotes. Su objetivo principal es **minimizar los desplazamientos físicos y el error humano** del empleado en el salón.

### 2. Cómo funciona el flujo (Caso de Uso CU-03)

La operatoria de la auditoría guiada sigue un flujo muy específico:

- **Generación Inteligente:** El Director Técnico solicita generar la hoja de ruta del día. El sistema no muestra los lotes al azar, sino que **agrupa los lotes con acciones pendientes por sucursal y por su ubicación exacta en la góndola**.
- **Recorrido Óptimo:** El sistema ordena estos ítems trazando el "recorrido óptimo" para el empleado.
- **Información en Pantalla:** Para cada ítem, el empleado visualiza el producto, el lote, el vencimiento, la **ubicación exacta (pasillo / góndola / nivel)** y la acción requerida que fue previamente dictaminada por el Director Técnico (ej. retirarlo para devolución o destrucción).
- **Confirmación:** Al llegar a la ubicación y completar la tarea, el empleado marca la acción como ejecutada directamente en el sistema.

### 3. Gestión de Excepciones

El sistema contempla un curso alternativo durante la auditoría física: si el empleado llega a la góndola y no puede ejecutar la acción (por ejemplo, porque el producto físico no se encuentra donde debería estar), registra una observación en el sistema, y este se encarga de notificar automáticamente al Director Técnico sobre el desvío.

### 4. Restricciones Críticas de Usabilidad (RNF)

Para que esta auditoría guiada sea ágil y no entorpezca el trabajo físico, estableciste requerimientos no funcionales (RNF) muy estrictos en cuanto a la experiencia de usuario:

- **Movilidad:** La interfaz de la hoja de ruta debe poder ejecutarse y ser completamente legible en las pantallas de **dispositivos móviles** (como teléfonos de 5 pulgadas), ya que el empleado la usará mientras camina por los pasillos.
- **Regla de los 3 Clics:** Para confirmar el estado de un lote en la góndola, el diseño exige que el empleado **no deba realizar más de tres interacciones (clics o taps)** en la pantalla.

En resumen, la auditoría guiada en FarmaTracker toma las decisiones complejas calculadas por el "Motor de Reglas" y aprobadas por el Director Técnico, y las traduce en un **paso a paso logístico secuencial, optimizado por ubicación y extremadamente simple de operar** desde el celular para el empleado de la farmacia.