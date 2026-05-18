¡Hola! Te explico paso a paso cómo configurar **qutebrowser** para **restaurar todas las ventanas anteriores** al abrirlo de nuevo y cómo hacer **zoom**. Todo se configura en el archivo `~/.config/qutebrowser/config.py`.

## 1. **Restaurar todas las ventanas anteriores (session restore)**

Añade estas líneas a tu `config.py`:

```python
# Restaurar sesión completa al iniciar (todas las ventanas y pestañas)
c.session.lazy_restore = True

# O para restaurar inmediatamente (más agresivo)
# c.session.lazy_restore = False

# Guardar/cargar sesión automáticamente
config.bind('<Ctrl-q>', 'session-save')
config.bind('<Ctrl-Shift-q>', 'session-load')

# Opcional: Auto-guardar sesión cada X segundos
c.auto_save_session = True
```

**Pasos:**
1. Edita `~/.config/qutebrowser/config.py` (créalo si no existe)
2. Añade el código de arriba
3. Reinicia qutebrowser: `:restart`

**Resultado:** Al cerrar qutebrowser, guarda todo automáticamente. Al abrirlo de nuevo, **restaura TODAS las ventanas y pestañas exactamente como estaban**.

---

## 2. **Zoom (acercar/alejar)**

### **Atajos de teclado por defecto:**
```
+          → Zoom IN (acercar)
-          → Zoom OUT (alejar)
0          → Zoom 100% (normal)
gf         → Zoom IN página completa
gF         → Zoom OUT página completa
```

### **Configuración personalizada (opcional):**
```python
# Bindings personalizados para zoom
config.bind(',+', 'zoom-in')
config.bind(',-', 'zoom-out')
config.bind(',0', 'zoom')

# Zoom por defecto al 100%
c.zoom.default_target = 'page'

# Nivel inicial de zoom
c.zoom.level.min = 50
c.zoom.level.max = 500
c.zoom.level.to_factor = 1.2  # Incremento del 20%
```

### **Zoom con ratón (opcional):**
```python
# Zoom con rueda del ratón + Ctrl
config.bind('<Ctrl-+>', 'zoom-in')
config.bind('<Ctrl-->', 'zoom-out')
config.bind('<Ctrl-0>', 'zoom')
```

---

## **Configuración completa recomendada:**

```python
# ==================== SESSION RESTORE ====================
c.session.lazy_restore = True
c.auto_save_session = True

# ==================== ZOOM ===========================
config.bind(',+', 'zoom-in')
config.bind(',-', 'zoom-out')
config.bind(',0', 'zoom')
c.zoom.level.to_factor = 1.2

# ==================== ATAJOS ÚTILES ===================
config.bind('<Ctrl-q>', 'session-save')
config.bind('<Ctrl-Shift-q>', 'session-load')
```

**¡Listo!** Guarda el archivo y ejecuta `:restart` en qutebrowser. 

¿Necesitas ayuda con algo más? 🚀