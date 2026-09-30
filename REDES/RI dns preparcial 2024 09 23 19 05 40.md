A continuación, se presenta un resumen estructurado de la transcripción, incluyendo detalles clave y posibles preguntas de examen con sus respuestas:

### **Sondeo DNS y Aclaraciones sobre Segmentos de Wireshark**

#### **1. DNS (Sistema de Nombres de Dominio)**

El DNS es un sistema fundamental en las redes de datos que permite traducir nombres de dominio legibles por humanos (ej. google.com) a direcciones IP numéricas que las computadoras utilizan para comunicarse.

**1.1. Origen y Evolución**

- **Archivo HOSTS:** Inicialmente, la resolución de nombres se realizaba mediante un archivo de texto local llamado "HOSTS" en cada dispositivo.
    
    - **Limitaciones:**
        
        - **Espacio de nombres plano:** No permitía nombres duplicados.
        - **Mantenimiento tedioso:** Requería actualizar manualmente el archivo en cada dispositivo.
        - **Centralización:** Stanford era el host principal del archivo, lo que generaba saturación y problemas de escalabilidad.
        
    
- **Sistema Cliente-Servidor:** Para superar estas limitaciones, se creó el DNS, un sistema distribuido cliente-servidor.
    
    - **Resolver:** El cliente DNS que se ejecuta en el dispositivo del usuario.
    - **Name Server:** El servidor DNS que resuelve los nombres.
    

**1.2. Estructura del Espacio de Nombres DNS**

- **Jerarquía:** El espacio de nombres DNS es jerárquico, comenzando desde la raíz.
    
    - **Root (Raíz):** Representado por un punto (.).
    - **Top-Level Domains (TLD):** Dominios de nivel superior (ej. .com, .edu, .org, .ar, .cl).
    - **Second-Level Domains:** Dominios de segundo nivel (ej. google.com, utn.edu.ar).
    - **Subdominios:** Niveles adicionales dentro de un dominio (ej. frba.utn.edu.ar, www.frba.utn.edu.ar).
    
- **Delegación:** La administración de los nombres se delega a diferentes entidades en cada nivel de la jerarquía.
    
    - Los servidores raíz tienen referencias a los servidores de los TLDs.
    - Las entidades como NIC (Network Information Center) en Argentina (nic.ar) gestionan los dominios de su país.
    - Las organizaciones (ej. UTN) son responsables de sus propios dominios y subdominios.
    
- **FQDN (Fully Qualified Domain Name):** El nombre completo de un host, incluyendo todos sus dominios hasta la raíz (ej. www.frba.utn.edu.ar.).

**1.3. Zonas de Autoridad y Resource Records (RR)**

- **Archivo de Zona:** Un dominio se implementa en un archivo de texto llamado "archivo de zona" en un servidor DNS.
- **Resource Records (RR):** ==Son lineas o entradas en el archivo zona o dentro del archivo de zona, que asocian nombres con información específica, es decir indica la asociación de un mensaje y una dirección IP.==
==Implementar un dominio es escribir en el archivo de zona (en un servidor primario)==
==Si creo mi dominio, en algún lado del mundo hay un servidor DNS que tenga mi archivo zona para mi dominio.==
==Las entradas son diferentes y se conocen estos tres:==
    - **Tipo A:** Asocia un nombre de host con una dirección IPv4. ==Asocia estáticamente el nombre de un host con una dirección IP.==
    - **Tipo AAAA (Cuádruple A):** Asocia un nombre de host con una dirección IPv6.
    - **Tipo MX (Mail Exchange):** Contiene nombres asociados con las direcciones de servidores de correo, indicando a dónde enviar los correos electrónicos para un dominio. ==Asocia dominio de email con la dirección de servidores  de correo.==
    - **Tipo CNAME (Canonical Name):** Permite que un nombre se resuelva como otro (alias), útil para delegación o re dirección. ==Permite asociar mas de  un nombre de host a una única dirección IP (alias).==
    
- **TTL (Time To Live):** Define por cuánto tiempo es válida una respuesta DNS en la caché de un servidor o cliente. Esto mejora el rendimiento al evitar consultas repetidas.

**1.4. Roles de los Servidores DNS**

- **Servidor Primario:** Donde se crea y gestiona el archivo de zona original de un dominio.
- **Servidor Secundario:** Replica la información del servidor primario para proporcionar redundancia y alta disponibilidad.
- **Servidores Caché/Resolutores:** Servidores que no alojan zonas, pero atienden las peticiones de resolución de nombres de los clientes, realizando consultas recursivas e iterativas.

**1.5. Proceso de Resolución de Nombres**

