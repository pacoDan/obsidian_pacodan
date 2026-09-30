### **1) Hub Logístico de Drones para Envíos Express**

Un centro de distribución de última milla entrega paquetes urgentes en zona urbana mediante drones eléctricos. Los pedidos llegan según un proceso con intervalo entre arribos (IA) determinado por una f.d.p. conocida. Al ingresar un pedido, se asigna al primer dron disponible; si todos están volando o recargando, el pedido espera en una cola virtual.

El tiempo de vuelo de entrega y regreso responde a una f.d.p. expresada en minutos. Al regresar al hub, cada dron verifica su nivel de batería: el 40% requiere una recarga rápida antes del próximo vuelo (demora f.d.p. en minutos), mientras que el 60% restante puede salir inmediatamente a otra entrega. Para recargar, el dron requiere conectarse a una base de recarga automatizada; si todas las bases están ocupadas, el dron debe esperar en la plataforma. Si un pedido pasa más de 30 minutos en cola, se cancela automáticamente y se otorga un cupón de compensación de $10 USD.

- **Variables de Control:**
    1. **\(N\)**: Cantidad de drones en la flota operativa.
    2. **\(M\)**: Cantidad de bases de recarga automatizada en el hub.
- **Variables Resultado:**
    - Porcentaje de pedidos cancelados por superar el tiempo límite de espera.
    - Porcentaje de tiempo ocioso promedio de los drones y de las bases de recarga.
    - Costo total operativo diario (fijo por dron + fijo por base + costo por cupones de cancelación).
- **Objetivo de la Simulación:** Determinar la combinación óptima de **\((N, M)\)** que minimice el costo total operativo garantizando que los pedidos cancelados no superen el 2% del total.

La **Tabla de Eventos Independientes** es la herramienta de análisis previo que permite definir la concatenación dinámica y la evolución temporal de un modelo de simulación evento a evento.

### Reglas Metodológicas de la Tabla

- **Estructura**: Posee cuatro columnas: **Evento** (evento actual), **Evento Futuro No Condicionado (EFNC)**, **Evento Futuro Condicionado (EFC)** y **Condición**.
- **Evento Independiente**: Un evento es independiente si genera como EFNC uno igual a sí mismo (mediante un dato o variable aleatoria concatenadora, como el intervalo entre arribos) o ninguno.
- **Evaluación de la Condición**: La condición expresada en la cuarta columna se evalúa siempre sobre el vector de estado **ya actualizado** tras la ocurrencia del evento actual.

### Tabla de Eventos Independientes: Hub Logístico de Drones

Aplicando esta metodología al enunciado del centro de distribución de drones:

|Evento (Actual)|Evento Futuro No Condicionado (EFNC)|Evento Futuro Condicionado (EFC)|Condición (sobre el vector de estado actualizado)|
|:--|:--|:--|:--|
|**Llegada de Pedido (`LLEGP`)**|`LLEGP`|`FINV_k`|\(ND_{libres} > 0\) *(Hay al menos un dron disponible en el hub para despachar el pedido inmediatamente)*|
|||_Ingreso a Cola Virtual / Cancelación_|\(ND_{libres} = 0\) *(No hay drones libres. Si el pedido pasa más de \(TL = 30\text{ min}\) en cola, se procesa su cancelación y cupón)*|
|**Fin de Vuelo de Dron (`FINV_k`)**|Ninguno|`FINV_k`|\(R > 0.40 \land NS_{pedidos} > 0\) *(La batería no requiere recarga y hay pedidos esperando en la cola virtual)*|
|||`FINR_j`|\(R \le 0.40 \land NB_{libres} > 0\) *(Requiere recarga de batería y hay al menos una base de recarga libre)*|
|||_Espera en Plataforma_|\(R \le 0.40 \land NB_{libres} = 0\) *(Requiere recarga pero todas las bases están ocupadas)*|
|**Fin de Recarga en Base (`FINR_j`)**|Ninguno|`FINR_j`|\(NS_{plataforma} > 0\) *(Hay drones en la plataforma esperando conectarse a una base)*|
|||`FINV_k`|\(NS_{pedidos} > 0\) *(El dron que acaba de recargarse toma un pedido de la cola virtual y sale a volar)*|


### Detalles del Funcionamiento

