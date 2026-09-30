## Resumen Estructurado: MPLS (Multi-Protocol Label Switching)

MPLS es una tecnología de red que mejora la eficiencia del reenvío de paquetes al combinar la conmutación de Capa 2 con el enrutamiento de Capa 3. Se considera un protocolo de **Capa 2.5**.

### **I. Problema que Resuelve MPLS**

En las redes IP tradicionales, cada router realiza las siguientes operaciones para cada paquete:

- Lee la dirección IP de destino en la cabecera del datagrama.
- Realiza una búsqueda de "mejor coincidencia" (longest match) en su tabla de enrutamiento para determinar el siguiente salto.
- Esta búsqueda se realiza **independientemente en cada salto**, lo que puede ser computacionalmente intensivo y generar latencia, especialmente en routers de core con grandes tablas de enrutamiento y alto volumen de tráfico.

### **II. Concepto Fundamental de MPLS: Conmutación por Etiquetas**

MPLS introduce un mecanismo de reenvío basado en **etiquetas** (labels) en lugar de direcciones IP.

- **Etiqueta (Label):** Un identificador corto (4 bytes) de longitud fija y con significado local para cada router.
- **Búsqueda de Coincidencia Exacta:** Los routers MPLS leen la etiqueta y realizan una búsqueda de coincidencia exacta en una tabla de conmutación de etiquetas. Esta operación es mucho más rápida y eficiente que la búsqueda de "mejor coincidencia" de IP.
- **Multiprotocolo:** Al conmutar etiquetas, MPLS es independiente del protocolo de Capa 3 subyacente (IP, IPX, etc.), permitiendo transportar diferentes tipos de tráfico de manera uniforme.

### **III. Arquitectura y Componentes de MPLS**

**A. Planos del Router:** Los routers tienen dos planos principales:

- **Plano de Control:**
    
    - Aquí operan los protocolos de enrutamiento (ej., OSPF, BGP) para aprender las rutas IP.
    - También operan los **protocolos de distribución de etiquetas** (ej., LDP - Label Distribution Protocol), que se encargan de intercambiar información sobre las etiquetas entre los LSRs.
    - Toda la información procesada (rutas IP y mapeos de etiquetas) se escribe en el Plano de Datos.
    
- **Plano de Datos (Forwarding Plane):**
    
    - Es el plano ejecutivo donde se realiza el reenvío de paquetes.
    - Contiene la **Tabla de Enrutamiento IP (FIB - Forwarding Information Base)**.
    - Contiene la **Tabla de Conmutación de Etiquetas (LFIB - Label Forwarding Information Base)**, que mapea etiquetas de entrada a etiquetas de salida y próximos saltos.
    

**B. Componentes de la Red MPLS:**

- **Dominio MPLS:** La parte de la red donde se utiliza MPLS.
- **LSR (Label Switching Router):** Cualquier router que soporta MPLS y puede conmutar etiquetas.
- **Edge LSR (E-LSR) / Provider Edge (PE) Router:**
    
    - Se ubican en el **borde** del dominio MPLS.
    - **Funciones de Ingreso:** Reciben paquetes IP de redes no-MPLS, los clasifican en FECs y les **añaden (push)** una etiqueta MPLS.
    - **Funciones de Egreso:** Reciben paquetes etiquetados, les **quitan (pop)** la etiqueta MPLS y reenvían el paquete IP a redes no-MPLS.
    
- **Core LSR / Provider (P) Router:**
    
    - Se ubican en el **core** del dominio MPLS.
    - **Funciones:** Solo conmutan etiquetas. Reciben un paquete con una etiqueta, la **cambian (swap)** por una nueva etiqueta y lo reenvían. Son los más eficientes.
    

### **IV. Conceptos Clave en MPLS**

**A. Etiqueta (Label):**

- **Identificador:** Un valor corto (20 bits) que identifica una FEC.
- **Longitud Fija:** 4 bytes (32 bits).
- **Significado Local:** El valor de la etiqueta tiene significado solo para el LSR que la recibe.
- **Formato:**
    
    - **Label (20 bits):** El valor numérico de la etiqueta.
    - **Exp (3 bits):** Bits experimentales, a menudo usados para QoS.
    - **S (Stack Bit - 1 bit):** Indica si es la última etiqueta en la pila (MPLS soporta apilamiento de etiquetas para escenarios complejos como VPNs jerárquicas).
    - **TTL (Time To Live - 8 bits):** Similar al TTL de IP, se decrementa en cada salto LSR.
    

**B. FEC (Forwarding Equivalence Class):**

- **Definición:** Un grupo de paquetes que comparten los mismos atributos de reenvío (ej., mismo destino, mismo QoS, misma VPN) y, por lo tanto, se tratan de la misma manera sobre un mismo camino MPLS.
- **Asignación:** La clasificación de un paquete en una FEC se realiza en el E-LSR de entrada.
- **Propósito:** Permite aplicar el mismo tratamiento (misma etiqueta, mismo camino) a un conjunto de tráfico.

