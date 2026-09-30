## 1. `Global C-State Control → Auto`

Controla los **estados de reposo de la CPU**.

Cuando el Ryzen no tiene trabajo, puede poner los núcleos y otras partes del procesador en estados de bajo consumo.

```text
C0 → trabajando
C1 → reposo ligero
C6 → reposo profundo
```

Con:

```text
Global C-State Control → Auto
```

la BIOS/firmware decide qué estados de reposo utilizar.

### ¿Qué consigue?

- Menor consumo en idle.
- Menor temperatura cuando está sin carga.
- Menor consumo eléctrico.
- Permite que el Ryzen entre y salga de estados de bajo consumo.

### ¿Por qué puede causar problemas?

Una BIOS/firmware antigua puede tener problemas con determinadas transiciones de:

```text
CPU funcionando
       ↓
CPU idle
       ↓
C-State profundo
       ↓
CPU vuelve a trabajar
```

Si tu mini-PC se reinicia **principalmente cuando está sin hacer nada**, esta configuración se vuelve sospechosa.

Por eso existe la prueba:

```text
Global C-State Control → Disabled
```

Pero **no lo pondría así todavía**. Primero `Auto`.

---

# 2. `CPPC → Auto`

CPPC significa **Collaborative Processor Performance Control**.

Es el mecanismo mediante el cual el sistema operativo y el firmware del Ryzen colaboran para decidir **a qué nivel de rendimiento debe funcionar el procesador**.

En lugar de decir simplemente:

> "CPU a 2 GHz"

el sistema puede solicitar algo parecido a:

> "Necesito aproximadamente este nivel de rendimiento."

El Ryzen decide internamente cómo conseguirlo.

### Ejemplo

Si estás escribiendo en el teclado:

```text
poca carga
↓
CPPC solicita poco rendimiento
↓
CPU reduce consumo
```

Si empiezas a comprimir un archivo:

```text
mucha carga
↓
CPPC solicita más rendimiento
↓
Ryzen aumenta frecuencia/voltaje
```

### ¿Qué interesa?

Normalmente:

```text
CPPC → Auto / Enabled
```

Porque es parte normal de la administración de energía de Ryzen.

---

# 3. `CPPC Preferred Cores → Auto`

Esto es todavía más interesante.

Los núcleos de un Ryzen **no necesariamente son idénticos en comportamiento**. Algunos pueden alcanzar mejores frecuencias/eficiencia que otros.

CPPC Preferred Cores permite que el firmware y el sistema operativo sepan cuáles son los núcleos preferidos.

Por ejemplo, conceptualmente:

```text
Core 0  ★★★★★
Core 1  ★★★★
Core 2  ★★★★
Core 3  ★★★
```

Cuando existe una tarea ligera que necesita mucho rendimiento, el sistema puede preferir uno de los mejores núcleos.

### ¿Qué ganas?

- Mejor rendimiento en cargas ligeras.
- Mejor comportamiento del boost.
- Mejor distribución de tareas.
- Puede ayudar a la eficiencia.

Normalmente:

```text
CPPC Preferred Cores → Auto / Enabled
```

No es una configuración que yo tocaría buscando solucionar directamente la NVMe caliente.

---

# 4. `PBO → Auto`

PBO significa **Precision Boost Overdrive**.

Es una de las opciones más importantes para la temperatura del Ryzen.

El Ryzen normalmente tiene límites de potencia y corriente.

PBO puede permitir que el procesador utilice más margen cuando existe capacidad térmica y eléctrica.

Conceptualmente:

```text
Configuración normal

CPU
 ↓
límite de potencia
 ↓
temperatura controlada
```

Con PBO más agresivo:

```text
CPU
 ↓
más potencia disponible
 ↓
más frecuencia
 ↓
más voltaje
 ↓
más temperatura
```

### En una mini-PC

Esto es importante porque una mini-PC tiene mucha menos capacidad de refrigeración que un PC de escritorio.

Si alguien dejó:

```text
PBO → Advanced
PPT → muy alto
TDC → muy alto
EDC → muy alto
```

podrías tener un Ryzen consumiendo más de lo que la refrigeración o fuente fueron diseñadas para manejar.

Por eso para diagnóstico me gusta:

```text
PBO → Auto
```

o incluso:

```text
PBO → Disabled
```

### Importante

PBO afecta principalmente al **CPU**, no explica por sí solo que una NVMe se caliente estando prácticamente inactiva.

---

# 5. `EXPO/XMP → Disabled`

Esto es diferente: afecta a la **RAM**.

EXPO/XMP permite utilizar perfiles de memoria con frecuencias/timings más agresivos que los valores JEDEC estándar.

Por ejemplo:

```text
JEDEC
DDR5-4800
```

frente a:

```text
EXPO
DDR5-6000
timings más agresivos
```

El segundo puede ofrecer más rendimiento, pero exige más al controlador de memoria.

### ¿Por qué desactivarlo para diagnosticar?

Porque una RAM marginalmente inestable puede producir:

```text
pantallazo
freeze
reinicio
corrupción
Linux que no termina de arrancar
Windows que se reinicia
```

Y puede parecer un problema de CPU.

Por eso:

```text
EXPO/XMP → Disabled
```

es una configuración conservadora para las pruebas.

### En tu caso es particularmente interesante

Porque dices que:

> Windows 10 y Linux reinician muchísimo más que Windows 11.

Una memoria/configuración marginal puede comportarse de forma diferente según el sistema operativo, kernel, drivers y patrón de uso.

