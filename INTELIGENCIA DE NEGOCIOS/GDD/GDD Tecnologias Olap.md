**Inteligencia de Negocios (Business Intelligence)**

- **Concepto Base:** Conjunto de metodologías, herramientas y estructuras para reunir, depurar y transformar datos en información integrada, y esta en conocimiento, con el fin de optimizar la toma de decisiones.
    
    - **Rama 1: Jerarquía y Transformación de la Información - _[Base para todo Científico e Ingeniero de Datos]_**
        
        - **Datos:** Unidad semántica mínima y discreta sin valor por sí sola (ej. un número, un nombre) que puede provenir de orígenes internos/externos y ser cuantitativa o cualitativa.
        - **Información:** Conjunto de datos procesados y asociados a un significado.
            - _Operaciones de Transformación (Data Processing):_
                - _Contextualizar:_ Entender el propósito de generación.
                - _Categorizar:_ Definir unidades de medida.
                - _Calcular:_ Aplicar procesamiento matemático o estadístico.
                - _Corregir:_ Eliminar errores o inconsistencias (Data Cleansing).
                - _Condensar:_ Resumir y agregar datos.
        - **Conocimiento:** Fusión de información, experiencia y valores que permite la toma de decisiones mediante la predicción de consecuencias, comparación, búsqueda de conexiones y conversación.
    - **Rama 2: Paradigmas de Bases de Datos - _[Core de Arquitectura para Ingenieros de Datos]_**
        
        - **OLTP (On Line Transaction Processing / Modelo Transaccional):**
            - _Enfoque:_ Ejecución de transacciones individuales, representa el 99% de los sistemas (sistemas operativos).
            - _Características:_ Almacenamiento **normalizado**, registro de datos a nivel de detalle, alta volatilidad y optimización para la creación/actualización continua por múltiples usuarios.
        - **OLAP (On Line Analytical Processing / Modelo Relacional Multidimensional):**
            - _Enfoque:_ Sistemas para el análisis y la toma de decisiones gerencial (1% de los sistemas).
            - _Características:_ Almacenamiento **desnormalizado**, registro de información global por dimensiones, persistencia (no volátil), actualización por bloques desde múltiples orígenes y vistas de alto nivel optimizadas para análisis.
    - **Rama 3: Estructura y Pipeline OLAP - _[Core de flujos ETL para Ingenieros de Datos]_**
        
        - **Concepto Base:** Los datos deben ser capturados de fuentes operativas, duplicados y almacenados físicamente separados del sistema de origen.
        - **Justificación de la Separación (Arquitectura):**
            - _Rendimiento:_ Acceso rápido a grandes volúmenes para análisis interactivos sin afectar la operación.
            - _Integración:_ Combinación de múltiples orígenes con distintas codificaciones y periodicidades.
            - _Filtrado y Ajuste:_ Limpieza de datos y modificación de estructuras heterogéneas (ej. contabilidades de otros países, diferencias departamentales, datos externos como demografía).
            - _Consistencia:_ Homogeneización de datos que se actualizan a diferentes ritmos en sus fuentes.
            - _Historia Temporal:_ Integración de datos de años anteriores para incorporar el "tiempo" como dimensión analítica.
            - _Perspectivas Diferenciadas:_ Ajuste del nivel de resumen, elevando los datos operacionales detallados hacia vistas agregadas.
            - _Seguridad:_ Evitar la sobreescritura de los datos en uso transaccional.
    - **Rama 4: Bases de Datos Multidimensionales (BDM) - _[Core de Modelado para Científicos de Datos]_**
        
        - **Concepto Base:** Estructuras guiadas por "dimensiones" (patrones de interés) que definen variables y caminos de consolidación.
        - **Análisis por Intersección:** La información se interpreta cruzando variables en un cubo (ej. vistas regionales, vistas de producto, vistas temporales o ad-hoc).
        - **Manejo de la Densidad (Sparsity):**
            - _Dispersión de Datos:_ Al cruzar múltiples dimensiones, el número de celdas crece exponencialmente. En la práctica, entre el 80% y 95% de las combinaciones están vacías o en cero (ej. no todo producto se vende en toda tienda cada día).
        - **Topologías de Modelado Multidimensional:**
            - _Hipercubo:_ Toda la información se estructura y presenta implícitamente en un único y masivo cubo multidimensional.
            - _Multicubo:_ La información se divide y almacena en varios objetos más pequeños, densos y separados, cada uno con dimensiones particulares.