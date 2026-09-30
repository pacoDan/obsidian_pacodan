Sí: **ese dato es muy importante** y cambia bastante la hipótesis.

Si con Windows 10 o Linux se reinicia a los **2–3 segundos**, mientras que con Windows 11 puede funcionar bastante tiempo, yo sospecharía primero de **firmware/BIOS + gestión de energía/PCIe**, y después de hardware. No asumiría que Windows 11 "arregló" el problema.

El hecho de que además **conectar/desconectar USB o jack de audio pueda provocar un reinicio** hace que los C-States sean solo una parte de la investigación.

### Lo que probaría, en este orden

**1. BIOS → cargar valores predeterminados**

Haz `Load Optimized Defaults` / `Load Setup Defaults`.

Después, sin modificar voltajes ni frecuencias, revisa:

- `Global C-State Control` → **Auto**
- `CPPC` → **Auto/Enabled**
- `CPPC Preferred Cores` → **Auto/Enabled**
- `PBO` → **Auto/Disabled**
- `EXPO/XMP` → **Disabled**
- `PCIe ASPM` → **Auto**
- `M.2/PCIe Link Speed` → **Gen3** para probar
- `Power Supply Idle Control` → **Typical Current Idle**, si aparece.

**2. Haz una prueba exclusivamente en BIOS**

Esto es fundamental.

Arranca y **no dejes que cargue ningún sistema operativo**. Quédate dentro de la BIOS durante 10–15 minutos.

Luego prueba:

- conectar USB;
- desconectar USB;
- conectar/desconectar el jack de audio.

Si **también se reinicia dentro de la BIOS**, Windows 10/11/Linux dejan de ser el sospechoso principal.

Ahí pensaría seriamente en:

**fuente → placa/VRM → USB/audio → firmware BIOS → NVMe/PCIe.**

### 3. La diferencia Windows 10/Linux vs Windows 11 es una pista

Puede haber una diferencia en cómo los sistemas manejan:

- estados de energía ACPI;
- PCIe ASPM;
- NVMe APST;
- USB power management;
- CPU idle;
- CPPC;
- suspensión/idle del Ryzen.

Es posible que Windows 11 esté evitando el estado de bajo consumo que provoca el fallo, mientras que Windows 10/Linux sí lo utilizan.

Por eso **que Windows 11 aguante no demuestra que el hardware esté sano**.

De hecho, si la máquina falla casi inmediatamente con Linux, sería interesante hacer una prueba con un Linux que permita arrancar con parámetros de kernel relacionados con ACPI/PCIe/NVMe, pero **no empezaría por ahí**: primero aislaría BIOS y hardware.

### 4. La NVMe que se calienta estando casi sin uso es particularmente sospechosa

Si la NVMe está, por ejemplo, a una temperatura alta **sin actividad significativa**, quiero saber:

- temperatura en BIOS;
- temperatura cuando Windows 11 está recién arrancado;
- temperatura después de 5–10 minutos sin hacer nada;
- modelo exacto de NVMe.

Si puedes, dime **qué temperatura marca**.

Una NVMe Gen4 en una mini-PC puede calentarse bastante, pero si está caliente incluso en reposo, quiero comprobar si está manteniendo activo el enlace PCIe o si hay un problema con su gestión de energía.

### 5. Hay una prueba muy buena: arrancar sin la NVMe

Si puedes hacerlo de forma segura:

**retira temporalmente la NVMe** y entra solamente a BIOS.

No hace falta instalar nada.

Después prueba repetidamente:

- USB;
- audio;
- esperar varios minutos.

Si **sin NVMe deja de reiniciarse**, tenemos una pista enorme hacia:

**NVMe ↔ PCIe ↔ alimentación/firmware.**

Si **sin NVMe sigue apagándose al insertar el jack o USB**, entonces miraría mucho más hacia **fuente/placa/USB/audio**.

---

Y hay un detalle que me hace ser bastante prudente: **que al insertar un jack de audio se comporte como si hubieras pulsado el botón de encendido no es un comportamiento normal de C-State**.

Eso me hace querer comprobar primero si existe un problema eléctrico o de alimentación antes de tocar parámetros avanzados de Ryzen.

Si me pasas **modelo exacto de la mini-PC, Ryzen H exacto, modelo de la NVMe, potencia de la fuente (por ejemplo 19 V × 6,32 A) y temperatura aproximada de la NVMe**, puedo decirte qué sospecharía primero y qué pruebas haría sin arriesgar la placa.


---
---

### Ruta que probaría primero

Entra a BIOS con `Del` o `F2`.

Para restaurar primero:

```text
Save & Exit
 └── Load Optimized Defaults
     └── Yes
```

o puede aparecer como:

```text
Save & Exit
 └── Load Setup Defaults
     └── Yes
```

También puede existir directamente:

```text
Exit
 └── Load Optimized Defaults
```

Después **guarda y vuelve a entrar en BIOS** antes de modificar cualquier otra cosa.

---

## 1. C-States

Busca:

```text
Advanced
 └── AMD CBS
      └── CPU Common Options
           └── Global C-State Control
```

Puede aparecer como:

```text
Advanced
 └── AMD CBS
      └── CPU Common Options
           └── Global C-State Control
```

Valores:

```text
Global C-State Control → Auto
```

o:

```text
Global C-State Control → Enabled
```

**No lo pondría en Disabled inicialmente.**

