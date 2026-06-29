### Estructura de la Presentación (Slide by Slide)

**Slide 1: Presentación del equipo**

- **Nombre del Equipo:** Grupo 105.
- **Integrantes y Roles:**
    - **Jorge Osvaldo Tripodi:** Project Manager (PM) y Especialista en Control de Calidad (ECC).
    - **Jhon Daniel Olmedo Paco:** Arquitecto de Software (ARQS) y Analista de Sistemas (AS).
    - **Ariel Emilio Benito:** Líder Técnico (LT).
    - **Joaquin Andres Soaje:** Desarrollador Backend (DB) y Analista de Sistemas (AS).
    - **Elias Nicolas Aires:** Desarrollador Frontend (DF) y Tester (QA).

**Slide 2: Presentación del proyecto y la problemática que resuelve**

- **El Proyecto:** **FarmaTracker** es una plataforma tecnológica bajo un modelo **SaaS (Software as a Service)** diseñada para la gestión de inventario crítico en entornos farmacéuticos.
- **¿Cómo surgió la idea y qué problemática resuelve?:** Surge de la necesidad de evitar la pérdida física y económica por medicamentos vencidos en las góndolas de las farmacias. Actualmente, las auditorías son manuales, complejas y propensas a errores. Además, existen plazos estrictos de las droguerías para aceptar devoluciones y normativas rigurosas (ANMAT) para descartar residuos patogénicos, lo que representa un alto costo logístico si no se gestiona a tiempo.

**Slide 3: Objetivos y Alcances**

- **Objetivos Principales:**
    - **Maximizar el recupero de capital** mediante la detección proactiva de vencimientos.
    - **Automatizar las acciones correctivas** (Devolución, Traslado inter-sucursal, Promoción o Destrucción).
    - **Garantizar el cumplimiento normativo** de la ANMAT asegurando la trazabilidad logística.
- **Alcances del Sistema:**
    - Módulo de alertas por ventanas temporales críticas.
    - Motor de Reglas lógicas para sugerir acciones automáticamente.
    - Gestión de "Hojas de Ruta" para optimizar el recorrido físico del empleado en la farmacia.
    - Dashboard Gerencial (KPIs) para visualizar mermas y ahorros operativos.

**Slide 4: Metodologías de Gestión, Análisis y Diseño**

- **Gestión del Proyecto:** Se utilizó el **marco de trabajo del PMI** (Project Management Institute), basando la estructura en el modelo OEP (Objetivos – Entregables – Plan). Se elaboró una Estructura de Desglose del Trabajo (WBS), un cronograma de Gantt y Matrices de Riesgos y Comunicaciones.
- **Análisis y Diseño:** Se aplicaron los lineamientos del **Proceso Unificado** empleando **UML (Unified Modeling Language)** para el modelado estático y dinámico (diagramas de clases, secuencias, estado y entidad-relación). La especificación de requerimientos se documentó rigurosamente utilizando el **estándar IEEE 830-1993**.

**Slide 5: Arquitectura de Software Aplicada**

- **Arquitectura:** Se definió una arquitectura web clásica de **tres capas**.
- **Stack Tecnológico:**
    - **Frontend:** React (para la interfaz de usuario).
    - **Backend:** Spring Boot (para el motor de reglas y la lógica de negocio).
    - **Base de Datos:** PostgreSQL (base de datos relacional).
- **Infraestructura (Despliegue):** El sistema corre sobre un servidor on-premise (o infraestructura cloud AWS/Azure) y utiliza **Cloudflare** como proxy inverso para gestionar el DNS, simplificar la implementación y ofrecer protección contra bots y ciberataques.

**Slide 6: Presupuesto y Viabilidad _(Slide extra recomendado para Inversionistas)_**

- **Presupuesto Estimado:** El proyecto tiene un costo total calculado en **$14.410,00 USD**.
- **Distribución:** Esto incluye $12.600 USD destinados a Recursos Humanos (840 horas de dirección y desarrollo), $400 USD de infraestructura Cloud, $100 USD en licencias, y una reserva de contingencia del 10% ($1.310 USD) para mitigar riesgos legales o demoras de integración.

**Slide 7: Trabajos Futuros y Siguientes Pasos**

- **Implementación Real:** Transición de la presente carpeta técnica y de diseño a una fase de construcción e implementación en un entorno real de farmacia.
- **Capacitación:** Llevar a cabo una capacitación intensiva al personal técnico y directores de las farmacias para asegurar que se aproveche al máximo el "Motor de Reglas" y la automatización.
- **Evolución Técnica:** Mejorar la integración visual entre el Dashboard de KPIs y la generación automática de manifiestos legales en PDF. Ampliar la cobertura de pruebas automatizadas en el backend para eliminar cualquier margen de error crítico y reducir la deuda técnica mediante refactorización continua.