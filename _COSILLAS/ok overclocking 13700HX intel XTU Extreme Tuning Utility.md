## 🚀 Desbloqueando la Potencia Turbo (Límites de Watts)

Busca la sección de Power Limits y ajusta los valores de la siguiente manera para que el i7-13700HX pueda respirar libremente:

1. Turbo Boost Power Max (PL1): Este es el límite de potencia sostenida. De fábrica suele venir limitado entre 55W y 75W en laptops. Súbelo de forma amena a 95W o 115W. Con tus enfriadores, el chip disipará este consumo sin problemas.
2. Turbo Boost Short Power Max (PL2): Este es el límite de potencia para ráfagas cortas (frecuencias turbo máximas). Suele venir en unos 115W-135W. Puedes elevarlo a 150W o 175W. Al tener disipación extra en los VRMs, las fases de poder de tu placa soportarán este pico de corriente perfectamente.
3. Turbo Boost Power Time Window (Tau): Es el tiempo en segundos que el procesador tiene permitido quedarse en el estado de máxima potencia (PL2) antes de bajar al PL1. Desliza esta barra al máximo permitido (usualmente entre 96 y 128 segundos) para prolongar el rendimiento turbo.

---

## ¿Por qué sigue siendo clave el Core Voltage Offset aquí?

Ahora que vas a subir la potencia turbo en Watts, el Core Voltage Offset juega un papel crucial:

- Si dejas el Offset en 0V, el procesador gastará esos Watts extra de forma ineficiente (inyectando demasiado voltaje para frecuencias medias).
- Si aplicas un Offset Negativo (Undervolt) leve de `-0.060V` a `-0.080V`, obligas al procesador a optimizar cada Watt. Al necesitar menos energía por cada ciclo, el procesador alcanzará frecuencias turbo mucho más altas (por ejemplo, verás los núcleos P trepar a 4.8 GHz o 5.0 GHz reales) antes de chocar contra el nuevo límite de Watts que acabas de configurar.

---

## ⚠️ El último candado (La BIOS de la Laptop)

Si aplicas estos cambios en los Watts dentro de XTU y al darle a Apply notas que los valores se regresan solos a su estado original o siguen sin verse reflejados en la frecuencia, la laptop tiene un bloqueo llamado EDP/Current Limit o Power Limit Lock en su firmware.

Para romperlo del todo, es necesario habilitar el "Modo Rendimiento Extremo" o "Modo Custom/Manual" desde el software propietario de tu laptop (como _Lenovo Vantage, ASUS Armoury Crate/G-Helper, HP Omen Gaming Hub_, etc.) o activando el overclocking directamente en su BIOS.

Para darte el camino exacto para saltarte este bloqueo de energía de firmware, ¿me podrías confirmar la marca y modelo exacto de tu laptop? (Por ejemplo: _Lenovo Legion, ASUS ROG, HP Omen, Acer Predator_, etc.).