SLIDE 1

**FarmaTracker**, debemos ver su arquitectura no solo como una estructura de software, sino como el **núcleo estratégico** que permite transformar una farmacia pasiva en una operación proactiva de recuperación de activos.

El corazón de esta solución late a través de cuatro componentes fundamentales que articulan el éxito del negocio:

1. **La Sucursal como Ámbito Operativo:** La **Sucursal** es mucho más que una ubicación física; es el ecosistema donde convergen los medicamentos y los usuarios. Es el ámbito donde se generan las incidencias y se ejecutan las hojas de ruta, permitiendo que el sistema centralice el control de múltiples puntos de venta en un modelo SaaS escalable.
2. **El Lote como Unidad de Inteligencia:** En el mundo farmacéutico, el producto es el medicamento, pero la unidad de gestión es el **Lote**. Nuestra arquitectura trata al lote como la unidad mínima de seguimiento, ya que todas sus unidades comparten una fecha de vencimiento crítica. Es el objeto físico que se audita en las góndolas y el que activa toda la cadena de valor cuando ingresa en una ventana temporal de riesgo.
3. **El Motor de Reglas como el Cerebro del Sistema:** Aquí reside la verdadera potencia de FarmaTracker. El **Motor de Reglas** automatiza la detección de lotes críticos, eliminando la discrecionalidad y el error humano. Analiza variables complejas como el tipo de fármaco, las políticas de los laboratorios y la rotación de stock para procesar proyecciones y emitir alertas tempranas. Sin este motor, la auditoría seguiría siendo una tarea manual e ineficiente de "caja por caja".
4. **La Acción Correctiva como Valor de Negocio:** El objetivo final del sistema no es solo informar, sino resolver. Cada alerta disparada por el motor de reglas culmina en una **Acción Correctiva o de Resolución** sugerida (ya sea devolución, traslado, promoción o destrucción). Esta es la pieza arquitectónica que materializa el objetivo de **maximizar el recupero de capital**, permitiendo que el Director Técnico tome decisiones profesionales que eviten que un medicamento se convierta en una pérdida total o en un residuo patogénico.


---

SLIDE 2  FEFO

para la  operación rentable y una pérdida de activos reside en la capacidad de anticipación. El **algoritmo FEFO (First Expired, First Out)** no es solo un método de inventario, es la **piedra angular de FarmaTracker** que asegura que los medicamentos con vencimientos más cercanos sean detectados y gestionados con prioridad absoluta.

A través del **Motor de Reglas**, el cerebro del sistema, transformamos un simple dato de calendario en una **Acción de Resolución estratégica**, eliminando la discrecionalidad y el error humano.

Desde que un **Lote** ingresa en una ventana de riesgo, el sistema procesa la información para permitir los siguientes caminos de resolución:

1. **Devolución a la Droguería:** Es la prioridad para maximizar el **recupero de capital**. El motor analiza las políticas del laboratorio y, si estamos dentro de la ventana permitida, genera automáticamente el **manifiesto de devolución** para retornar el producto antes de que pierda su valor comercial.
2. **Traslado Inter-sucursal:** Si el motor detecta que el producto tiene baja rotación en la ubicación actual, sugiere el movimiento hacia otra sucursal con mayor demanda, evitando que el activo caduque en la estantería.
3. **Promoción Comercial:** Para medicamentos de venta libre, el sistema activa una alerta para que el personal inicie **acciones comerciales** (descuentos o promociones), acelerando la salida del stock crítico mediante la venta directa.
4. **Destrucción de Residuos Patogénicos:** Cuando el lote ya ha vencido o está fuera de toda ventana de devolución, el sistema asiste en la **gestión de destrucción**. Esto asegura el **cumplimiento normativo ante ANMAT**, generando las actas legales necesarias para tratar el medicamento como residuo patogénico de forma segura.
5. **Ignorar:** El **Director Técnico**, bajo su criterio profesional, puede decidir que una alerta no requiere acción física inmediata, cerrando la incidencia en el sistema si el riesgo se considera aceptable o inexistente en ese contexto específico.
6. **Postergar:** Si la decisión requiere una revisión posterior o la espera de una nueva normativa, el sistema permite **reprogramar la alerta**. Esto quita la incidencia de la lista de pendientes actual, pero garantiza que **no se pierda de vista**, reapareciendo automáticamente en una fecha futura definida.

