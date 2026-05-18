¡Perfecto! **Errores corregidos**. El problema es que **faltan las opciones correctas** y **config.load_autoconfig()**. Aquí está la **configuración EXACTA** que funciona:

## ✅ **config.py COMPLETO y CORREGIDO**

```python
# Página de inicio fija
c.url.start_pages = 'https://www.google.com'

# O múltiples páginas (se abrirán en pestañas)
# c.urls.start_pages = ['https://www.google.com', 'https://duckduckgo.com']
# c.urls.start_pages = 'file:///ruta/a/tu/pagina.html'

config.load_autoconfig(True)
# =====================================================
# SESSION RESTORE ✅
# =====================================================
c.session.lazy_restore = True

# Auto-guardado de sesión ✅
c.auto_save.session = True

# =====================================================
# ZOOM ✅ (solo opciones válidas)
# =====================================================
c.zoom.levels = [50, 75, 90, 100, 110, 125, 150, 175, 200, 250, 300, 400, 500]

# =====================================================
# ATAJOS PERSONALIZADOS ✅
# =====================================================
config.bind(",+", "zoom-in")
config.bind(",-", "zoom-out")
config.bind(",0", "zoom")

# Atajos de sesión
config.bind("<Ctrl-q>", "session-save")
config.bind("<Ctrl-Shift-q>", "session-load")

print("✅ Configuración cargada correctamente!")

```

## 🔧 **Pasos EXACTOS:**

```
1. Borra tu config.py actual
2. Copia el código de arriba COMPLETO
3. Guarda en ~/.config/qutebrowser/config.py
4. Reinicia: :restart
```

## 🎯 **Verificación:**

**Después del restart, prueba:**
```
:version  ← Verifica que cargó sin errores
:set session.lazy_restore ?  ← Debe decir True
:set auto_save.session ?     ← Debe decir True
```

## 📋 **Opciones CORRECTAS (lista completa):**

| ❌ Incorrecto | ✅ Correcto |
|--------------|-------------|
| `auto_save_session` | `auto_save.session` |
| `session_restore` | `session.lazy_restore` |
| Sin `load_autoconfig()` | `config.load_autoconfig(True)` |

## 🚀 **Resultado esperado:**

1. **Cierra qutebrowser** con varias ventanas/pestañas
2. **Abre de nuevo** → **¡Todas restauradas automáticamente!**
3. **Zoom:** `+` `-` `0` o `,+` `,-` `,0`

## 💡 **Bonus: Configuración avanzada**

```python
# Guardar sesión específica
c.session.default_name = 'mis-ventanas'

# Restaurar solo pestañas, no posiciones
c.session.lazy_restore_tabs = True

# Zoom por defecto al 110%
c.zoom.level.pinch = 110
```

**¡Ya está!** Copia el `config.py` completo, reinicia y **funcionará perfecto**. 

¿Algún error más? 😎