- **Consulta Recursiva:** El resolver (cliente) envía una consulta a su servidor DNS configurado (ej. el del ISP), esperando una respuesta definitiva (la IP o un error).
- **Consultas Iterativas:** El servidor DNS consultado, si no tiene la respuesta en caché, inicia una serie de consultas iterativas a otros servidores DNS en la jerarquía (Root, TLD, etc.) hasta encontrar la autoridad para el dominio solicitado.
- **Consulta Inversa:** Permite preguntar el nombre asociado a una dirección IP (ej. utilizado por `traceroute` o SMTP para validación).

**1.6. Protocolos y Puertos**

- **UDP Puerto 53:** Las consultas DNS utilizan UDP en el puerto 53 para una resolución rápida y ágil.
- **TCP Puerto 53:** Se utiliza para transferencias de zona entre servidores DNS y para consultas DNS que superan el tamaño de un paquete UDP.

---

#### **2. Aclaraciones sobre Segmentos de Wireshark (Análisis de Trama Ethernet)**

El análisis de capturas de tráfico con herramientas como Wireshark es crucial para entender el funcionamiento de las redes.

**2.1. Formato de la Trama Ethernet**

- **Memorización:** Es fundamental memorizar el formato de la trama Ethernet para el primer parcial.
- **Campos Clave:**
    
    - **Dirección MAC Destino:** La dirección MAC del siguiente salto (router, switch, host).
    - **Dirección MAC Origen:** La dirección MAC del dispositivo que envía la trama.
    - **Tipo/Longitud:** Indica el protocolo de capa superior (ej. IP) o la longitud de los datos.
    
- **Preámbulo:** No se muestra en Wireshark, ya que es una secuencia de bits utilizada para la sincronización a nivel físico.

**2.2. Transmisión de la MAC Address**

- **Orden de Transmisión:** La MAC address se transmite al revés (el octavo bit se transmite primero).
- **Visualización en Wireshark:** Wireshark siempre muestra la MAC address en el orden correcto (derecho), no en el orden de transmisión.
- **Identificador de Grupo (Multicast/Unicast):** El octavo bit de la MAC address indica si es una dirección unicast (0) o multicast (1).

**2.3. Identificación de IP Origen y Destino en Wireshark**

- **Cabecera IP:** La cabecera IP siempre comienza con "45" (versión 4, 5 palabras de cabecera).
- **Direcciones IP:** Las últimas dos palabras de la cabecera IP corresponden a la dirección IP de origen y destino.

**2.4. MTU (Maximum Transmission Unit) y Fragmentación**

- **Límite Ethernet:** La longitud máxima del campo de datos en una trama Ethernet es de aproximadamente 1500 bytes.
- **Datagrama IP:** Un datagrama IP puede tener una longitud máxima de hasta 65535 bytes.
- **Fragmentación:** Si un datagrama IP es más grande que la MTU de la interfaz de red, debe ser fragmentado. Sin embargo, es más eficiente que la capa IP conozca la MTU y envíe paquetes que no requieran fragmentación.
- **Jumbo Frames:** En situaciones especiales, Ethernet puede soportar "Jumbo Frames" de hasta 9000 o 16000 bytes, permitiendo datagramas IP más grandes sin fragmentación.

**2.5. Colisiones en Redes Inalámbricas (Wireless)**

- **Detección de Colisiones:** En redes inalámbricas, no hay una detección directa de colisiones como en Ethernet cableado (CSMA/CD).
- **Mecanismo:** Si ocurre una colisión, se interpreta como un fallo en la transmisión. El emisor no recibe el `ACK` (acknowledgement) y retransmite el paquete.
- **Problema del Nodo Oculto:** Un nodo puede colisionar con otro sin que el primero lo "vea" directamente. La falta de `ACK` es la señal de que algo salió mal.
- **No se sensa el medio durante la transmisión:** A diferencia de las redes cableadas, en wireless no se sensa el medio mientras se transmite.

---

#### **Posibles Preguntas de Examen y Respuestas**

**Preguntas sobre DNS:**

1. **Pregunta:** ¿Cuáles fueron las principales limitaciones del archivo HOSTS que llevaron a la creación del DNS?
    
    - **Respuesta:** Las limitaciones incluían un espacio de nombres plano (no permitía nombres duplicados), mantenimiento tedioso y centralizado (requería actualizaciones manuales y saturaba el servidor principal en Stanford).
    
