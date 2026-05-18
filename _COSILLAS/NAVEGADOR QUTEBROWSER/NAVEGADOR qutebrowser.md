## 🎯 **Guía rápida qutebrowser: URL + AdBlock**

### 1. **Editar URL (navegación)**

**Método principal: Modo comando**
```
1. Presiona `:` (dos puntos)
2. Escribe: `open https://ejemplo.com`
3. Presiona ⏎ ENTER
```

**Atajos rápidos:**
```
:open google.com           → Navegar
:open -t google.com        → Nueva pestaña
:open -b google.com        → Nueva ventana
:open -r google.com        → Reemplazar pestaña actual

o  → Abrir URL en pestaña actual
O  → Nueva pestaña
```

**Ejemplo práctico:**
```
:open https://youtube.com
:open -t https://github.com
```

### 2. **Activar AdBlock (3 métodos)**

#### **Método 1: AdBlock nativo (Recomendado)**
Crea `~/.config/qutebrowser/config.py`:
```python
# AdBlock básico integrado
c.content.blocking.method = 'auto'
c.content.blocking.hostslists = [
    'https://raw.githubusercontent.com/easylist/easylist/master/easylist/easylist.txt',
    'https://raw.githubusercontent.com/easylist/easylist/master/easylist/easyprivacy.txt',
]

# Opcional: listas más agresivas
c.content.blocking.hostslists += [
    'https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts'
]
```

#### **Método 2: uBlock Origin (Mejor protección)**
```bash
# Instalar extensión
pip install qutebrowser-uBlock
```

En `config.py`:
```python
config.load_autoconfig(True)

# uBlock Origin
c.content.webengines.allowed_extensions = [
    'mp4', 'webm', 'ogg', 'ogv', 'opus'
]

# Deshabilitar JS por defecto (opcional)
c.content.javascript.enabled = False
```

#### **Método 3: Configuración completa anti-ads**
```python
# ~/.config/qutebrowser/config.py COMPLETO
import os

c.content.blocking.method = 'auto'
c.content.blocking.hostslists = [
    'https://easylist.to/easylist/easylist.txt',
    'https://easylist.to/easylist/easyprivacy.txt',
    'https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts',
]

# Anti-tracking agresivo
c.content.cookies.accept = 'no-3rdparty'
c.content.doctype.remove = True
c.content.headers.user_agent = 'Mozilla/5.0 (compatible; MinimalBrowser)'

# Rendimiento CPU antigua
c.content.javascript.enabled = False  # Desactivar JS global
c.content.plugins = False
c.content.autoplay = False
c.content.media.audio_mute = True

# Interfaz minimalista
c.window.hide_decoration = True
c.tabs.show = 'never'  # Ocultar tabs (navegar con gt/gT)
```

### 3. **Atajos útiles para navegar**

```
:open URL              → Navegar
o URL                  → Abrir rápido
gt/gT                  → Siguiente/Anterior pestaña
J/K                    → Pestañas arriba/abajo
d                      → Cerrar pestaña
u                      → Pestaña anterior
yy                     → Copiar URL
```

### 4. **Instalación + Configuración 1-click**

```bash
# 1. Instalar
sudo apt install qutebrowser

# 2. Crear config completa
mkdir -p ~/.config/qutebrowser
cat > ~/.config/qutebrowser/config.py << 'EOF'
c.content.blocking.method = 'auto'
c.content.blocking.hostslists = [
    'https://easylist.to/easylist/easylist.txt',
    'https://easylist.to/easylist/easyprivacy.txt',
]
c.content.javascript.enabled = False
c.content.cookies.accept = 'no-3rdparty'
EOF

# 3. Ejecutar
qutebrowser
```

### 5. **Verificar AdBlock funcionando**
```
1. Ve a: :open https://d3ward.github.io/toolz/adblock.html
2. Debe mostrar "You're protected! 🎉"
```

**¡Listo!** Ahora tienes qutebrowser súper ligero con adblock potente. 

**Probar ahora:** Presiona `:` → `open https://ejemplo.com` 🚀

¿Alguna duda con los atajos o configuración?