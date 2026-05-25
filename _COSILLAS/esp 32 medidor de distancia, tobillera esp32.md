Un enfoque adecuado para este sistema es tratarlo como un sistema de **proximidad inalámbrica basada en RSSI Wi-Fi** (Received Signal Strength Indicator), donde cada ESP32-CAM mide la intensidad de señal del otro ESP32 para estimar distancia aproximada.

Como no buscás precisión centimétrica sino una detección aproximada (“más de 2 metros”), el RSSI es suficiente y evita agregar sensores ultrasónicos, ToF o BLE externos.

## Arquitectura general

![Image](https://images.openai.com/static-rsc-4/U_peOAcPcmQH7UB8otL8rYFnC8DdYgiLwhIVFc6fa1wq9txuiymE8YZ7j59ka3EwIQXAADVzYTsscAJbScfgRfIE94ufgFZYT-KbjL0tSLSR_FKen55HTXf-SnYXl1kEyqtbrL4nVrT2hvPZKFYjJHTu12HcIqjAJQQrmfju9CdRUB4_RyDou8dbj4MdEzm2?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/qshmHE5HZF-UY7tJqIa8GmHYa6RjvZIOQrkZ9vcfNxYXgCDq2jrt73vPFPI3mCxCkZvkA1iNd32gxZFapCg1cxnfG_qvBxu7T-Le8F7unnpdcNBhy9Nqn9c9Vh_j1EwS3FtTQ-zcJf9LpmGxOZUxslRMO_AKyGGr4vxJoCzjD1zjk3sYhbxCwNfqaWvZ3T7q?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/V36K279erDPaUh4zmWBZoTrreRC7lc_yM7F9NFlLGuP1KyZiPl36wC4Afa9NhLZGTNGB-ymstPwDbbp1m3t7CmDmVDQ8HQvea2_IUdtFgfsVtXv5zRJx7Bti1a0mak0PFQSgnU5zlFQKLI3MBO1-sdwWW1kIkkK6S4_G5Z7-B3etWeX8f18MOuVrChad_Fxz?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/S-OhQjey_PaeZ4HFIeNIwpt7gCV2Xxqsy9EfgYN2z_hIEFG5RuOFFS8h1eBP5cdwt4-uHOqYq8-3LXBjhLM2pF7LvqMtW4bPg5nixK5VwXTPEngp7Ys2B-Fy67AB2nty5PuVrC3AE2TPDToxTvt4d6zc00U4g3RHgnRMFhBfgG_GEL7TINf6dEwmEU8QCO39?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Co_mWf16KtI04yW9CtLKb5tmSQGfGQd1Gg5SRJYzWyXKuhoJHPsPGk3yuQNqDji9gowmVQn1RhvNolDSkacHEZP3IN4cd8tUgciCYILR9TIGpOsgv-WNRJ-ZTG3DbVDVc5zsM854Yl0wct1Uzel27cG2h1nwL0ncbr553HQCWRT23s0pMH5fiDq6TPOTJPfR?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/xjznTTQKVUWG6g1TRSu1W5AJvFQTutXanoXdZqOzAZdUohGtpf-3XBLFxtB2AwhvmaNLxRth_cbu8Ctoe4bXk8yGCh0p6KQkTi5SCgVI7Ka77R91Bxc0FvZK_uj7A8knhXpNfiVDq4tTmIFe9C2I_rHJ1tvHh-1nmONf9okC5v-rdmFuq40HcrezKE8rjCjv?purpose=fullsize)

## Componentes del sistema

### Nodo A — ESP32-CAM

- Modo Wi-Fi STA/AP o ESP-NOW
    
- Emite beacon o paquetes periódicos
    
- Mide RSSI recibido del Nodo B
    
- Calcula distancia aproximada
    
- Enciende LED si supera umbral
    

### Nodo B — ESP32-CAM

- Igual comportamiento que Nodo A
    
- Ambos nodos son simétricos
    

### Canal inalámbrico Wi-Fi

- Utiliza:
    
    - RSSI
        
    - potencia de señal
        
    - pérdida por propagación
        

### Lógica de proximidad

Convierte RSSI → distancia aproximada mediante modelo de propagación.

Ejemplo típico:

- −40 dBm → muy cerca
    
- −55 dBm → ~1 metro
    
- −65 dBm → ~2 metros
    
- −75 dBm → lejos
    

Esto depende muchísimo de:

- paredes
    
- orientación
    
- interferencia
    
- antena
    
- rebotes
    

Pero para “se alejó demasiado” funciona bastante bien.

---

# Diagrama de Arquitectura (PlantUML)

```plantuml
@startuml

skinparam componentStyle rectangle

node "ESP32-CAM A" as A {

  component "WiFi Driver" as WIFI_A
  component "RSSI Scanner" as RSSI_A
  component "Distance Estimator" as DIST_A
  component "Alert Controller" as ALERT_A
  component "Flash LED" as LED_A

  WIFI_A --> RSSI_A
  RSSI_A --> DIST_A
  DIST_A --> ALERT_A
  ALERT_A --> LED_A
}

cloud "WiFi Channel\n(2.4 GHz RSSI)" as WIFI

node "ESP32-CAM B" as B {

  component "WiFi Driver" as WIFI_B
  component "RSSI Scanner" as RSSI_B
  component "Distance Estimator" as DIST_B
  component "Alert Controller" as ALERT_B
  component "Flash LED" as LED_B

  WIFI_B --> RSSI_B
  RSSI_B --> DIST_B
  DIST_B --> ALERT_B
  ALERT_B --> LED_B
}

A <--> WIFI
B <--> WIFI

@enduml
```

---

# Flujo lógico

## 1. Beacon Wi-Fi

Cada ESP32:

- transmite paquetes periódicos
    
- o mantiene conexión Wi-Fi activa
    

---

## 2. Medición RSSI

El ESP32 obtiene algo similar a:

```c
WiFi.RSSI()
```

o scan:

```c
WiFi.scanNetworks()
```

similar a:

```bash
nmcli dev wifi list
```

---

## 3. Estimación de distancia

Modelo simplificado:

d = 10^{\frac{(A - RSSI)}{10n}}

Donde:

- `d` = distancia aproximada
    
- `A` = RSSI a 1 metro
    
- `n` = factor ambiental (2 a 4)
    
- `RSSI` = señal medida
    

Ejemplo:

- RSSI = −67 dBm
    
- Resultado ≈ 2 metros
    

---

## 4. Umbral

```text
SI distancia > 2 m
    encender LED
SINO
    apagar LED
```

---

# Arquitectura de comunicación recomendada

## Opción 1 — Wi-Fi STA/AP

Un ESP32:

- Access Point
    

Otro:

- Station
    

Ventajas:

- simple
    
- estable
    
- RSSI directo
    

Desventajas:

- más consumo
    

---

## Opción 2 — ESP-NOW (RECOMENDADO)

![Image](https://images.openai.com/static-rsc-4/0fcc5EBm-Lwd-2ZVfIi_Pm1vWOfm5isEBihwzfO7B34puW3qkPiY6e8xa-psCc_6JkiBl76J1yCrLqJrxobOo4amGsh3a-xOArVJpLcFUFqFtLEkisxapZ_zZAmlKgadAPy8A0eT-memlvzAE9xOgHcMVNucg2VPmvwH6ADXJsFxhuVa4--jTmJ6tHvX-E4g?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/yHcGgvq0DVxXKsiODgGALH7_Tirj0oVT6M5OjvkFXmIALq7QSkW-QfWLJFDF4vNqhmGQcnrxG2q6tdPsm91NoCADYZDsscumffu3gDudwX3C0TXLwEn7oNJGSBWbZjsIYHjLDji3ERvXBSLUxmxyntbdj4UR0iFLrEnc7zLKU6YoAeiOr23HSlx3gJuuxMCs?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/KNGAPuXlsq_2dnjfMdpBNRYm6PcLftTJmq987FdAkN5F-7qBBnhk-1fZnCu9A_iWKbbGY9j_54ZzJTQWaiXIW892PAfos5iC7Sw3De254cktlVj_GVysc7mrAvWeNAS7c7jmJ-L_5gZBjMVPlZ6q5PY2yFTizBDJsnhp2tmE2fDiV7zw-RfD6ugnNyHYzxc2?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/7z07grwMO5ImXk1VtYtk9Tf5pvccDhyCM2w1Z7kCVxC1oOM8A0YzzS9Is326W69vUN9NkISIUCAH9CjLs54_rQlEqFghfLXtm1hi34z0yXfzyDQISJoXoxgZykn-FvfXLbN4mDV8kXA0oCm_PmQ5UVrwt_Xz9unSPR1wOrSwF6_SgLyi-Fpn-Thr5akCUs3R?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/jVMjSeaIuDUBsm6HMVEm8nZnOK_6LHlqL9h6sidaHUdppjiLVXsISXDaHIajdyLD6Juxeuhoe0rr7qHu6NU3eXqmrmpiI2mjx_B-u6F5XzIHaNy1ceoijKdL_eXK-SfKPctZOybO1VbEq3FeyvZtWKt2H-JIHZ0_bJwC-SG6SRrc4AuP83GmgW4R5lSzApKl?purpose=fullsize)

ESP-NOW:

- peer-to-peer
    
- ultra liviano
    
- baja latencia
    
- no necesita router
    
- ideal para proximidad
    

Además:

- permite obtener RSSI de paquetes recibidos
    
- menor consumo
    
- más robusto para este caso
    

---

# Arquitectura recomendada final

```text
ESP32-CAM A
   |
   |  ESP-NOW packets
   |
ESP32-CAM B

Ambos:
- envían heartbeat
- leen RSSI
- calculan distancia
- activan LED si > 2m
```

---

# Consideraciones reales importantes

## El RSSI NO es preciso

La distancia fluctúa bastante:

- cuerpo humano
    
- orientación
    
- paredes
    
- interferencia
    

Por eso conviene:

- usar promedio móvil
    
- aplicar filtro
    

Ejemplo:

```text
RSSI promedio = últimos 10 valores
```

---

# Mejora recomendada

## Histéresis

Evita que el LED parpadee.

Ejemplo:

```text
Encender LED  -> > 2.2 m
Apagar LED    -> < 1.8 m
```

---

# Arquitectura interna recomendada del firmware

```text
Task 1:
  enviar heartbeat

Task 2:
  leer RSSI

Task 3:
  calcular distancia

Task 4:
  controlar LED
```

Usando:

- FreeRTOS del ESP32
    
- tareas concurrentes
    

---

# Tecnologías recomendadas

- ESP-IDF o Arduino Framework
    
- ESP-NOW
    
- FreeRTOS
    
- RSSI Wi-Fi
    
- Filtro EMA o Moving Average
    

---

# Nivel de precisión esperado

|Entorno|Precisión aproximada|
|---|---|
|Abierto|±1 metro|
|Interior|±2 a 4 metros|
|Con obstáculos|muy variable|

Para:

- alarma de alejamiento
    
- proximidad
    
- anti pérdida
    

es completamente viable.