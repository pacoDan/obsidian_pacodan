## Resumen Estructurado: ATM y MPLS

### **I. ATM (Asynchronous Transfer Mode)**

ATM es un protocolo de red de conmutación de celdas, diseñado para ser una red de transporte unificada para múltiples servicios (voz, datos, video) con garantía de Calidad de Servicio (QoS).

**A. Características Clave:**

- **Orientado al Bit:** Como los protocolos modernos, es transparente y no tiene códigos asociados.
- **Uso de Celdas:** La característica más distintiva. Las unidades de datos son de **longitud fija y muy pequeña (53 bytes)**.
    
    - **5 bytes:** Cabecera.
    - **48 bytes:** Cuerpo (campo de datos).
    - **Ventaja de Celdas Pequeñas:** Permite un retardo fijo y bajo (baja latencia) en el procesamiento, ya que no hay que esperar a recibir mensajes de longitud variable. Facilita la multiplexación y la gestión de QoS.
    
- **Multiplexación de Conexiones:** Permite múltiples circuitos virtuales sobre un mismo acceso (similar a Frame Relay).
- **Soporte de Calidad de Servicio (QoS):** Capacidad de identificar y priorizar tráfico, garantizando valores de retardo, pérdida, etc.
- **Transmisión Asincrónica:** Se refiere al uso del servicio (establecimiento/liberación de conexiones), no a la transmisión de celdas (que es sincrónica).
- **Orientado a la Conexión:** Requiere el establecimiento de una conexión (circuito virtual) antes de la transmisión de datos.
- **Ancho de Banda a Demanda:** Permite solicitar diferentes anchos de banda para diferentes conexiones.

**B. Modelo de Capas ATM:**

ATM se sitúa en la **capa de enlace (Capa 2)** del modelo OSI, pero tiene múltiples subcapas:

1. **Capa Física (Capa 1):**
    
    - Funciones: Delimitación de celdas, monitoreo de errores, inserción de celdas vacías (para mantener el sincronismo en la transmisión continua).
    - Tecnologías: Originalmente para LAN (25/51 Mbps), pero en WAN se usa sobre **SDH (Synchronous Digital Hierarchy)** (STM-1 a 155 Mbps, STM-4 a 622 Mbps, etc.).
    - **Establecimiento de Sincronismo (Diagrama de Estados):**
        
        - **Captura:** Analiza bit a bit, buscando un HEC (Header Error Control) correcto.
        - **Presincronismo:** Una vez encontrado un HEC correcto, verifica varias celdas consecutivas. Si fallan, vuelve a Captura.
        - **Sincronismo:** Una vez que se confirman varias celdas correctas, se asume el sincronismo. Aquí se entra en los modos de corrección/detección de errores. Si hay múltiples errores consecutivos, se pierde el sincronismo y se vuelve a Captura.
        
    
2. **Capa ATM (Capa 2 - Subcapa Inferior):**
    
    - Maneja la cabecera de 5 bytes de la celda.
    - **Formato de Celda (53 bytes):**
        
        - **Cabecera (5 bytes):**
            
            - **GFC (Generic Flow Control):** 4 bits, solo en interfaz UNI (User-Network Interface). Control de flujo entre usuario y red.
            - **VPI (Virtual Path Identifier):** Identificador de camino virtual. Más grande en NNI (Network-Network Interface).
            - **VCI (Virtual Channel Identifier):** Identificador de canal virtual.
            - **CLP (Cell Loss Priority):** 1 bit. Similar al DE (Discard Eligibility) de Frame Relay. Si está en 1, la celda es descartable en caso de congestión.
            - **HEC (Header Error Control):** 8 bits. CRC para control de errores **solo en la cabecera**. Permite detectar y corregir **un solo bit erróneo** (más de uno indica un problema grave de sincronismo). También ayuda en la delimitación de celdas.
            
        - **Cuerpo (48 bytes):** Campo de datos.
        
    - **VPI/VCI:** Par de identificadores jerárquicos. Un VPI puede contener múltiples VCIs. Permite multiplexar conexiones.
        
        - **VP Switch:** Cambia solo el VPI.
        - **VC Switch:** Cambia VPI y VCI.
        
    - **Tipos de Conexiones Virtuales (VCC - Virtual Channel Connection):**
        
        - **Usuario a Usuario:** Conexión de extremo a extremo para datos.
        - **Usuario a Red:** Para señalización (establecimiento dinámico de conexiones) y agregación de tráfico.
        - **Red a Red:** Para intercambio de información administrativa entre nodos ATM.
        
    - **Circuitos Virtuales Permanentes (PVC):** Configuración estática.
    - **Circuitos Virtuales Conmutados (SVC):** Establecimiento dinámico (similar a una llamada telefónica).
    
