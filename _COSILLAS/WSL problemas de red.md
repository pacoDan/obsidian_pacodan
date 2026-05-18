El comando:

```bash
getcap /bin/ping
```

sirve para **ver qué capacidades especiales (capabilities) tiene asignado el binario `ping`**.

---

### 🧠 ¿Qué son las “capabilities”?

En Linux, en lugar de dar permisos completos de root, se pueden dar **privilegios específicos y limitados** a un programa. Eso forma parte del sistema de capacidades del kernel (Linux kernel).

Por ejemplo:

- `cap_net_raw` → permite usar sockets “raw” (necesarios para `ping`)
    
- Evita tener que ejecutar todo como root
    

---

### 🔍 ¿Qué hace exactamente el comando?

- `getcap` → muestra las capacidades de un archivo
    
- `/bin/ping` → el ejecutable que estás inspeccionando
    

---

### 📌 Ejemplo de salida

```bash
/bin/ping = cap_net_raw+ep
```

Esto significa:

- `cap_net_raw` → puede usar sockets raw (lo que necesita `ping`)
    
- `+ep`:
    
    - `e` = effective (se aplica al ejecutarse)
        
    - `p` = permitted (está permitido usarla)
        

---

### ❗ Si no muestra nada

Si ejecutás:

```bash
getcap /bin/ping
```

y no devuelve nada, significa que:

👉 `ping` **NO tiene la capacidad necesaria**

Y por eso aparece tu error:

```
missing cap_net_raw+p capability
```

---

### 🔧 Cómo se relaciona con tu problema

`ping` necesita enviar paquetes ICMP directamente → eso requiere privilegios bajos del sistema.

Antes se resolvía con:

- `setuid root` (más inseguro)
    

Ahora se usa:

- capabilities (`cap_net_raw`) → más seguro y específico
    

---

### 🧩 Resumen simple

`getcap /bin/ping` te dice:

👉 “¿Este `ping` tiene permiso especial para funcionar sin ser root?”

- ✔️ Sí → debería funcionar
    
- ❌ No → falla con el error que viste
    

---

Si querés, puedo explicarte también la diferencia entre **capabilities vs setuid**, que es clave para entender por qué Linux moderno usa esto.