El **FEFO** alimentando al **Motor de Reglas** permite que FarmaTracker deje de ser una herramienta de monitoreo para convertirse en una plataforma de **logística inversa proactiva**. De esta forma, cumplimos con el modelo **OEP** del proyecto: transformar la detección del lote en un **entregable operativo** que garantiza la rentabilidad y la integridad documental de la farmacia.


----
 SLIDE 3 REQS NO FUNCIONALES
 
NO es  solo un software de inventario; estamos ante una herramienta que gestiona activos de salud donde la trazabilidad y la eficiencia operativa son obligatorias para cumplir con normativas como las de ANMAT.

A continuación, detallo cómo los requerimientos de seguridad y usabilidad articulan los roles de nuestros usuarios clave:

### 1. Seguridad: La Garantía de Integridad

La seguridad en FarmaTracker no es opcional, es el cimiento que permite la **integridad documental**.

- **Acceso Segmentado:** Mediante los requerimientos **RF01** y **RF02**, implementamos una autenticación obligatoria y una clasificación estricta por roles (Director Técnico y Empleado).
- **Menús Personalizados:** El sistema aplica el **RF03**, ofreciendo menús diferenciados para que cada usuario interactúe solo con lo que le compete, mitigando riesgos de manipulación indebida de datos estratégicos.
- **Trazabilidad Total:** Todo cambio de estado en una incidencia genera un **log de auditoría** que registra quién, cuándo y qué acción se tomó, asegurando que cada movimiento sea auditable ante inspecciones sanitarias.

### 2. Usabilidad: Eficiencia en el Punto de Acción

La usabilidad es lo que transforma la aceptación del sistema en el salón de ventas.

- **Regla de los Tres Clics:** Hemos definido un requerimiento no funcional (**RNF**) de usabilidad crítico: la interfaz de la hoja de ruta no debe requerir más de tres interacciones para confirmar el estado de un lote.
- **Diseño Mobile-First:** Entendemos que el trabajo ocurre en la góndola, por lo que el sistema está optimizado para pantallas de 5 pulgadas, permitiendo una lectura clara durante el recorrido físico.

### 3. El Empleado como Ejecutor Operativo

El rol del **Empleado** es el par de manos que materializa la estrategia en el salón.

- Es el responsable de la **recepción física** y el control de góndolas.
- Su herramienta principal es la **Hoja de Ruta de Auditoría (CU-03)**, una guía operativa que le indica la ubicación exacta (pasillo, nivel) y el lote a revisar, minimizando el error humano y optimizando su tiempo de trabajo.
- Como ejecutor, su interacción con el sistema es ágil: recibe notificaciones de acciones requeridas y marca las tareas como completadas tras el chequeo físico.

### 4. El Director Técnico como Estratega de Negocio

El **Director Técnico** (o Dueño) es el cerebro estratégico del sistema, con privilegios administrativos totales.

- **Gestión del Motor de Reglas:** Es quien parametriza las ventanas de tiempo y criterios de criticidad (**RF04**), definiendo la inteligencia que detectará los lotes críticos.
- **Toma de Decisiones:** Ante cada incidencia, el DT actúa como el validador final. Utiliza el sistema para decidir si un lote se debe **devolver, trasladar, promocionar o destruir**, basándose en la sugerencia del motor de reglas y su criterio profesional.
- **Visión Analítica:** A través del **Dashboard de KPIs (RF07)**, monitorea métricas como el ahorro operativo estimado y el ranking de criticidad por laboratorio, transformando los datos en decisiones que maximizan el recupero de capital para la farmacia.