3. **Capa de Adaptación ATM (AAL - ATM Adaptation Layer) (Capa 2 - Subcapa Superior):**
    
    - **Propósito:** Acomodar PDUs (Protocol Data Units) de longitud variable (ej. paquetes IP) en las celdas ATM de 48 bytes de carga útil.
    - **Funciones:** Manejo de errores de transmisión, segmentación y reensamblado, manejo de celdas perdidas/mal ordenadas, control de flujo.
    - **Subcapas:**
        
        - **Subcapa de Convergencia (CS):** Adapta el formato de los datos al tipo de servicio.
        - **Subcapa de Segmentación y Reensamblado (SAR):** Divide los datos en fragmentos de 48 bytes para las celdas ATM.
        
    - **Clases de Servicio (QoS):** ATM define clases de servicio para diferentes tipos de tráfico:
        
        - **Clase A (CBR - Constant Bit Rate):** Tasa de bit constante, requiere sincronización. Ej: Voz y video sin comprimir (64 kbps).
            
            - **AAL1:** Capa de adaptación para Clase A.
            
        - **Clase B (VBR - Variable Bit Rate):** Tasa de bit variable, requiere sincronización. Ej: Voz y video comprimidos.
            
            - **AAL2:** Capa de adaptación para Clase B.
            
        - **Clase C (VBR-NRT - Variable Bit Rate Non-Real Time):** Datos orientados a la conexión, tasa de bit variable, no requiere sincronización.
        - **Clase D (UBR - Unspecified Bit Rate):** Datos no orientados a la conexión, tasa de bit variable, no requiere sincronización. Best effort.
            
            - **AAL3/4 y AAL5:** Capas de adaptación para Clases C y D. AAL5 es la más usada para datos IP.
            
        
    

**C. Calidad de Servicio (QoS) en ATM:**

- **Proceso de Establecimiento de Conexión:**
    
    1. **Solicitud del Usuario:** Pide una VCC, especificando:
        
        - **Categoría de Servicio:** CBR, VBR (real-time/non-real-time), ABR (Available Bit Rate - tasa mínima garantizada), UBR (Unspecified Bit Rate - best effort, el más económico).
        - **Atributos de Tráfico:**
            
            - **PCR (Peak Cell Rate):** Tasa máxima de celdas.
            - **SCR (Sustained Cell Rate):** Tasa sostenida de celdas.
            - **MBS (Maximum Burst Size):** Tamaño máximo de ráfaga (para VBR).
            - **MCR (Minimum Cell Rate):** Tasa mínima (para ABR).
            
        - **Parámetros de Calidad de Servicio (QoS):**
            
            - **CDV (Cell Delay Variation):** Variación del retardo entre celdas (jitter). Se mitiga con buffers de de-jitter, introduciendo retardo.
            - **MCTD (Maximum Cell Transfer Delay):** Retardo máximo de transferencia de celda (extremo a extremo).
            - **CLR (Cell Loss Ratio):** Tasa máxima de pérdida de celdas soportada. (Ej: Voz sin comprimir tolera cierta pérdida, datos no).
            
        
    2. **Control de Admisión de Conexión (CAC):** La red evalúa si tiene los recursos para cumplir con la QoS solicitada. Si no, puede rechazar o negociar.
    3. **Control de Parámetros de Usuario (UPC):** Una vez establecida la conexión, la red monitorea que el usuario cumpla con los parámetros contratados (PCR, SCR). Si el usuario excede, las celdas se marcan con CLP=1 (descartables en congestión), pero no se descartan inmediatamente.
    

**D. Estado Actual de ATM:**

- Aunque fue diseñado para ser de extremo a extremo, su complejidad y costo lo hicieron poco práctico en LAN (donde Ethernet dominó).
- Actualmente, ATM está **restringido al Core de la red de los proveedores** como tecnología de transporte, no siendo expuesto directamente a los usuarios finales.

---

### **II. MPLS (Multi-Protocol Label Switching)**

