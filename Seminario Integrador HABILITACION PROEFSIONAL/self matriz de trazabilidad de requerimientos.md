A continuación se presenta la **Matriz de Trazabilidad de Requerimientos final** para el proyecto **FarmaTracker**. Esta versión vincula exclusivamente los requerimientos funcionales con los **cuatro Objetivos de Negocio (OEP)** definidos formalmente en el Acta de Constitución, cumpliendo con el procedimiento de la **Fase 3: Diseño** del plan de ejecución.

### Matriz de Trazabilidad de Requerimientos - FarmaTracker

|ID Req.|Descripción del Requerimiento|Objetivo de Negocio (OEP)|Caso de Uso Asociado|Artefactos de Diseño / Implementación (Fase 3)|
|:--|:--|:--|:--|:--|
|**RF04**|Gestión del motor de reglas para detección de vencimientos.|**2. Estandarizar la toma de decisiones** (Eliminar error humano mediante reglas uniformes).|**CU-02** Sugerir Acción Correctiva.|Diagrama de Clases, Modelo de Dominio (Entidades Lote/Regla).|
|**RF06**|Generación automática de alertas de vencimiento.|**1. Maximizar el recupero de capital** (Detección temprana para devoluciones oportunas).|**CU-01** Gestionar Alerta de Vencimiento.|Diagrama de Secuencia, Lógica de Alertas en Backend.|
|**RF05**|Visualización de lotes organizados por ubicación física.|**3. Optimizar la eficiencia operativa** (Reducir horas-hombre en búsqueda manual).|**CU-03** Generar Hoja de Ruta de Auditoría.|Documento de Diseño de interfaces final (Vista Mobile).|
|**RF09**|Generación de documentos PDF (Manifiestos y Actas).|**4. Asegurar el cumplimiento normativo** (Garantizar trazabilidad legal ante ANMAT).|**CU-04** Reg. Devolución / **CU-06** Destrucción.|Documento de Arquitectura (Módulo de impresión PDF).|
|**RF07**|Dashboard de KPIs y métricas de impacto económico.|**1. Maximizar el recupero de capital** (Monitoreo de ahorros y mermas evitadas).|**CU-05** Consultar Dashboard de KPIs.|Diagrama de Componentes, Diccionario de Datos (Métricas).|
|**RF11**|Consulta y gestión de lotes mediante vista unificada.|**3. Optimizar la eficiencia operativa** (Simplificar la tarea técnica diaria).|**CU-01** Gestionar Alerta de Vencimiento.|DER (Diagrama Entidad Relación), Frontend (React/Wireframes).|
|**RNF04**|Seguridad de acceso por roles (Director Técnico vs. Empleado).|**4. Asegurar el cumplimiento normativo** (Integridad documental y auditoría de accesos).|Todos los Casos de Uso.|Documento de Arquitectura de Software (Capa de Seguridad).|

---

### Procedimiento de Validación de la Matriz (Fase 3)

Para completar el informe de esta matriz según los estándares de tus fuentes:

1. **Verificación de Cobertura:** Se debe confirmar que cada objetivo estratégico (OEP) del Acta de FarmaTracker cuenta con al menos un requerimiento funcional y un artefacto técnico que lo respalde.
2. **Trazabilidad Técnica:** Cada requerimiento (ej. RF04) debe tener su correspondiente representación en el **Diagrama de Clases** (estructura) y en los **Diagramas de Secuencia** (comportamiento) para asegurar que el diseño cubre la funcionalidad.
3. **Aprobación de Responsabilidades:** Según la Matriz de Asignación de Responsabilidades (RAM), el **Analista de Sistemas** elabora (E) los diagramas de secuencia vinculados, mientras que el **Sponsor** firma (F) la validación final del cumplimiento de los objetivos.
4. **Uso de la Matriz:** Este documento es la salida principal de la Fase 3 y sirve como base para iniciar la etapa de **Pruebas Funcionales**, asegurando que se testee cada objetivo de negocio planteado.