**C. LSP (Label Switched Path):**

- **Definición:** Es el camino de conmutación de etiquetas. Una ruta predefinida a través de uno o más LSRs que sigue un paquete de una FEC particular.
- **Establecimiento:** Los LSPs se establecen mediante los protocolos de distribución de etiquetas (ej., LDP).

### **V. Funcionamiento de MPLS (Flujo de Paquetes)**

1. **Ingreso al Dominio MPLS (E-LSR de Entrada):**
    
    - Un paquete IP llega a un E-LSR.
    - El E-LSR examina la cabecera IP y clasifica el paquete en una FEC.
    - Consulta su LFIB y **añade (push)** una etiqueta MPLS al paquete. La etiqueta se inserta entre la cabecera de Capa 2 (ej., Ethernet) y la cabecera IP.
    - El paquete etiquetado se reenvía al siguiente LSR en el LSP.
    
2. **Dentro del Dominio MPLS (Core LSRs):**
    
    - Un Core LSR recibe un paquete etiquetado.
    - Lee la etiqueta de entrada.
    - Consulta su LFIB, que le indica la etiqueta de salida y el próximo salto.
    - **Cambia (swap)** la etiqueta de entrada por la etiqueta de salida y reenvía el paquete al próximo LSR. Esta operación es muy rápida.
    
3. **Salida del Dominio MPLS (E-LSR de Salida):**
    
    - El E-LSR de salida recibe el paquete etiquetado.
    - Consulta su LFIB, que le indica que debe **quitar (pop)** la etiqueta.
    - El paquete IP original (sin etiqueta MPLS) se reenvía a la red de destino.
    

### **VI. Manejo del TTL en MPLS**

- **Copia de TTL (Push):** Cuando un paquete IP ingresa al dominio MPLS, el valor del TTL de la cabecera IP se copia al campo TTL de la etiqueta MPLS.
- **Decremento en LSRs:** Cada LSR en el LSP decrementa el TTL de la etiqueta.
- **Copia de Salida (Pop):** Cuando el paquete sale del dominio MPLS, el valor del TTL de la etiqueta se copia de nuevo a la cabecera IP.
- **Importancia:** Asegura que el TTL de IP refleje el número real de saltos a través de la red, incluso dentro del dominio MPLS.
- **Opción de "Esconder" TTL:** Si no se realiza la copia del TTL al entrar y salir, el dominio MPLS se percibe como un único salto para el TTL de IP, ya que el TTL original de IP no se decrementa dentro del dominio.

### **VII. MPLS como Capa 2.5**

- **Posición:** Se inserta entre la Capa 2 (enlace de datos) y la Capa 3 (red).
- **Funciones Limitadas:** No realiza todas las funciones de Capa 2 (ej., control de errores, delimitación de tramas) ni de Capa 3 (ej., enrutamiento completo, fragmentación).
- **Integración:** Integra lo mejor de la Capa 3 (enrutamiento inteligente) con lo mejor de la Capa 2 (conmutación rápida).

### **VIII. Beneficios y Aplicaciones de MPLS**

- **Aceleración del Reenvío:** Principal beneficio, gracias a la conmutación de etiquetas.
- **Calidad de Servicio (QoS):** Permite implementar políticas de QoS de manera granular por FEC.
- **Ingeniería de Tráfico:** Permite dirigir el tráfico por rutas específicas (LSPs) para optimizar el uso de los recursos de la red, balancear cargas o evitar congestión.
- **VPNs (Virtual Private Networks):** Una de las aplicaciones más importantes. Permite a los proveedores ofrecer servicios de VPN a sus clientes sobre una infraestructura compartida, creando redes privadas virtuales aisladas.
- **Soporte Multiprotocolo:** Puede transportar IPv4, IPv6 y otros protocolos de Capa 3.
- **Facilita Nuevos Servicios:** Permite la creación de servicios que no serían posibles con el enrutamiento IP tradicional.

---

## Posibles Preguntas y Respuestas para Exámenes

### **Preguntas Conceptuales:**

1. **Pregunta:** ¿Cuál es el problema fundamental que MPLS busca resolver en las redes IP tradicionales?
    
    - **Respuesta:** El problema es la ineficiencia de la búsqueda de "mejor coincidencia" (longest match) en las tablas de enrutamiento IP que se realiza en cada salto. MPLS lo resuelve con una conmutación de etiquetas más rápida basada en coincidencia exacta.
    
