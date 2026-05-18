### Prueba Simple: ESP32-CAM Moto + ESP32-CAM "Tobillera" → LED4 ON al Perder WiFi

**Setup minimalista**: 
- **ESP32-CAM #1 (MOTO)**: Monitorea WiFi de la "tobillera". Si pierde señal → **enciende LED4 (GPIO4)**.
- **ESP32-CAM #2 (TOBILLERA)**: Solo emite WiFi como beacon.

**Prueba**: Aleja moto >100m → LED4 prende = OK. Luego integrás relé.

#### 1. **Hardware (ya lo tenés)**
```
MOTO ESP32-CAM:
- LED4 → GPIO4 (pin físico más fácil acceso)
- Nada más por ahora

TOBILLERA ESP32-CAM:
- Solo WiFi ON
```

#### 2. **Código TOBILLERA (Beacon WiFi - Copia y pega)**
```cpp
#include "WiFi.h"

const char* ssid = "TOBILLERA_MOTO_01";  // Nombre único
const char* password = "12345678";

void setup() {
  Serial.begin(115200);
  WiFi.softAP(ssid, password);  // Crea hotspot
  Serial.println("Tobillera WiFi: " + String(ssid));
  Serial.print("IP: ");
  Serial.println(WiFi.softAPIP());
}

void loop() {
  // Solo emite WiFi 24/7
  delay(1000);
}
```

#### 3. **Código MOTO (Detecta pérdida → LED4 ON)**
```cpp
#include "WiFi.h"

const char* ssid = "TOBILLERA_MOTO_01";
const char* password = "12345678";

#define LED_PIN 4  // GPIO4 = LED onboard ESP32-CAM

bool wasConnected = false;
unsigned long lastCheck = 0;

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);  // LED OFF inicial
  
  WiFi.begin(ssid, password);
  Serial.println("MOTO: Buscando tobillera...");
}

void loop() {
  if (millis() - lastCheck > 5000) {  // Chequeo cada 5s
    bool connected = (WiFi.status() == WL_CONNECTED);
    
    if (connected && !wasConnected) {
      Serial.println("✓ Conectado a tobillera");
      digitalWrite(LED_PIN, LOW);  // LED OFF = seguro
    }
    
    if (!connected && wasConnected) {
      Serial.println("✗ Tobillera perdida -> ALARMA!");
      digitalWrite(LED_PIN, HIGH);  // LED4 ON = FUERA RADIO
    }
    
    wasConnected = connected;
    lastCheck = millis();
  }
  
  delay(1000);
}
```

#### 4. **PASO A PASO Prueba (5 minutos)**
1. **Flashea**:
   - Tobillera: Sube código → WiFi "TOBILLERA_MOTO_01".
   - Moto: Sube código → busca esa red.

2. **Test cerca (0-50m)**:
   ```
   Serial Moto: "✓ Conectado"
   LED4: OFF (verde/seguro)
   ```

3. **Test lejos (100-200m)**:
   ```
   Serial Moto: "✗ Tobillera perdida -> ALARMA!"
   LED4: ON (rojo/peligro)
   ```

4. **Ver Serial** (Arduino IDE Tools > Serial Monitor 115200 baud).

#### 5. **Próximas Integraciones (una vez LED4 OK)**
```
LED4 ON → 
  1. Relé GPIO2 (corta ignición)
  2. Cámara foto (guarda SD)
  3. LoRa backup
  4. Buzzer alarma
```

**¡Funciona 100% sin internet!** Radio efectivo: **50-300m** (depende antena/obstáculos).

**Resultado esperado**:
```
[TOBILLERA cerca] → LED4 OFF
[TOBILLERA lejos] → LED4 ON (prueba OK)
```

Subí **pantalla Serial** cuando pruebes → confirmo y agrego relé. ¿Nombres WiFi listos? 🚀