En conclusión, mientras el **Empleado** garantiza que la realidad física de la góndola coincida con el sistema, el **Director Técnico** utiliza FarmaTracker como un tablero de control para asegurar la rentabilidad y el cumplimiento normativo. Esta sinergia, protegida por una capa de seguridad robusta, es lo que hace de nuestra arquitectura el núcleo del éxito farmacéutico.




----

SLIDE PRE


### Arquitectura e Infraestructura: El Motor de Misión Crítica de FarmaTracker

"Colegas, para que FarmaTracker cumpla su promesa de disponibilidad los 365 días del año y garantice la integridad de activos de salud, hemos diseñado una **arquitectura web de tres capas** que prioriza la estabilidad sobre la novedad.

#### 1. El Stack Tecnológico: Robustez y Rapidez

Nuestra arquitectura se apoya en tres pilares maduros:

- **Frontend en React:** Elegido por su velocidad de desarrollo y su vasto ecosistema de librerías, permitiéndonos construir interfaces complejas como el Dashboard de KPIs y las Hojas de Ruta con una experiencia de usuario ágil.
- **Backend en Spring Boot:** Como ingenieros, sabemos que la lógica de un **Motor de Reglas** requiere un entorno robusto. Spring Boot nos da la estabilidad y el manejo de concurrencia necesario para procesar incidencias en menos de 3 segundos por lote.
- **Persistencia en PostgreSQL:** Optamos por una base de datos relacional sólida para asegurar la trazabilidad absoluta exigida por ANMAT, donde cada relación entre Lote, Medicamento y Sucursal sea atómica e inquebrantable.

#### 2. La Decisión de Infraestructura: ¿Por qué On-Premise?

A diferencia de la tendencia masiva hacia el Cloud público (AWS/Azure), hemos tomado la decisión estratégica de desplegar la solución en un **único servidor físico On-Premise**. Esta elección no es aleatoria, se basa en un análisis de **Factibilidad Técnica y Económica**:

- **Modelo de Negocio B2B:** FarmaTracker no es un sistema de consumo masivo para millones de usuarios; es una plataforma SaaS para farmacias donde la escala de usuarios concurrentes es finita y predecible. La elasticidad infinita de la nube aquí sería una sobreingeniería costosa.
- **Eficiencia de Costos a Largo Plazo:** Al evitar los costos recurrentes y variables de las instancias en la nube, logramos una solución mucho más económica y predecible para el cliente final, eliminando la complejidad operativa de gestionar servicios cloud complejos para una necesidad que un servidor bien dimensionado puede resolver.
- **Trade-offs controlados:** Reconocemos que sacrificamos cierta elasticidad y tolerancia a fallos automatizada, pero lo compensamos con una capa de seguridad externa.

#### 3. El Híbrido Estratégico: Cloudflare como Proxy

Para mitigar los riesgos de una infraestructura local, integramos **Cloudflare como proxy inverso**. Esto nos permite:

- Gestionar el acceso externo de forma segura sin exponer directamente el servidor.
- Obtener protección contra ataques DDoS y bots a costo cero (plan gratuito).
- Centralizar el manejo de DNS sin depender de hardware adicional.

#### 4. Por qué no otras alternativas

Consideramos arquitecturas de microservicios o despliegues serverless, pero fueron descartadas por la **deuda técnica** y la complejidad de red que introducirían en un proyecto de este ciclo de vida. Un **monolito modular** en un servidor On-Premise nos da el control total sobre la latencia del Motor de Reglas y simplifica el plan de CI/CD para el equipo de desarrollo.

En resumen, la infraestructura de FarmaTracker no busca ser la más compleja, sino la más **eficiente y confiable**. Es una arquitectura pensada para que el **Director Técnico** tenga la seguridad de que sus datos están a salvo y que el **Empleado** pueda ejecutar su Hoja de Ruta sin demoras, todo bajo un esquema de costos que hace al proyecto viable y escalable para el mercado farmacéutico."