---

## 2. CPPC

Una ruta habitual:

```text
Advanced
 └── AMD CBS
      └── CPU Common Options
           ├── CPPC
           └── CPPC Preferred Cores
```

Configura:

```text
CPPC                → Auto / Enabled
CPPC Preferred Cores → Auto / Enabled
```

En algunas BIOS aparece dentro de:

```text
Advanced
 └── AMD CBS
      └── NBIO Common Options
```

Así que si no está en `CPU Common Options`, revisa también `NBIO`.

---

## 3. PBO

Puede estar en:

```text
Advanced
 └── AMD CBS
      └── SMU Common Options
           └── Precision Boost Overdrive
```

o:

```text
Advanced
 └── AMD Overclocking
      └── Precision Boost Overdrive
```

En una BIOS de mini-PC yo empezaría con:

```text
Precision Boost Overdrive → Auto
```

o:

```text
Precision Boost Overdrive → Disabled
```

Si encuentras:

```text
PBO Limits
PPT
TDC
EDC
Scalar
Curve Optimizer
CPU Voltage
SoC Voltage
```

**no los toques todavía.**

---

## 4. Power Supply Idle Control

Esta opción es particularmente interesante por tus reinicios.

Normalmente está dentro de:

```text
Advanced
 └── AMD CBS
      └── CPU Common Options
           └── Power Supply Idle Control
```

Si aparece:

```text
Power Supply Idle Control
```

para la prueba pondría:

```text
Typical Current Idle
```

Esto es **especialmente interesante en tu caso**, porque tienes reinicios que parecen relacionados con transiciones de alimentación/idle.

---

## 5. PCIe ASPM

Puede estar en una sección completamente diferente:

```text
Advanced
 └── PCI Subsystem Settings
      └── PCI Express Settings
           └── ASPM
```

o:

```text
Advanced
 └── PCIe Configuration
      └── PCI Express Configuration
           └── ASPM
```

Busca nombres como:

```text
PCI Express ASPM
ASPM Support
Native ASPM
PCIe ASPM
```

Primera prueba:

```text
ASPM → Auto
```

Si posteriormente queremos hacer una prueba específica de estabilidad, podemos probar:

```text
ASPM → Disabled
```

pero **no cambiaría esto todavía junto con diez parámetros más**.

---

## 6. M.2 / velocidad PCIe de la NVMe

Aquí puede ser más difícil encontrarlo.

Prueba:

```text
Advanced
 └── PCIe Configuration
      └── M.2 Configuration
```

o:

```text
Advanced
 └── AMD PBS
      └── PCIe Configuration
```

o:

```text
Advanced
 └── AMD CBS
      └── NBIO Common Options
           └── PCIe Configuration
```

Busca:

```text
M.2 Link Speed
PCIe Link Speed
PCIe Speed
PCIe x4 Link Speed
M.2 PCIe Speed
```

Si encuentras algo como:

```text
Auto
Gen1
Gen2
Gen3
Gen4
```

para nuestra prueba:

```text
M.2 / PCIe Link Speed → Gen3
```

Esto es **una prueba diagnóstica**, no necesariamente la configuración final.

---

## 7. EXPO/XMP

En una mini-PC puede ni siquiera aparecer.

Busca:

```text
Advanced
 └── AMD CBS
      └── UMC Common Options
```

o:

```text
Advanced
 └── DRAM Configuration
```

o simplemente:

```text
Advanced
 └── Memory Configuration
```

Busca:

```text
EXPO
XMP
DOCP
Memory Profile
DRAM Profile
```

Para diagnóstico:

```text
EXPO/XMP/DOCP → Disabled
```

y:

```text
Memory Frequency → Auto
```

---

# Pero yo haría algo diferente en TU máquina

No modificaría todo lo anterior de golpe.

Haría esta secuencia:

### Prueba 1 — BIOS limpia

```text
Save & Exit
 └── Load Optimized Defaults
```

Después:

```text
Global C-State Control       → Auto
CPPC                         → Auto
CPPC Preferred Cores         → Auto
PBO                          → Auto
EXPO/XMP                     → Disabled
ASPM                         → Auto
M.2 Link Speed               → Auto
Power Supply Idle Control    → Typical Current Idle
```

Guarda:

```text
F10 → Save Changes and Exit
```

### Prueba 2 — quedarse en BIOS

Después vuelve a entrar a BIOS y **no arranques Windows**.

Déjala 10–15 minutos.

Luego prueba:

```text
USB conectado → desconectar
USB desconectado → conectar
Audio → conectar jack
Audio → desconectar jack
```

Esto nos va a decir muchísimo.

---

### Si se reinicia incluso dentro de BIOS

Entonces **no seguiría buscando configuraciones de Windows 11/10/Linux**.

La investigación pasa a:

```text
Fuente 19 V / 65 W
       ↓
entrada DC de la placa
       ↓
VRM / regulación
       ↓
USB / audio / chipset
       ↓
PCIe
       ↓
NVMe
```

Y ahí la temperatura anormal de la NVMe adquiere todavía más importancia.

**Si puedes, mándame una foto de la pantalla principal de tu BIOS y de la pestaña `Advanced`.** Con una foto puedo decirte **"entra aquí → aquí → aquí"** según los menús que realmente tenga tu AMI, en vez de darte rutas genéricas que pueden no existir en esa mini-PC.