1. **Llegada de Pedido (`LLEGP`)**: La función de densidad de probabilidad del intervalo entre arribos (\(IA\)) actúan como el dato concatenador que genera incondicionalmente la próxima llegada (`LLEGP`). Solo se programa un fin de vuelo (`FINV_k`) si hay un dron libre (\(ND_{libres} > 0\)).
2. **Fin de Vuelo (`FINV_k`)**: No concatena otro fin de vuelo en forma no condicionada. Se evalúa probabilísticamente el nivel de batería (\(R\)): si no necesita recarga (\(60%\)) y hay pedidos en cola, el dron vuelve a salir inmediatamente (`FINV_k`); si necesita recarga (\(40%\)) y hay bases libres, inicia la recarga (`FINR_j`).
3. **Fin de Recarga (`FINR_j`)**: Al liberarse la base \(j\), si hay drones en plataforma, la base inicia la recarga del siguiente dron (`FINR_j`). El dron recién recargado pasa a estar operativo para tomar pedidos en cola.




---

### **2) Auto-scaling e Infraestructura Cloud para Streaming de Eventos**

Una plataforma de streaming transmite eventos deportivos en vivo y debe gestionar la infraestructura de servidores cloud para absorber picos de tráfico. Los usuarios se conectan al servicio con una frecuencia que responde a una f.d.p. equiprobable de arribos que se triplica durante el horario del partido. La duración de la conexión de cada usuario responde a otra f.d.p. en minutos.

Para procesar la demanda, la empresa mantiene un grupo de servidores activos. La capacidad máxima de cada servidor es de 1000 conexiones concurrentes. Si al llegar un usuario la capacidad global de los servidores activos está al máximo, el sistema intenta realizar un escalado automático (auto-scaling) instanciando un grupo de **\(K\)** servidores adicionales. El tiempo de arranque y aprovisionamiento de las nuevas instancias sigue una f.d.p. uniforme de entre 2 y 5 minutos. Durante ese tiempo de espera, si no hay lugar, hasta un 15% de los usuarios acepta esperar; el resto abandona la plataforma.

- **Variables de Control:**
    1. **\(S_{base}\)**: Cantidad fija de servidores encendidos desde el inicio del evento.
    2. **\(K\)**: Cantidad de servidores adicionales que se encienden en cada evento de auto-scaling.
- **Variables Resultado:**
    - Porcentaje de usuarios que abandonan la plataforma por falta de capacidad inmediata.
    - Porcentaje de uso promedio de los servidores encendidos.
    - Costo total de infraestructura (costo fijo por hora por servidor activo + penalización de $5 USD por cada usuario perdido).
- **Objetivo de la Simulación:** Hallar la combinación óptima de **\((S_{base}, K)\)** que minimice los costos de servidor e impactos económicos por usuarios perdidos.

---

### **3) Gestión de Stock y Mantenimiento Preventivo en Planta Embotelladora**

Una planta industrial de bebidas funciona continuamente. El insumo principal (jarabe concentrado) se consume a un ritmo diario aleatorio determinado por una f.d.p. en litros. El jarabe se almacena en un tanque principal y se reabastece mediante pedidos al proveedor de una cantidad **\(Q\)** litros, los cuales demoran un tiempo aleatorio de entrega (f.d.p. en días).

Por otro lado, la línea de embotellado requiere mantenimientos preventivos programados cada **\(H\)** horas de operación continua. Cada mantenimiento programado insume exactamente 4 horas y cuesta $50.000. Si la máquina opera más allá de **\(H\)** horas sin mantenimiento, la probabilidad de falla catastrófica aumenta: existe un 5% diario de probabilidad de rotura imprevista, cuya reparación demorará entre 12 y 24 horas (f.d.p.) con un costo fijo de $250.000. Durante cualquier parada (programada o por falla), la producción se detiene y no se consume jarabe, pero se incurre en un costo de parálisis de $10.000 por hora.

- **Variables de Control:**
    1. **\(Q\)**: Cantidad de litros de jarabe a pedir al proveedor en cada reorden.
    2. **\(H\)**: Intervalo de horas de operación continua programado entre mantenimientos preventivos.
- **Variables Resultado:**
    - Porcentaje de tiempo de parada no planificada por fallas mecánicas.
    - Cantidad de días con faltante de insumo (tanque de jarabe vacío).
    - Costo total operativo (mantenimientos + reparaciones por falla + almacenamiento + costos de faltante y paradas).
