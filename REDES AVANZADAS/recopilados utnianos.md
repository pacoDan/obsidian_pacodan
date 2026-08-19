https://bruno-cobos.notion.site/Preguntas-de-parciales-y-finales-99fce358ed944f6ab9a8660948794c9f

# Preguntas de parciales y finales

# Parciales

**Un router BGP descubre a sus vecinos mediante el RDP (router discovery protocol). Falso**

Se realiza la configuración de manera manual, no descubre a los vecinos.

**¿Cuál de las representaciones de la siguiente dirección IPv6 es válida?  
`2001:0db8:00f0:0000:0000:03d0:0000:00ff`**

- `2001:0db8:00f0:0:0:03d0:0000:00ff`
- `2001:0db8:00f0::03d0:0:00ff`
- `2001:0db8:00f0::3d0:0:00f`
- `2001:db8:f0:0:0:3d0:0:f`

¿Cuál de las representaciones de la siguiente dirección IPv6 es válida?  
`2001:0db8:0000:0000:0000:0000:0000:0c50`

- `2001:0db8::0c50`
- `2001:0db8:0:0:0:0:0:0c50`
- `2001:db8::c50`

**El número de Sistema Autónomo es un identificador de 2 o 4 bytes, y un parámetro obligatorio en la configuración del protocolo.**

**Verdadero**

**Cuáles son ciertas respecto de BGP**

1. La información de ruteo puede redistribuirse en RIP y viceversa.  
    ?
2. Es el protocolo preferido para el intercambio de información de ruteo en la  
    Internet. Verdadero
3. Permite la detección y autoconfiguración con peers en su misma red LAN. Falso. No hay detección, se debe configurar de manera manual.
4. Permite establecer sesiones de eBGP con vecinos distantes dentro de un mismo sistema autónomo.  
    Falso. eBGP son conexiones entre routers de distintos sistemas autónomos.

**Si la dirección de próximo salto no es alcanzable, las ruta informada es ignorada. Verdadero**

**El mensaje UPDATE sirve para transferir información de ruteo entre vecinos BGP.** **Verdadero**

**La etiqueta MPLS se inserta entre la cabecera de capa de red y la cabecera de capa transporte. Falso**

La etiqueta se inserta entre la capa 3 (Capa de red) y capa 2 (Capa de enlace)

**El protocolo IGMP permite el armado de “core-based trees”.** **Falso**

El protocolo IGMP que permite a los nodos unirse a un grupo Multicast. Para eso se utiliza el mensaje IGMP Report

**El Route Distinguisher permite unificar prefijos de diferentes longitudes.** Falso

**La dirección IP (origen) de la fuente de Multicast identifica al grupo.** Falso

Se identifican mediante una dirección multicast.

**En la implementación de VPNs, los P routers no conocen las rutas de los clientes.** Verdadero

**El LDP (Label Distribution Protocol) se encarga de distribuir las etiquetas fuera de la red MPLS.** Falso

**Si la dirección de próximo salto no es alcanzable, las ruta informada es ignorada.** Verdadero

**Un nodo solo puede participar de un grupo Multicast a la vez.** Falso

**El IGMP snooping evita el broadcast de contenido multicast en un switch.** Verdadero

**El protocolo PIM-SM (Sparse Mode) utiliza la solución de core-based tree con RP router.** Verdadero

Crea core-based trees así como source-based trees con Joins explícitos

# Finales

### Multicast

**Definición**

Multicasting es la comunicación entre uno-a-muchos y de muchos-a-muchos. Un mensaje desde un origen (fuente) hacía a algunos. Los destinatarios de ese mensaje son los miembros de un grupo multicast.

**¿Cuál es la aplicación de multicast?**

Las aplicaciones son:

- Multimedia.
    - Un grupo de usuarios sintonizan una transmisión de audio o video.
    - Una fuente genera contenido y lo transmite.
    - Múltiples receptores reciben una copia única.
- Teleconferencia.
    - La transmisión de un miembro de un grupo la reciben todos los miembros
- Cómputo distribuido.
    - Resultados intermedios son envíados a todos los participantes.
- Base de datos.
    - Todas las copias de un archivo son replicadas al mismo tiempo.
    - Esto es común cuando uno tiene un cluster, y uno de los miembros envía una actualización que tiene que aplicarse en múltiples destinos.

**Tipos de arboles formados en multicast**

- Shortest Path Tree o Source-Based Tree
    - Es un árbol que minimiza el costo desde el origen hacia cada receptor.
    - Bueno para un único origen.
    - Para más de un origen, un árbol por origen.
    - Fácil de calcular.
- Minimum-Cost Tree
    - Es un árbol con la menor cantidad de ramas
    - Bueno para múltiples orígenes
    - Muy caro de calcular (no es práctico para más de 30 nodos)
    - En la la práctica no se utiliza
- Core-based Tree
    - Un router es designado “core” (Rendezvous Point), también llamado _punto de encuentro,_ es decir*,* un nodo identificado en la red en donde se encuentra la fuente y el suscriptor.
    - Cada receptor utiliza el camino más corto para llegar a RP (reverse shortest path / Reverse path forwarding)

**¿Cómo se identificar multicast ipv4 e ipv6?**

Para ipv4, la clase D fue reservada para direcciones multicast. En ipv6, las direcciones multicast tienen el prefijo FF00::/8