2. **Pregunta:** Explique la diferencia entre una consulta DNS recursiva y una iterativa.
    
    - **Respuesta:** Una **consulta recursiva** es realizada por el resolver (cliente) a su servidor DNS configurado, esperando una respuesta definitiva (la IP o un error). Una **consulta iterativa** es realizada por un servidor DNS a otros servidores en la jerarquía (Root, TLD, etc.) para encontrar la autoridad del dominio solicitado, recibiendo referencias a otros servidores hasta llegar al que tiene la información.
    
3. **Pregunta:** Mencione y describa brevemente al menos tres tipos de Resource Records (RR) en un archivo de zona DNS.
    
    - **Respuesta:**
        
        - **Tipo A:** Asocia un nombre de host con una dirección IPv4.
        - **Tipo MX (Mail Exchange):** Indica los servidores de correo responsables de un dominio.
        - **Tipo CNAME (Canonical Name):** Crea un alias para un nombre de host existente.
        - **Tipo AAAA:** Asocia un nombre de host con una dirección IPv6.
        
    
4. **Pregunta:** ¿Qué es el TTL en DNS y cuál es su importancia?
    
    - **Respuesta:** TTL (Time To Live) es un valor que indica por cuánto tiempo una respuesta DNS puede ser almacenada en caché por un servidor o cliente. Su importancia radica en mejorar el rendimiento del sistema DNS al reducir la cantidad de consultas repetidas a los servidores autoritativos, disminuyendo la carga de la red y acelerando la resolución de nombres.
    
5. **Pregunta:** ¿Por qué las consultas DNS utilizan UDP en el puerto 53? ¿En qué caso se utiliza TCP en el puerto 53?
    
    - **Respuesta:** Las consultas DNS utilizan UDP en el puerto 53 porque es un protocolo sin conexión, lo que permite una resolución de nombres rápida y eficiente, ideal para la naturaleza de las consultas DNS que requieren agilidad. TCP en el puerto 53 se utiliza principalmente para transferencias de zona entre servidores DNS (cuando un servidor secundario sincroniza su base de datos con el primario) y para consultas DNS que superan el tamaño máximo de un paquete UDP.
    

**Preguntas sobre Wireshark y Trama Ethernet:**

1. **Pregunta:** Si estás analizando una captura de Wireshark y ves una dirección MAC, ¿en qué orden se muestra y cómo se relaciona con el orden de transmisión real?
    
    - **Respuesta:** Wireshark siempre muestra la dirección MAC en el orden correcto (derecho). Sin embargo, en la transmisión real a nivel físico, la MAC address se transmite al revés, comenzando por el octavo bit.
    
2. **Pregunta:** ¿Cómo puedes identificar la dirección IP de origen y destino en una trama IP capturada con Wireshark?
    
    - **Respuesta:** La cabecera IP siempre comienza con los bytes "45" (indicando IPv4 y una cabecera de 5 palabras). Las últimas dos palabras (los últimos 8 bytes) de la cabecera IP corresponden a la dirección IP de origen y la dirección IP de destino, respectivamente.
    
3. **Pregunta:** ¿Cuál es la relación entre la MTU de una interfaz Ethernet y el tamaño máximo de un datagrama IP? ¿Qué sucede si un datagrama IP excede la MTU?
    
    - **Respuesta:** La MTU (Maximum Transmission Unit) de una interfaz Ethernet tradicional es de aproximadamente 1500 bytes, que es el tamaño máximo de datos que puede transportar una trama Ethernet. Un datagrama IP puede ser mucho más grande (hasta 65535 bytes). Si un datagrama IP excede la MTU de la interfaz por la que debe ser enviado, el datagrama debe ser fragmentado en segmentos más pequeños que se ajusten a la MTU de la interfaz.
    
4. **Pregunta:** En redes inalámbricas, ¿cómo se detectan las colisiones si no hay un mecanismo directo como en Ethernet cableado?
    
    - **Respuesta:** En redes inalámbricas, no hay una detección directa de colisiones. En su lugar, una colisión se interpreta como un fallo en la transmisión. El emisor espera un `ACK` (acknowledgement) del receptor. Si no recibe el `ACK` dentro de un tiempo determinado, asume que hubo una colisión o un error de transmisión y retransmite el paquete.
    
5. **Pregunta:** ¿Qué es el "wildcard" en el contexto de las Access Lists de Cisco para la configuración de túneles IPsec?
    
    - **Respuesta:** El "wildcard" en las Access Lists de Cisco es la inversa de la máscara de subred. Donde en una máscara de subred se usarían 255 para indicar bits de red y 0 para bits de host, en un wildcard se usan 0 para indicar bits de red y 255 para bits de host. Por ejemplo, una máscara de subred de `255.255.255.0` se representaría con un wildcard de `0.0.0.255`. Se utiliza para definir rangos de direcciones IP en las reglas de los Access Lists.