2. **Pregunta:** ¿Por qué se considera a MPLS un protocolo de "Capa 2.5"?
    
    - **Respuesta:** Porque se inserta entre la Capa 2 (enlace de datos) y la Capa 3 (red). No realiza todas las funciones de ninguna de las dos capas, sino que toma aspectos de enrutamiento (Capa 3) y conmutación rápida (Capa 2) para optimizar el reenvío.
    
3. **Pregunta:** Defina qué es una FEC (Forwarding Equivalence Class) en MPLS y cuál es su propósito.
    
    - **Respuesta:** Una FEC es un grupo de paquetes que comparten los mismos atributos de reenvío (ej., mismo destino, mismo QoS, misma VPN) y, por lo tanto, se tratan de la misma manera sobre un mismo camino MPLS. Su propósito es agrupar el tráfico para aplicarles la misma etiqueta y el mismo tratamiento de reenvío, simplificando la conmutación.
    
4. **Pregunta:** ¿Qué es un LSP (Label Switched Path) y cómo se establece?
    
    - **Respuesta:** Un LSP es el camino predefinido a través de uno o más LSRs que sigue un paquete de una FEC particular dentro del dominio MPLS. Se establece mediante protocolos de distribución de etiquetas, como LDP (Label Distribution Protocol).
    

### **Preguntas de Funcionamiento y Componentes:**

1. **Pregunta:** Describa la diferencia de funciones entre un Edge LSR (PE router) y un Core LSR (P router) en un dominio MPLS.
    
    - **Respuesta:**
        
        - **Edge LSR (PE router):** En el borde del dominio. Recibe IP, clasifica en FEC, **añade (push)** etiqueta. Al salir, **quita (pop)** etiqueta y reenvía IP.
        - **Core LSR (P router):** En el core del dominio. Solo **cambia (swap)** etiquetas. Son los más eficientes.
        
    
2. **Pregunta:** Explique el proceso de "push", "swap" y "pop" de etiquetas en un LSP.
    
    - **Respuesta:**
        
        - **Push:** El E-LSR de entrada añade una etiqueta al paquete IP.
        - **Swap:** Los Core LSRs intermedios cambian la etiqueta de entrada por una nueva etiqueta de salida.
        - **Pop:** El E-LSR de salida quita la etiqueta del paquete antes de reenviarlo como IP.
        
    
3. **Pregunta:** ¿Cómo se maneja el TTL (Time To Live) en MPLS y por qué es importante que se copie entre la cabecera IP y la etiqueta MPLS?
    
    - **Respuesta:** El TTL de IP se copia al TTL de la etiqueta al entrar al dominio MPLS. Los LSRs decrementan el TTL de la etiqueta. Al salir, el TTL de la etiqueta se copia de nuevo a la cabecera IP. Es importante para que el TTL de IP refleje el número real de saltos a través de la red, incluso dentro del dominio MPLS, y para que los mecanismos de detección de bucles y descarte de paquetes funcionen correctamente.
    
4. **Pregunta:** ¿Qué información contiene una etiqueta MPLS de 4 bytes?
    
    - **Respuesta:** Contiene el valor de la **Label (20 bits)**, bits **Exp (3 bits)** para uso experimental/QoS, el **Stack Bit (S - 1 bit)** que indica si es la última etiqueta en la pila, y el **TTL (8 bits)**.
    

### **Preguntas de Aplicación y Beneficios:**

1. **Pregunta:** Mencione dos beneficios clave de implementar MPLS en la red de un proveedor de servicios.
    
    - **Respuesta:**
        
        - **Mayor eficiencia y velocidad de reenvío:** Gracias a la conmutación de etiquetas.
        - **Creación de VPNs (Virtual Private Networks):** Permite a los proveedores ofrecer redes privadas virtuales a sus clientes sobre una infraestructura compartida, aislando el tráfico de cada cliente.
        - **Ingeniería de Tráfico:** Permite dirigir el tráfico por rutas específicas para optimizar el uso de los recursos de la red.
        
    
2. **Pregunta:** ¿Cómo contribuye MPLS a la implementación de Calidad de Servicio (QoS) y la Ingeniería de Tráfico?
    
    - **Respuesta:**
        
        - **QoS:** Los bits Exp en la etiqueta MPLS pueden usarse para marcar el tráfico y aplicar diferentes políticas de QoS por FEC.
        - **Ingeniería de Tráfico:** Permite establecer LSPs explícitos que no necesariamente siguen la ruta más corta de IP, lo que permite balancear cargas, evitar congestión en enlaces específicos o priorizar cierto tráfico.
        
    
3. **Pregunta:** ¿Qué significa que MPLS es "multiprotocolo"?
    
    - **Respuesta:** Significa que MPLS puede transportar y conmutar paquetes de diferentes protocolos de Capa 3 (como IPv4, IPv6, IPX, etc.) de manera uniforme, ya que su decisión de reenvío se basa en la etiqueta y no en la cabecera del protocolo de Capa 3.