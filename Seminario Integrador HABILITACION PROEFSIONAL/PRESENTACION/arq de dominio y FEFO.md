Para comprender a fondo la **arquitectura del dominio** de FarmaTracker y cómo se integra con el **algoritmo FEFO**, es fundamental analizar cómo el sistema traduce una necesidad operativa en entidades de software que interactúan entre sí.

### El rol del algoritmo FEFO

**FEFO** significa _First Expired, First Out_ (Primero en Vencer, Primero en Salir). En el contexto del proyecto, esta es la **lógica operativa fundamental** que FarmaTrack busca optimizar. El objetivo de implementar FEFO es asegurar que los medicamentos con las fechas de vencimiento más cercanas sean los primeros en ser detectados y gestionados por la farmacia, evitando que se conviertan en mermas o pérdidas totales.

### Arquitectura del Dominio

El modelo de dominio materializa esta lógica de negocio a través de entidades conceptuales centrales y reglas estrictas de interacción. El flujo lógico de estas relaciones se estructura de la siguiente manera:

**1. Estructura Comercial y Trazabilidad Básica**

- El ecosistema nace en la **`Sucursal`**, que es el ámbito obligatorio donde se configuran y controlan las operaciones.
- La sucursal contiene **`Medicamentos`**, los cuales están asociados directamente a un **`Proveedor (Droguería)`**.
- A su vez, los medicamentos agrupan a la unidad mínima de control del sistema: el **`Lote`**. Cada lote es físico, tiene una ubicación en la góndola y una fecha de caducidad, compartida por todas las unidades que lo componen.

**2. Evaluación Automatizada (Donde opera FEFO)**

- Aquí interviene la entidad clave: el **`Motor de Reglas`**. Este componente aplica la lógica FEFO al interactuar dinámicamente con todo el universo de lotes activos del sistema.
- El motor analiza de forma constante la proximidad de la fecha de caducidad de los lotes frente a las reglas parametrizadas (como los convenios y ventanas temporales de devolución permitidos por las droguerías).

**3. Activación del Riesgo e Intervención Profesional**

- Cuando el Motor de Reglas detecta que un `Lote` ha ingresado en una ventana crítica de tiempo, **genera automáticamente una `Incidencia`** en estado "Pendiente".
- El sistema no toma decisiones definitivas por su cuenta. La incidencia es el elemento intermedio obligatorio que debe ser revisado explícitamente por la entidad **`Usuario`** con el rol de **`Dueño / Director Técnico`**.

**4. Disparo de la Decisión (Acción Correctiva)**

- Tras inspeccionar la incidencia, el Director Técnico dispara una **`Acción Correctiva`**.
- El sistema le ofrece un catálogo unificado de seis posibles resoluciones estratégicas: _Devolución_ (retorno al proveedor), _Traslado_ (redistribución a otra sucursal), _Promoción Comercial_ (descuento en mostrador), _Destrucción_ (manejo como residuo patogénico), _Ignorar_ o _Postergar_.

**5. Consolidación Logística y Legal**

- Una vez que se aplica la `Acción Correctiva`, esta modifica el estado del `Lote` y se registra formalmente en una **`Hoja de Ruta`** diaria. La hoja de ruta agrupa los lotes y sirve para guiar el recorrido físico del empleado (rol operativo) en la góndola de la sucursal.
- Si la acción correctiva implica una salida externa de la farmacia (como _Devolución_ a la droguería o _Destrucción_), el sistema emite automáticamente un **`Manifiesto`**. Este es un documento legal en formato PDF que asegura la trazabilidad exigida por las normativas de control sanitario.

En perspectiva técnica, este modelo de dominio demuestra un alto nivel de madurez, ya que separa claramente los datos estáticos (Medicamentos, Sucursales) del procesamiento de reglas de negocio automatizado (Motor de Reglas + FEFO), reservando el control estratégico final al actor humano competente (Director Técnico) mediante las Incidencias y Acciones Correctivas.