**Clases de direcciones multicast**

**Caracteríticas del tráfico multicast**

Una fuente emite un mensaje. El mismo se transmite una única vez. Se determina el mejor camino a cada red donde haya miembros (el resultado es un spanning-tree). El paquete envíado es replicado en cada punto de bifurcación del árbol. El paquete tiene la dirección IP del grupo multicast, pero la fuente no sabe quienes son los miembros de ese grupo, sino que es la red quien se encarga de implementar los mecanismos necesarios para que el mensaje llegue a los miembros.

**¿Cómo se realiza el ruteo?**

El problema está en construir un árbol que conecte a todos los miembros del grupo multicast, es decir, que el origen puede enviarle una copia del mensaje a todos los miembros del grupo de la forma más eficiente posible.

Los protocolos de ruteo utilizan una de las siguientes soluciones:

- Source-based tree.
    - Se crea un shortest-path tree para cada origen
    - El árbol se crea desde el receptor al origen
- Core-based Tree
    - Construye un sólo árbol de distribución para todos los orígenes.
    - Un router es designado “core” que funciona como punto de encuentro entre la fuente y el suscriptor.
    - Cada receptor utiliza el camino más corto para llegar a RP (reverse shortest path / Reverse path forwarding)

Se generan entradas de ruteo en cada uno de los routers de la red para manejar el tráfico. Las entradas en tabla de ruteo son diferentes para source-based-trees y core-based trees:

- Source-based tree: `(Source, group)` o `(S, G)`
- Core-based tree: `(*, G)`

**¿Qué protocolos intervienen?**

- Distance Vector Multicast Routing Protocol (DVMRP)
    - Primer protocolo de ruteo para Multicast
    - Implementa flood-and-prune
- **Multicast Open Shortest Path First (MOSPF)**
    - Extensión para multicast para OSPF. Cada router calcula el shortest-path tree basado en su base de datos de link state.
    - No es muy utilizado.
- **Protocol Independent Multicast (PIM)**
    - PIM Dense Mode (PIM-DM) → crea un source-based tree utilizando flood-and-prune
        - Los subscriptores están juntos o agrupados
        - Si están todos juntos puedo plantear la idea de hacer un flood
    - PIM Sparse Mode (PIM-SM) → crea core-based trees así como source-based trees con Joins explícitos
        - Si los subscriptores están dispersos, conviene este enfoque

### MPLS

**Route Distinguishers**

**Definición**

MPLS es un mecanismo de conmutación basado en etiquetas (labels) que identifican las redes destino. Es una aproximación para reducir el tiempo de conmutación de los paquetes. Conmutar etiquetas es más rápido que conmutar diagramas que contienen direcciones IPs porque la etiqueta tiene una longitud fija, y en la tabla de ruteo voy a tener una entrada para esa etiqueta, por lo que voy a buscar por una coincidencia exacta. Está diseñado para transportar protocolos de capa 3

**Propagación TTL**

1. Al ingresar un paquete IP a la red MPLS, el TTL se decrementa en 1 y se copia de la cabecera IP a la etiqueta.
2. Al egreso de la red MPLS, el TTL se decrementa en 1 y se copia de la etiqueta a la cabecera IP.

**¿Que es un FEC?**

Una FEC (Forwarding Equivalent Class)es un grupo de paquetes tratados de la misma manera, sobre un mismo camino. La conmutación de paquetes en MPLS consiste en asignar un paquete a una FEC determinada y determinar el próximo salto para cada FEC.

El router de border analiza el tráfico entrante y en fucnión de la dirección IP destino y origen y el tipo de servicio, determina si eso corresponde a una FEC existente, para lo cual ya hay una etiqueta asignada. Si corresponde a una nueva FEC, tiene que asignar una nueva etiqueta.

**¿Qué es TTL y cómo se relaciona con IP?**

Time To Live (TTL) es un mecanismo que consiste en un campo que indica el tiempo que un paquete tiene antes de que su vida termine y sea descartado. El TTL es descontado en 1 por cada salto que realiza el paquete (viajar de un nodo a otro). Si el TTL llega a 0, el paquete es descartado.

**¿Qué es LSR? ¿Qué tipos hay?**

Un Label switch router (LSR) es un router que soporta MPLS, es decir, es capaz de entener las etiqueta MPLS y de recibir y transmitir paquetes etiquetados. Existen tres tipos de LSRs:

- _Ingress LSRs_ ⇒ **Reciben un paquete que no está etiquetado y le insertan una etiqueta.
- _Egress LSRs_ ⇒ Reciben un paquete etiquetado, remueve la etiqueta (label) y lo envía en un enlace de datos.
- _Intermediate LSRs_ ⇒ Reciben un paquete etiquetado, realizan una operación sobre el, switchean el paquete y lo envían por el enlace de datos correcto.

Los primeros dos tipos se los denomina _Edge LSR_ o _provider edge_ (PE). A los LSR intermedios se los denomina _provider_ (P)

## IPSec

**¿Qué modos de operación hay?**

- Modo transporte
- Modo túnel

**¿Qué protocolos intervienen?**

- AH Protocol
    - Autenticidad del origen
    - Integridad de los datos
- ESP Protocol
    - Autenticidad del origen
    - Integridad de los datos
    - Privacidad