MPLS es una tecnología de red que mejora la eficiencia del reenvío de paquetes al combinar la conmutación de Capa 2 con el enrutamiento de Capa 3. No es un protocolo de Capa 2, sino de **Capa 2.5**.

**A. Problema en Redes IP Tradicionales:**

- Cada router lee la dirección IP de destino en la cabecera del datagrama.
- Realiza una búsqueda de "mejor coincidencia" (longest match) en su tabla de enrutamiento.
- Esta búsqueda se realiza **independientemente en cada salto**, lo que puede ser ineficiente para grandes volúmenes de tráfico.

**B. Solución MPLS:**

- **Conmutación Basada en Etiquetas:** En lugar de leer la cabecera IP, los routers MPLS leen una **etiqueta** de longitud fija.
- **Coincidencia Exacta:** La búsqueda de la etiqueta es una coincidencia exacta, mucho más rápida y eficiente que la búsqueda de "mejor coincidencia" en una tabla de enrutamiento IP.
- **Independencia del Protocolo de Capa 3:** Al conmutar etiquetas, MPLS puede transportar cualquier protocolo de Capa 3 (IPv4, IPv6, etc.) de manera uniforme.
- **Aplicaciones:** Enrutamiento IP unicast/multicast, VPNs (Virtual Private Networks), Calidad de Servicio (QoS), Ingeniería de Tráfico.

**C. Arquitectura MPLS:**

1. **Planos del Router:**
    
    - **Plano de Control:** Donde corren los protocolos de enrutamiento (OSPF, BGP) y los protocolos de distribución de etiquetas (LDP - Label Distribution Protocol). Aquí se construye la información de enrutamiento y etiquetado.
    - **Plano de Datos (Forwarding Plane):** Donde se realiza la conmutación de paquetes. Contiene la tabla de enrutamiento IP y la **tabla de conmutación de etiquetas (LFIB - Label Forwarding Information Base)**.
    
2. **Componentes de la Red MPLS:**
    
    - **Dominio MPLS:** La parte de la red donde se utiliza MPLS.
    - **LSR (Label Switching Router):** Cualquier router que soporta MPLS y puede conmutar etiquetas.
    - **Edge LSR (E-LSR) / Provider Edge (PE) Router:** Routers en el borde del dominio MPLS.
        
        - **Funciones:**
            
            - **Ingreso:** Reciben datagramas IP, les **añaden (push)** una etiqueta MPLS. Clasifican el tráfico en FECs.
            - **Egreso:** Reciben datagramas etiquetados, les **quitan (pop)** la etiqueta MPLS, y reenvían el datagrama IP.
            
        
    - **Core LSR / Provider (P) Router:** Routers dentro del dominio MPLS.
        
        - **Funciones:** Solo conmutan etiquetas. Reciben un paquete con una etiqueta, la **cambian (swap)** por otra, y lo reenvían. Son los más eficientes.
        
    

**D. Funcionamiento de MPLS (Ejemplo de Flujo):**

1. **Ingreso al Dominio MPLS:**
    
    - Un datagrama IP llega a un E-LSR (PE router).
    - El E-LSR clasifica el datagrama en una **FEC (Forwarding Equivalence Class)**. Una FEC es un grupo de paquetes que se tratan de la misma manera sobre un mismo camino (ej. todo el tráfico a la red X, o todo el tráfico urgente a la red X).
    - El E-LSR consulta su tabla de conmutación de etiquetas y **añade (push)** una etiqueta MPLS al datagrama. La etiqueta se inserta entre la cabecera de Capa 2 y la cabecera IP.
    - El paquete etiquetado se envía al siguiente LSR.
    
2. **Dentro del Dominio MPLS:**
    
    - Los LSRs intermedios reciben el paquete etiquetado.
    - Leen la etiqueta, consultan su tabla de conmutación de etiquetas.
    - **Cambian (swap)** la etiqueta por una nueva etiqueta y reenvían el paquete. Esta operación es muy rápida.
    - La información sobre qué etiqueta usar para cada destino se aprende a través de protocolos como LDP.
    
3. **Salida del Dominio MPLS:**
    
    - El último LSR antes de salir del dominio (otro E-LSR o PE router) recibe el paquete etiquetado.
    - Consulta su tabla de conmutación de etiquetas y **quita (pop)** la etiqueta MPLS.
    - El datagrama IP original se reenvía a la red de destino.
    

