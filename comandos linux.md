
### 2. `nvim --remote-ui` + SSH

Neovim tiene soporte para separar el proceso de Neovim de la interfaz.

En el servidor:

```bash
nvim --headless --listen localhost:6666
```

Y desde tu máquina puedes conectar una UI de Neovim.

Pero hay una complicación: el socket debe ser accesible a través de SSH. Puedes hacer un túnel:

```bash
ssh -L 6666:localhost:6666 usuario@servidor
```

y después, localmente:

```bash
nvim --remote-ui --server localhost:6666
```

La arquitectura queda:

```text
Nvim UI local
     │
     │ localhost:6666
     ▼
SSH tunnel
     │
     ▼
Nvim remoto
     │
     ├── proyecto
     ├── LSP
     ├── Git
     └── herramientas
```

**Esto es conceptualmente mucho más cercano a VS Code Remote-SSH que `scp://`.**

---

### 3. Una alternativa todavía más sencilla: `ssh` + `nvim`

No descartaría esto.

```bash
ssh usuario@servidor
cd ~/proyecto
nvim .
```

Si utilizas una terminal local buena (Kitty, WezTerm, Alacritty, etc.), la experiencia puede ser excelente.

Además, todo está donde debe estar:

```text
Servidor
│
├── ~/proyecto
├── .git
├── node_modules
├── .venv
├── LSP
├── Docker
└── herramientas
```

No hay sincronización.

No hay montaje.

No hay copias temporales.

No hay problemas de `node_modules`, permisos, symlinks, watchers, etc.

---

### 4. `sshfs` es bueno, pero no es equivalente a Remote-SSH

Con:

```bash
sshfs usuario@servidor:/home/usuario/proyecto ~/proyecto
```

puedes hacer:

```bash
nvim ~/proyecto
```

y parece local.

Pero realmente tienes:

```text
Nvim local
    │
    ▼
filesystem FUSE
    │
    ▼
SSH
    │
    ▼
filesystem remoto
```

Para editar archivos funciona muy bien.

El problema aparece con proyectos grandes:

- LSP local vs remoto
    
- `node_modules`
    
- `.git`
    
- watchers
    
- compiladores
    
- `rg`
    
- Python virtualenv
    
- permisos
    
- latencia
    
- millones de operaciones pequeñas
    

Por eso **no es mi primera elección para proyectos remotos grandes**.

---

## Lo que yo elegiría

Si tu objetivo es:

> "Quiero tener mi Neovim/GUI en mi PC, pero que el proyecto realmente viva y se ejecute en el servidor."

Entonces buscaría una solución basada en:

```text
             TU PC
        ┌─────────────┐
        │ Neovim UI   │
        └──────┬──────┘
               │
              SSH
               │
               ▼
        ┌─────────────┐
        │    nvim     │
        │             │
        │  proyecto   │
        │  LSP        │
        │  git        │
        │  tools      │
        └─────────────┘
            SERVIDOR
```

Y **no**:

```text
Nvim local
   ↓
scp
   ↓
archivo remoto
```

porque eso está pensado principalmente para acceso remoto a archivos, no para convertir un proyecto remoto en un workspace completo.

Si quieres, puedo mostrarte **3 configuraciones concretas para Neovim**:

1. **Neovim local + proceso Nvim remoto + SSH tunnel** (la más parecida a VS Code Remote-SSH).
    
2. **Neovim local + SSHFS** (la más fácil).
    
3. **Neovim remoto + GUI local** (la más potente/limpia).
    

Y puedo darte los comandos y configuración de `init.lua`/plugins para que puedas hacer algo tipo:

```bash
nvim-remote usuario@servidor:/home/usuario/proyecto
```

y que se abra **como un proyecto local**, pero ejecutándose realmente en el servidor.



----




**1. Archivo puntual**

```bash
vim scp://usuario@servidor/proyecto/src/main.py
```

**2. Navegar por el proyecto remoto con netrw**

```bash
vim scp://usuario@servidor/proyecto/
```

Luego puedes entrar en directorios y abrir archivos.

**3. Montar el proyecto remoto con SSHFS — recomendado para Vim local**

```bash
sshfs usuario@servidor:/ruta/proyecto ~/proyecto-remoto
cd ~/proyecto-remoto
vim .
```

Aquí Vim ve:

```text
~/proyecto-remoto/
├── src/
├── tests/
├── package.json
├── README.md
└── ...
```

como si fuera un proyecto local.


* Con SSHFS, puedes abrir otra terminal SSH para ejecutar comandos en el servidor.

**Si tu objetivo es exactamente "quiero abrir Vim en mi PC y que `vim /proyecto` sea realmente un proyecto que está en el servidor", SSHFS es una solución mucho más apropiada que `scp://`.**






----



### 📁 Archivos y directorios

**`rsync` — sincronizar carpetas**

```bash
rsync -av images/ images2/
```

Copia el contenido de `images` a `images2`. Si volvés a ejecutarlo, solo transferirá lo que haya cambiado.


**`du` — saber cuánto ocupa**

```bash
du -sh Documentos/
```

