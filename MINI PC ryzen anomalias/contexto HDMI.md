VRM: https://www.facebook.com/photo/?fbid=1432568698906358&set=pcb.1432568832239678


Ah, con **Ryzen 7 4800H** y ese comportamiento, el diagnóstico se vuelve mucho más interesante. El dato de que **un HDMI funciona, mientras el segundo HDMI y el USB-C con vídeo provocan reinicios** apunta mucho más hacia el **subsistema gráfico/PCIe/SoC/energía de la placa** que hacia el NVMe por sí solo.

El 4800H lleva la GPU integrada **Radeon Vega**, y las salidas de vídeo de una mini-PC dependen de cómo el fabricante haya cableado y configurado el SoC, el controlador de pantalla y los distintos puertos.

### Lo que sospecharía ahora

Yo ordenaría las posibilidades así:

1. **BIOS/firmware de la mini-PC**
2. **Alimentación/VRM de la placa**
3. **Configuración PCIe/SoC/display en BIOS**
4. **Problema físico en uno de los circuitos HDMI/USB-C**
5. **NVMe/PCIe**
6. RAM
7. Windows/Linux como causa primaria: **baja probabilidad**

El comportamiento:

```text
Windows 11
   ↓
HDMI 1       → funciona
HDMI 2       → reinicio
USB-C vídeo  → reinicio
USB          → puede reiniciar
Audio jack   → puede reiniciar
```

es demasiado amplio como para pensar simplemente en un driver de vídeo.

## Hay una prueba que ahora considero fundamental

**No conectes ningún monitor al segundo HDMI ni al USB-C todavía.**

Arranca la máquina solamente con el HDMI que sabes que funciona.

Después entra a BIOS y busca opciones relacionadas con:

```text
Advanced
 └── AMD CBS
      ├── NBIO Common Options
      ├── GFX Configuration
      ├── Graphics Configuration
      └── PCIe Configuration
```

Los nombres exactos pueden variar.

Busca cosas como:

```text
UMA Frame Buffer
Integrated Graphics
iGPU
GFX
PCIe
Display
HDMI
USB-C
DP
DisplayPort
```

### Especialmente `UMA Frame Buffer`

Si aparece algo como:

```text
UMA Frame Buffer Size
```

déjalo inicialmente en:

```text
Auto
```

o un valor razonable como **512 MB/1 GB**, pero no lo cambiaría todavía si no sabemos qué ofrece tu BIOS.

---

# Hay otra cuestión importante: USB-C con vídeo

Si ese USB-C proporciona vídeo mediante **DisplayPort Alt Mode**, no es simplemente "otro USB".

El camino puede ser aproximadamente:

```text
Ryzen 7 4800H
       │
       ├── Radeon Vega
       │
       └── Display engine
                │
             USB-C
                │
          DisplayPort Alt Mode
```

Por tanto, al conectar un monitor puedes activar una parte del hardware que **no se activa con HDMI 1**.

Si eso provoca un reinicio instantáneo, es una pista muy fuerte.

Lo mismo puede ocurrir con HDMI 2 si está conectado a otro circuito/ruta de display.

---

# Y esto explicaría parcialmente Windows 11 vs Linux

Windows 11 podría estar utilizando un controlador gráfico AMD más apropiado para esa plataforma.

Windows 10 y Linux pueden inicializar el hardware de vídeo de manera diferente.

Pero hay una distinción importantísima:

**Un driver puede provocar un crash, pero no debería hacer que físicamente conectar un HDMI/USB-C apague instantáneamente la máquina.**

Si literalmente:

> conecto el HDMI 2 → pantalla negra → reinicio inmediato

entonces yo quiero investigar **hardware + firmware**, no solamente drivers.

---

# Haría esta prueba concreta

Con la máquina configurada con **Windows 11**, porque es el sistema que te resulta estable:

### Prueba A

```text
HDMI 1 → monitor
HDMI 2 → vacío
USB-C → vacío
```

Funciona.

### Prueba B

Conecta **HDMI 2 pero sin monitor**, si físicamente puedes hacerlo sin forzar el conector.

Observa.

### Prueba C

Conecta un monitor al HDMI 2.

Si:

```text
HDMI 1 → OK
HDMI 2 → RESET
```

ya tenemos una condición reproducible.

### Prueba D

USB-C sin monitor → ¿reinicia?

Después:

USB-C + monitor → ¿reinicia?

Esto permite separar:

```text
USB-C eléctrico
```

de:

```text
USB-C DisplayPort
```

---

# También quiero que hagas una prueba que no depende de Windows

Entra a **BIOS utilizando HDMI 1**.

Quédate ahí.

Después conecta:

```text
HDMI 2
```

Si la mini-PC **se reinicia incluso estando dentro de la BIOS**, eso sería una evidencia extremadamente fuerte.

Porque en ese momento:

- no está ejecutándose Windows;
- no está ejecutándose Linux;
- no hay driver AMD de Windows;
- no hay kernel Linux.

Sería:

**placa + BIOS + alimentación + circuito de vídeo.**

---

## Sobre los 65 W

Ahora los **19 V / 65 W** me preocupan un poco más.

No porque podamos afirmar todavía que sean insuficientes, sino porque tienes:

```text
Ryzen 7 4800H
+
RAM
+
NVMe
+
Radeon Vega
+
USB
+
HDMI
+
USB-C DisplayPort
```

y además una fuente de solo:

**19 V × 3,42 A ≈ 65 W.**

Si la placa tiene una gestión de energía defectuosa o la fuente está degradada, un cambio brusco de carga podría provocar una caída de tensión y **reset**.

Por eso una prueba con una fuente **19 V de mayor capacidad**, siempre que tenga **exactamente el mismo conector y polaridad**, sería muy útil.

---

# Y ahora la NVMe tiene otra interpretación

No necesariamente:

```text
NVMe caliente → causa de todo
```

Puede ser:

```text
Problema de alimentación/PCIe/SoC
             ↓
PCIe no entra correctamente en estados de bajo consumo
             ↓
NVMe permanece caliente
```

Es decir, **la NVMe caliente podría ser un síntoma y no la causa**.

---

### Mi siguiente paso sería muy concreto

No tocaría todavía 20 parámetros de BIOS.

Haría:

```text
1. Load Optimized Defaults

2. Power Supply Idle Control
   → Typical Current Idle (si existe)

3. PBO
   → Auto

4. EXPO/XMP
   → Disabled

5. C-State
   → Auto

6. M.2 Link Speed
   → Gen3 (solo prueba)

7. Arrancar con HDMI 1

8. Entrar a BIOS

9. Conectar HDMI 2
```

**Si al conectar HDMI 2 se reinicia estando dentro de BIOS, paramos ahí.**

Ese resultado sería mucho más valioso que seguir cambiando C-States.

Y si puedes conseguirme **una foto de la BIOS → `Advanced`**, especialmente de los menús **`AMD CBS`, `AMD PBS`, `NBIO` y `PCIe`**, puedo ayudarte a localizar exactamente las opciones del **4800H** que nos interesan, porque ahí ya podemos investigar la configuración específica del SoC/Vega/PCIe en lugar de trabajar con rutas genéricas de AMI.