**E. La Etiqueta MPLS:**

- **Formato:** 4 bytes (32 bits).
    
    - **Label (20 bits):** El valor de la etiqueta.
    - **Exp (3 bits):** Uso experimental (para QoS).
    - **S (Stack Bit - 1 bit):** Indica si es la última etiqueta en la pila (MPLS soporta apilamiento de etiquetas).
    - **TTL (Time To Live - 8 bits):** Similar al TTL de IP. Se decrementa en cada salto.
    

**F. TTL en MPLS:**

- **Copia de TTL:** Cuando un paquete IP ingresa al dominio MPLS, el TTL de la cabecera IP se copia al TTL de la etiqueta MPLS.
- **Decremento en LSRs:** Los LSRs decrementan el TTL de la etiqueta.
- **Copia de Salida:** Cuando el paquete sale del dominio MPLS, el TTL de la etiqueta se copia de nuevo a la cabecera IP.
- **Beneficio:** Permite que el TTL de IP siga funcionando correctamente a través del dominio MPLS, reflejando el número real de saltos.
- **Sin Copia de TTL (Opcional):** Si no se copia el TTL, el dominio MPLS se percibe como un único salto para el TTL de IP, ya que el TTL original de IP no se decrementa dentro del dominio.

**G. MPLS como Capa 2.5:**

- Se ubica entre la Capa 2 (enlace) y la Capa 3 (red).
- No realiza funciones de Capa 2 (delimitación, control de errores).
- No realiza todas las funciones de Capa 3 (enrutamiento completo, fragmentación).
- Su función principal es la conmutación eficiente basada en etiquetas.

**H. Beneficios Adicionales de MPLS:**

- **VPNs (Virtual Private Networks):** Permite a los proveedores crear redes privadas virtuales sobre una infraestructura compartida, dando a cada cliente la percepción de tener su propia red dedicada.
- **Ingeniería de Tráfico:** Permite dirigir el tráfico por rutas específicas, optimizando el uso de la red.

---

## Posibles Preguntas y Respuestas para Exámenes

### **Preguntas sobre ATM:**

1. **Pregunta:** ¿Cuál es la característica más distintiva de ATM y por qué se eligió ese tamaño de unidad de datos?
    
    - **Respuesta:** El uso de **celdas de tamaño fijo y pequeño (53 bytes)**. Se eligió para minimizar el retardo de procesamiento y la latencia, facilitando la multiplexación de diferentes tipos de tráfico (voz, datos, video) y la implementación de QoS.
    
2. **Pregunta:** Explique la función del campo HEC en la cabecera de una celda ATM. ¿Qué sucede si se detectan múltiples errores de bit en la cabecera?
    
    - **Respuesta:** El HEC (Header Error Control) es un CRC de 8 bits que controla errores **solo en la cabecera** de 5 bytes. Permite detectar y corregir **un solo bit erróneo**. Si se detectan múltiples errores de bit, la celda se descarta, ya que se asume una pérdida de sincronismo o un problema grave en la transmisión. También ayuda en la delimitación de celdas.
    
3. **Pregunta:** Describa la diferencia entre VPI y VCI en ATM y su propósito.
    
    - **Respuesta:** VPI (Virtual Path Identifier) y VCI (Virtual Channel Identifier) son un par de identificadores jerárquicos. Un VPI puede contener múltiples VCIs. Permiten multiplexar múltiples conexiones virtuales sobre un mismo acceso físico. El VPI agrupa canales, facilitando la gestión y el re-enrutamiento de bloques de conexiones.
    
4. **Pregunta:** ¿Qué es la Capa de Adaptación ATM (AAL) y por qué es necesaria? Mencione al menos dos clases de servicio y su AAL correspondiente.
    
    - **Respuesta:** La AAL es una subcapa de la Capa de Enlace de ATM que adapta los datos de longitud variable de las capas superiores a las celdas ATM de 48 bytes de carga útil. Es necesaria porque las aplicaciones no generan datos en bloques de 48 bytes.
        
        - **Clase A (CBR):** Voz/video sin comprimir, requiere sincronización. Usa **AAL1**.
        - **Clase B (VBR):** Voz/video comprimidos, requiere sincronización. Usa **AAL2**.
        - **Clase C/D (Datos):** Datos, no requieren sincronización. Usan **AAL3/4 o AAL5** (AAL5 es la más común para IP).
        
    
