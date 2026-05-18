
## 🥇 **Mejores opciones (más ligeras)**

### 1. **qutebrowser** (Recomendado #1)
```bash
# Instalar
sudo apt install qutebrowser  # Debian/Ubuntu
sudo dnf install qutebrowser  # Fedora
sudo pacman -S qutebrowser    # Arch
```
```
✅ Pros:
• Basado en QtWebEngine (Chromium ligero)
• Sin JS por defecto (configurable)
• ~100-200MB RAM
• Muy rápido en CPUs antiguas
• Bloqueo nativo de trackers/JS
• Configuración vía Python (potente)
```
**Configuración mínima JS:**
```python
# ~/.config/qutebrowser/config.py
content.javascript.enabled = False
content.autoplay = False
content.plugins = False
```

### 2. **Ungoogled Chromium** (con flags)
```bash
# Instalar versión ligera
sudo apt install ungoogled-chromium
# O compilar sin extras
```
```
Flags para CPU antigua (crear ~/.config/chromium-flags.conf):
--disable-background-timer-throttling
--disable-backgrounding-occluded-windows
--disable-renderer-backgrounding
--disable-features=TranslateUI
--disable-extensions
--disable-plugins-discovery
--no-sandbox  # Solo si es necesario
```

## 🥈 **Opciones ultra-ligeras**

### 3. **Links2** (Terminal - 10MB RAM)
```bash
sudo apt install links2
links2 -g https://ejemplo.com
```
```
✅ Perfecto para CPUs muy antiguas
✅ Sin JS completamente
✅ Renderizado gráfico ligero
✅ Navegación por teclado
```

### 4. **Dillo** (Gráfico minimalista)
```bash
sudo apt install dillo
```
```
✅ ~20-50MB RAM
✅ Sin JS/WebGL
✅ HTML/CSS básico muy rápido
✅ Ideal para documentación
```

### 5. **Otter Browser** (Retro)
```bash
# Flatpak (más fácil)
flatpak install flathub org.otter_browser.OtterBrowser
```
```
✅ Basado en QtWebKit antiguo
✅ Muy ligero (~80MB)
✅ Sin JS moderno problemático
✅ Estilo navegador 90s
```

## ⚙️ **Configuración optimizada para todas**

### Script de optimización general:
```bash
#!/bin/bash
# Guardar como optimize-browser.sh

export QTWEBENGINE_CHROMIUM_FLAGS="--disable-gpu --disable-software-rasterizer --disable-background-timer-throttling --no-sandbox --disable-extensions --disable-plugins --disable-javascript-harmony-shipping"
export QTWEBENGINE_DISABLE_SANDBOX=1
```

### Comparativa de consumo RAM:
```
qutebrowser:     100-250MB
Ungoogled Cr:    200-400MB
Otter:           80-150MB
Dillo:           20-50MB
Links2:          10-30MB
Supermium:     400-800MB ⚠️
```

## 🎯 **Recomendación final por caso:**

```
CPU muy antigua (< 2GB RAM):  Links2 + Dillo
CPU antigua (2-4GB RAM):     qutebrowser
Navegación básica:           Otter Browser
Más moderno pero ligero:     Ungoogled Chromium + flags
```

**qutebrowser es tu mejor apuesta** - combina velocidad, bajo consumo y control total sobre JS. ¡Pruébalo primero! 🚀

¿Necesitas ayuda instalando alguna específicamente?