---

# 6. `ASPM → Auto`

Esto tiene relación directa con **PCIe**, y por tanto puede ser bastante relevante para tu NVMe.

ASPM significa:

**Active State Power Management.**

Permite que el enlace PCIe reduzca su consumo cuando no está transmitiendo datos.

Conceptualmente:

```text
NVMe
  │
  │ PCIe
  │
Ryzen/Chipset
```

Cuando no hay actividad:

```text
PCIe activo
   ↓
ASPM
   ↓
estado de menor consumo
```

Cuando vuelve una operación:

```text
estado ahorro
   ↓
PCIe activo
   ↓
NVMe trabaja
```

### ¿Por qué nos interesa?

Porque tú tienes:

**NVMe caliente con poca actividad.**

Si existe un problema de firmware/PCIe/ASPM, el enlace podría no estar entrando correctamente en estados de bajo consumo.

Por eso:

```text
ASPM → Auto
```

es una configuración razonable.

Y posteriormente podemos hacer una prueba:

```text
ASPM → Disabled
```

para comprobar si cambia el comportamiento.

Pero hay una sutileza:

**ASPM Disabled no significa necesariamente que la NVMe vaya a estar más fría.**

Puede ocurrir exactamente lo contrario porque el enlace permanece activo.

---

# 7. `M.2 Link Speed → Auto`

Esta determina la **generación/velocidad del enlace PCIe utilizado por la NVMe**.

Por ejemplo:

```text
Gen3
≈ 8 GT/s por lane

Gen4
≈ 16 GT/s por lane
```

Una NVMe PCIe 4.0 puede funcionar a Gen4 si toda la plataforma lo permite.

### Auto

```text
M.2 Link Speed → Auto
```

significa que BIOS intenta negociar automáticamente la velocidad compatible.

### ¿Por qué quiero probar Gen3 en tu caso?

Porque es una excelente prueba diagnóstica.

Si tienes:

```text
Ryzen H
    ↓
PCIe
    ↓
NVMe Gen4
```

podemos forzar temporalmente:

```text
M.2 Link Speed → Gen3
```

y observar:

- temperatura NVMe;
- estabilidad;
- reinicios;
- comportamiento de Windows/Linux.

Si con Gen3:

**baja mucho la temperatura y desaparecen los reinicios**, tenemos una pista muy importante relacionada con **PCIe/NVMe/firmware**.

No significa automáticamente que la NVMe esté dañada.

---

# 8. `Power Supply Idle Control → Typical Current Idle`

Esta es probablemente la que **más me interesa probar en tu caso**, si realmente aparece en esa BIOS.

Es una configuración relacionada con cómo AMD maneja el comportamiento de **consumo muy bajo del procesador cuando está en idle**.

Hay una diferencia entre:

```text
Low Current Idle
```

y:

```text
Typical Current Idle
```

Simplificando:

### Low Current Idle

Permite que el sistema llegue a estados de consumo muy bajo.

```text
CPU idle
 ↓
consumo muy bajo
 ↓
estado profundo
```

Una fuente o plataforma que no se comporta correctamente en ese régimen puede presentar:

```text
freeze
reinicio
apagado
WHEA
problemas al salir de idle
```

### Typical Current Idle

Mantiene un comportamiento de corriente en idle más compatible con determinadas fuentes/plataformas.

```text
CPU idle
 ↓
consumo mínimo más "normal"
 ↓
menos exigencia a la fuente durante estados extremos de idle
```

Puede consumir **algo más en reposo**, pero puede mejorar la estabilidad de ciertas combinaciones de Ryzen + BIOS + fuente.

---

# En tu caso, las separaría así

|Configuración|Principalmente afecta|Importancia en tu caso|
|---|---|---|
|**Global C-State**|Reposo CPU|🔴 Alta|
|**CPPC**|Rendimiento/energía CPU|🟡 Media|
|**CPPC Preferred Cores**|Selección de núcleos|🟢 Baja|
|**PBO**|Potencia/boost CPU|🔴 Alta|
|**EXPO/XMP**|RAM|🔴 Alta para diagnosticar reinicios|
|**ASPM**|Ahorro PCIe|🔴 Alta por la NVMe|
|**M.2 Link Speed**|PCIe/NVMe|🔴 Muy alta|
|**Power Supply Idle Control**|Idle/energía Ryzen|🔴 Muy alta|

### Y haría esta configuración inicial

```text
Global C-State Control       → Auto
CPPC                         → Auto
CPPC Preferred Cores         → Auto
PBO                          → Auto
EXPO/XMP                     → Disabled
ASPM                         → Auto
M.2 Link Speed               → Gen3       ← SOLO PARA PRUEBA
Power Supply Idle Control    → Typical Current Idle
```

**Pero no cambies todo simultáneamente si queremos diagnosticar con precisión.**

Por tus síntomas, yo empezaría por:

```text
1. Load Optimized Defaults
2. EXPO/XMP → Disabled
3. PBO → Auto
4. Power Supply Idle Control → Typical Current Idle
5. M.2 → Gen3
6. C-State → Auto
```

Y después probaría **Windows 11 y, sobre todo, quedarse dentro de la BIOS** y conectar/desconectar USB/audio.

Si aun **dentro de BIOS** al insertar el jack o conectar el USB la máquina se apaga/reinicia, entonces ya no perdería mucho tiempo ajustando CPPC/C-States: **hay que investigar la alimentación de 19 V/65 W, el circuito USB/audio, la placa y el NVMe/PCIe.**