5. **Pregunta:** Explique el concepto de "jitter" (CDV) en ATM y cómo se maneja.
    
    - **Respuesta:** Jitter (Cell Delay Variation) es la variación en el retardo de llegada de las celdas. Se produce en redes de paquetes debido a la congestión variable. Se maneja en el receptor utilizando **buffers de de-jitter**, que almacenan las celdas para suavizar las variaciones y entregarlas a una tasa constante, aunque esto introduce un retardo adicional.
    
6. **Pregunta:** ¿Cuál es el rol del Control de Admisión de Conexión (CAC) y del Control de Parámetros de Usuario (UPC) en ATM?
    
    - **Respuesta:**
        
        - **CAC:** Es la función de la red que evalúa si tiene los recursos disponibles para aceptar una nueva conexión virtual con los parámetros de QoS solicitados por el usuario.
        - **UPC:** Es la función que monitorea el tráfico del usuario una vez establecida la conexión para asegurar que cumple con los parámetros contratados (ej. PCR, SCR). Si el usuario excede, las celdas se marcan como descartables (CLP=1).
        
    

### **Preguntas sobre MPLS:**

1. **Pregunta:** ¿Por qué se considera a MPLS un protocolo de "Capa 2.5"?
    
    - **Respuesta:** Se considera Capa 2.5 porque opera entre la Capa 2 (enlace) y la Capa 3 (red). No realiza funciones completas de Capa 2 (como delimitación o control de errores) ni de Capa 3 (como enrutamiento completo o fragmentación), sino que se enfoca en la conmutación eficiente basada en etiquetas.
    
2. **Pregunta:** Describa el problema que MPLS busca resolver en las redes IP tradicionales y cómo lo logra.
    
    - **Respuesta:** MPLS busca resolver la ineficiencia de la búsqueda de "mejor coincidencia" (longest match) en las tablas de enrutamiento IP en cada salto. Lo logra reemplazando esta búsqueda por una **conmutación de etiquetas** basada en una coincidencia exacta, que es mucho más rápida y eficiente.
    
3. **Pregunta:** Explique la diferencia de funciones entre un Edge LSR (PE router) y un Core LSR (P router) en un dominio MPLS.
    
    - **Respuesta:**
        
        - **Edge LSR (PE router):** Se encuentra en el borde del dominio MPLS. Es responsable de **añadir (push)** etiquetas a los paquetes IP que ingresan y **quitar (pop)** etiquetas a los paquetes que salen. Clasifica el tráfico en FECs.
        - **Core LSR (P router):** Se encuentra dentro del dominio MPLS. Solo se encarga de **cambiar (swap)** etiquetas, lo que lo hace muy eficiente y rápido.
        
    
4. **Pregunta:** ¿Qué es una FEC (Forwarding Equivalence Class) en MPLS y cuál es su propósito?
    
    - **Respuesta:** Una FEC es un grupo de paquetes que se tratan de la misma manera (ej. se enrutan por el mismo camino, reciben la misma QoS) dentro del dominio MPLS. El propósito es agrupar el tráfico para aplicarles la misma etiqueta y el mismo tratamiento de reenvío, simplificando la conmutación.
    
5. **Pregunta:** Describa cómo se maneja el TTL (Time To Live) en MPLS y por qué es importante.
    
    - **Respuesta:** Cuando un paquete IP ingresa al dominio MPLS, su TTL se copia al campo TTL de la etiqueta MPLS. Los LSRs intermedios decrementan el TTL de la etiqueta. Al salir del dominio, el TTL de la etiqueta se copia de nuevo a la cabecera IP. Esto es importante para que el TTL de IP siga funcionando correctamente y refleje el número real de saltos a través del dominio MPLS.
    
6. **Pregunta:** Mencione al menos dos beneficios clave de implementar MPLS en la red de un proveedor de servicios.
    
    - **Respuesta:**
        
        - **Mayor eficiencia y velocidad de reenvío:** Gracias a la conmutación de etiquetas.
        - **Creación de VPNs (Virtual Private Networks):** Permite a los proveedores ofrecer redes privadas virtuales a sus clientes sobre una infraestructura compartida, aislando el tráfico de cada cliente.
        - **Ingeniería de Tráfico:** Permite dirigir el tráfico por rutas específicas para optimizar el uso de los recursos de la red.