- **Objetivo de la Simulación:** Encontrar la combinación óptima **\((Q, H)\)** que minimice el costo total de funcionamiento de la planta.

---

### **4) Guardia Odontológica y Atención con Turnos Programados**

Un centro odontológico opera durante 12 horas diarias combinando atención programada y urgencias. Llegan pacientes con turno previo según un intervalo entre arribos (IA) determinado por f.d.p., y pacientes de guardia/urgencia (representan el 25% del total de llegadas, respondiendo a otra f.d.p. de arribos).

El centro cuenta con **\(N\)** odontólogos y **\(S\)** sillones dentales equipados. Un paciente para ser atendido requiere la disponibilidad simultánea de 1 odontólogo y 1 sillón. La atención de un turno programado demora según una f.d.p. dada, mientras que la atención de una urgencia requiere entre 30 y 60 minutos (f.d.p.). Se otorga prioridad estricta de atención a los pacientes de urgencia sobre los programados. Si un paciente de urgencia no encuentra recurso libre en un lapso de 20 minutos, se retira y se deriva a otro centro asistencial. Si un paciente con turno espera más de 45 minutos, el centro le reintegra el costo de la consulta ($15.000).

- **Variables de Control:**
    1. **\(N\)**: Cantidad de odontólogos contratados para el turno.
    2. **\(S\)**: Cantidad de sillones dentales instalados y funcionales en el consultorio.
- **Variables Resultado:**
    - Promedio de tiempo de espera en cola de pacientes con turno y de urgencias.
    - Porcentaje de pacientes de urgencia derivados por superar la tolerancia de espera.
    - Porcentaje de tiempo ocioso de los odontólogos y de uso de los sillones.
    - Costo total diario (sueldos de odontólogos + amortización/mantenimiento de sillones + reintegros por demoras).
- **Objetivo de la Simulación:** Determinar la mejor combinación de **\((N, S)\)** que reduzca las demoras y minimice los costos totales operativos.

---

### **5) Estación Multimodal de Carga de Vehículos Eléctricos e Hidrógeno**

Una estación de servicio en ruta abastece a dos tipos de vehículos sustentables: Eléctricos (EV) (80% de los arribos) y de Hidrógeno (FCEV) (20% restante), cuyos intervalos entre arribos responden a funciones de densidad de probabilidad independientes.

Para atender a los EV, la estación dispone de **\(C\)** cargadores ultrarrápidos. El tiempo de carga de un EV sigue una f.d.p. en minutos. Si no hay cargadores libres, los EV hacen una única cola; si hay más de 4 vehículos esperando, los nuevos arribos continúan su viaje sin cargar.

Para atender a los FCEV, la estación cuenta con 1 surtidor de hidrógeno alimentado por un tanque de depósito local de capacidad **\(Cap\)** (expresada en kg). Cada FCEV requiere entre 3 y 6 kg de hidrógeno (f.d.p. equiprobable). La carga insume 5 minutos fijos. Cuando el stock del tanque cae por debajo de los 200 kg, se emite un pedido automático de camión cisterna por \(1000\) kg que demora entre 2 y 4 horas en llegar (f.d.p.). Si el tanque de hidrógeno se queda totalmente vacío, los FCEV que llegan se retiran inmediatamente.

- **Variables de Control:**
    1. **\(C\)**: Cantidad de cargadores ultrarrápidos para vehículos eléctricos instalados.
    2. **\(Cap\)**: Capacidad máxima del tanque de almacenamiento de hidrógeno en kg.
- **Variables Resultado:**
    - Porcentaje de vehículos (EV y FCEV) rechazados por falta de espacio o falta de insumo.
    - Tiempo promedio de espera en cola para vehículos eléctricos.
    - Beneficio neto diario de la estación (ingresos por venta de energía e hidrógeno - costos de instalación/mantenimiento de cargadores - costo de almacenamiento de \(H_2\)).
- **Objetivo de la Simulación:** Evaluar las combinaciones de **\((C, Cap)\)** para definir la configuración que maximice el beneficio neto manteniendo la pérdida de clientes por debajo del 5%.

---

💡 **¿Te gustaría que desarrollemos el análisis conceptual completo (clasificación de variables, eventos independientes y tabla de eventos futuros) o el diagrama de flujo de alguno de estos enunciados?**