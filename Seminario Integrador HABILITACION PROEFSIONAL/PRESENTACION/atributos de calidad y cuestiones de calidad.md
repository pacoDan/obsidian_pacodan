En el contexto de la documentación técnica y la gestión de proyectos, los **atributos de calidad** se refieren a las características no funcionales del producto (como el software), mientras que las **cuestiones de calidad** abarcan los conceptos, costos y procesos para asegurar que el proyecto cumpla con los requisitos del cliente.

A continuación, te detallo ambos conceptos basándome en las fuentes proporcionadas:

### Atributos de Calidad del Software

Según el documento de Especificación de Requerimientos de Software (ERS), los atributos del software son factores críticos que determinan el nivel de desempeño y diseño del producto. Los principales que deben definirse son:

- **Confiabilidad:** Especifica los factores necesarios para determinar y garantizar el nivel de confiabilidad del software en el momento en que se entrega al cliente.
- **Disponibilidad:** Define los factores necesarios para garantizar que el software esté operativo y disponible, abarcando elementos como los puntos de control, los planes de recuperación ante caídas del sistema y los procedimientos de arranque.
- **Facilidad de Mantenimiento:** Especifica qué tipos de métricas y valores (por ejemplo, el nivel de modularidad, la estructura de las interfaces y la complejidad del código) asegurarán que el sistema sea fácil de mantener y modificar en el futuro.
- **Portabilidad:** Detalla los atributos relacionados con la facilidad para cambiar el software a otro servidor o sistema operativo. Esto incluye medir el porcentaje de componentes o código que son dependientes del servidor, el uso de lenguajes portables probados y el uso de sistemas operativos o compiladores particulares.

### Cuestiones de Calidad en la Gestión de Proyectos

De acuerdo con la Guía del PMBOK®, la calidad se define como el "grado en el que un conjunto de características inherentes satisface los requisitos". Para gestionar un proyecto exitosamente, el equipo de dirección debe tener en cuenta las siguientes cuestiones clave:

- **Calidad vs. Grado:** Es fundamental entender que no son lo mismo. El "grado" es una categoría asignada a productos que tienen el mismo uso funcional pero diferentes características técnicas. Una **baja calidad siempre es un problema** (por ejemplo, un software con muchos defectos o _bugs_), pero un **bajo grado puede no serlo** (por ejemplo, un software de alta calidad pero con funcionalidades muy básicas y limitadas).
- **Precisión vs. Exactitud:** La precisión es la consistencia con la que los valores de mediciones repetidas se agrupan (poca dispersión), mientras que la exactitud es la medida en que el valor medido está cercano al valor verdadero o real. Las mediciones precisas no son necesariamente exactas y viceversa; el equipo debe determinar qué nivel de ambas se requiere.
- **El Costo de la Calidad (COQ):** Refiere a los costos totales en los que se incurre para garantizar la calidad del proyecto. Se dividen en dos grandes grupos:
    - _Costos de cumplimiento (prevención y evaluación):_ Inversiones para prevenir errores, como la planificación de la calidad, capacitación de personal y sistemas de control.
    - _Costos por fallos (no cumplimiento):_ Gastos generados por no cumplir con los requisitos, tales como reprocesar productos defectuosos, desperdicio, reclamos de garantía y la pérdida de reputación.
- **La Prevención sobre la Inspección:** Uno de los principios fundamentales de la gestión moderna es que **la calidad se planifica, se diseña y se incorpora; no se incluye mediante inspección**. El costo de prevenir errores es generalmente mucho menor que el costo de tener que corregirlos una vez que son detectados por una inspección o por el cliente.
- **Satisfacción del cliente y mejora continua:** El proyecto debe producir lo que prometió y satisfacer las necesidades reales (conformidad y adecuación para su uso). Además, se debe aplicar el ciclo iterativo de _Planificar-Hacer-Revisar-Actuar_ para mejorar continuamente los procesos y reducir actividades que no agregan valor.

Para manejar todas estas cuestiones, los proyectos implementan tres procesos formales: la **Planificación de Calidad** (identificar las normas a seguir), el **Aseguramiento de Calidad o QA** (aplicar las actividades planificadas para asegurar que se usen los procesos correctos) y el **Control de Calidad o QC** (supervisar los entregables específicos para determinar si cumplen con los estándares y eliminar causas de rendimiento insatisfactorio).