Muestra el tamaño total de `Documentos`.

```bash
du -sh Documentos/*
```

Muestra cuánto ocupa cada elemento dentro de `Documentos`.

**`stat` — información detallada**

```bash
stat notas.txt
```

Muestra permisos, propietario, tamaño y fechas del archivo.

---

### 🔎 Buscar archivos

**`find` — búsqueda avanzada**

```bash
find . -name "*.txt"
```

Busca todos los archivos `.txt` desde el directorio actual.

```bash
find . -mtime +5
```

Busca archivos modificados hace más de 5 días.

⚠️ Con `-delete` hay que tener cuidado:

```bash
find . -name "*.tmp" -delete
```

Borra los `.tmp` encontrados.

**`locate` — búsqueda rápida**

```bash
locate passwd
```

Busca rutas que contengan `passwd` utilizando su base de datos.

**`updatedb` — actualizar esa base**

```bash
sudo updatedb
```

Actualiza la base que utiliza `locate`.

---

Muestra todo el archivo.

**`tac`**

```bash
tac frutas.txt
```

Muestra las líneas en orden inverso.

Si `frutas.txt` contiene:

```text
Manzana
Banana
Pera
```

`cat` muestra:

```text
Manzana
Banana
Pera
```

Mientras que `tac` muestra:

```text
Pera
Banana
Manzana
```

**`head`**

```bash
head -n 5 archivo.log
```

Muestra las primeras 5 líneas.

**`tail`**

```bash
tail -n 20 archivo.log
```

Muestra las últimas 20 líneas.

Para observar un log en vivo:

```bash
tail -f /var/log/auth.log
```

**`less`**

```bash
less archivo.log
```

Permite recorrer un archivo grande sin imprimirlo entero de golpe. Salís con `q`.

**`wc`**

```bash
wc -l archivo.txt
```

Cuenta líneas.

```bash
wc -w archivo.txt
```

Cuenta palabras.

```bash
wc -c archivo.txt
```

Cuenta bytes.

---

### ✂️ Filtrar y procesar texto

**`grep`**

```bash
grep "error" servidor.log
```

Muestra las líneas que contienen `error`.

Ignorando mayúsculas:

```bash
grep -i "error" servidor.log
```

Buscando dentro de carpetas:

```bash
grep -ri "error" logs/
```

**`cut`**  
Si tenemos:

```text
Juan,25,Argentina
Pedro,30,Chile
Ana,22,Uruguay
```

Podemos obtener solamente los nombres:

```bash
cut -d ',' -f 1 personas.csv
```

Resultado:

```text
Juan
Pedro
Ana
```

**`tee`**

```bash
ls -l | tee listado.txt
```

Muestra el resultado en pantalla **y además** lo guarda en `listado.txt`.

---

### 🧠 `awk`

`awk` es especialmente útil cuando tenés datos separados en columnas.

Por ejemplo:

```text
Juan 25 80
Pedro 30 90
Ana 22 75
```

Mostrar la primera columna:

```bash
awk '{print $1}' datos.txt
```

Resultado:

```text
Juan
Pedro
Ana
```

Mostrar nombre y tercera columna:

```bash
awk '{print $1, $3}' datos.txt
```

Resultado:

```text
Juan 80
Pedro 90
Ana 75
```

Filtrar:

```bash
awk '$3 >= 80 {print $1}' datos.txt
```

Resultado:

```text
Juan
Pedro
```

Calcular promedio de la tercera columna:

```bash
awk '{suma += $3} END {print suma/NR}' datos.txt
```

---

### 🌐 Red

**`dig`**

```bash
dig google.com
```

Consulta información DNS.

Usando específicamente Google DNS:

```bash
dig @8.8.8.8 google.com
```

**`curl`**

```bash
curl https://example.com
```

Obtiene el contenido de una página.

**`nmap`**

```bash
nmap 192.168.1.5
```

Comprueba puertos abiertos de un equipo que tengas autorización para analizar.

---

### 🔐 SSH

**Conectarse a otro servidor**

```bash
ssh usuario@192.168.1.10
```

**Copiar una clave pública**

```bash
ssh-copy-id usuario@192.168.1.10
```

Después podés entrar mediante tu clave SSH.

**Copiar un archivo**

```bash
scp archivo.txt usuario@192.168.1.10:/home/usuario/
```

---

### 🕳️ Túneles SSH

**`-L` — acceder a un servicio interno**

```bash
ssh -L 2020:10.8.0.2:22 usuario@servidor-puente
```

Conceptualmente:

```text
TU PC
  │
  │ localhost:2020
  ▼
SERVIDOR PUENTE
  │
  │
  ▼
10.8.0.2:22
```

Después:

```bash
ssh -p 2020 usuario@localhost
```

**`-D` — proxy SOCKS**

```bash
ssh -D 9999 usuario@servidor
```

Tu navegador puede utilizar `localhost:9999` como proxy SOCKS5.

**`-X` — aplicaciones gráficas**

```bash
ssh -X usuario@servidor
```

Luego, si el servidor y la configuración lo permiten:

```bash
gedit
```

La aplicación gráfica puede mostrarse en tu máquina local.

---

### 🖥️ Procesos y rendimiento

**`lscpu`**

```bash
lscpu
```

Información sobre la CPU.

**`free`**

```bash
free -h
```

Muestra RAM y swap en formato legible.

**`uptime`**

```bash
uptime
```

Muestra tiempo encendido y carga promedio.

**`w`**

```bash
w
```

Muestra usuarios conectados y qué están haciendo.

**`ps`**

```bash
ps aux
```

Lista procesos.

Para buscar uno:

```bash
ps aux | grep nginx
```

**`top`**

```bash
top
```

Monitoriza procesos y recursos en tiempo real.

**`htop`**

```bash
htop
```

Una alternativa más cómoda visualmente a `top`.

---

### ⚙️ Control de procesos

**`kill`**

```bash
kill 771
```

Envía una señal al proceso con PID `771`.

Si necesitás terminarlo de forma más contundente:

```bash
kill -9 771
```

⚠️ `kill -9` debería ser el último recurso.

**`sleep`**

```bash
sleep 10
```

Espera 10 segundos.

También:

```bash
sleep 1m
```

**Segundo plano**

```bash
sleep 60 &
```

El `&` hace que el proceso se ejecute en segundo plano.

---

### 💾 Discos

**`fdisk`**

```bash
sudo fdisk -l
```

Lista discos y particiones.

**`cfdisk`**

```bash
sudo cfdisk /dev/sdb
```

Abre una interfaz interactiva para administrar las particiones de `/dev/sdb`.

⚠️ Crear, borrar o modificar particiones puede provocar pérdida de datos.

**`dd`**

Ejemplo conceptual de crear una imagen:

```bash
sudo dd if=/dev/sdb of=disco.img bs=4M status=progress
```

- `if` → archivo/dispositivo de entrada.
    
- `of` → archivo de salida.
    
- `bs` → tamaño del bloque.
    
- `status=progress` → muestra progreso.
    

⚠️ `dd` es especialmente peligroso porque un `of=` incorrecto puede sobrescribir un disco.

---

### 👤 Usuarios y permisos

**`sudo`**

```bash
sudo apt update
```

Ejecuta el comando con privilegios administrativos.

**`whoami`**

```bash
whoami
```

Puede devolver:

```text
juan
```

**`pwgen`**

```bash
pwgen 16 3
```

Genera 3 contraseñas de 16 caracteres, si `pwgen` está instalado.

---

### ⏰ Automatización

**`crontab`**

Editar tus tareas programadas:

```bash
crontab -e
```

Por ejemplo:

```cron
0 11 * * * /home/juan/backup.sh
```

Significa:

```text
minuto     0
hora       11
día        cualquiera
mes        cualquiera
semana     cualquiera
```

→ Ejecutar `backup.sh` todos los días a las **11:00**.

---

### 🛠️ Utilidades

**`history`**

```bash
history
```

Buscar comandos anteriores:

```bash
history | grep ssh
```

**`which`**

```bash
which bash
```

Puede devolver:

```text
/usr/bin/bash
```

**`date`**

```bash
date
```

Para calcular una fecha futura en GNU/Linux:

```bash
date -d "7 days"
```

**`bc`**

```bash
echo "25 * 4" | bc
```

Resultado:

```text
100
```

**`exit`**

```bash
exit
```

Cierra la shell o la sesión SSH.

---

### 📝 Editores

**`nano`**

```bash
nano archivo.txt
```

- Escribís directamente.
    
- `Ctrl + X` → salir.
    
- `Ctrl + O` → guardar.
    

**`vim`**

```bash
vim archivo.txt
```

Una secuencia básica:

```text
i       → escribir
Esc     → volver al modo normal
:w      → guardar
:q      → salir
:wq     → guardar y salir
:q!     → salir descartando cambios
```

---

### ⌨️ Atajos Bash importantes

|Atajo|Función|
|---|---|
|`Ctrl + R`|Buscar en el historial|
|`Ctrl + A`|Ir al principio de la línea|
|`Ctrl + E`|Ir al final|
|`Ctrl + C`|Interrumpir comando|
|`Ctrl + L`|Limpiar pantalla|
|`Ctrl + W`|Borrar palabra anterior|
|`Ctrl + Z`|Suspender proceso|
|`Ctrl + D`|Cerrar sesión/EOF|
|`Ctrl + T`|Intercambiar los dos caracteres anteriores|
|`Esc + T`|Intercambiar las dos palabras anteriores|

**Una corrección importante de tu guía:** `rsync -av images/ images2/` **no significa simplemente "omitir archivos que ya existen"**. `rsync` compara metadatos y, según las opciones, determina qué necesita actualizar; además, `-a` incluye varias opciones de preservación y `-v` significa _verbose_.

También conviene tener especial cuidado con `dd`, `fdisk`, `cfdisk`, `kill -9`, `find ... -delete` y los túneles SSH, porque pueden modificar o interrumpir sistemas si se ejecutan sobre el objetivo equivocado.