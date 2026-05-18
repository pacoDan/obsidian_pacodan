Si te referís a qutebrowser (el navegador minimalista controlado por teclado), estos son los comandos y atajos más útiles para empezar, además de cómo activar bloqueo de anuncios.

# Atajos básicos importantes en qutebrowser

## Pestañas

- `J` → pestaña anterior
    
- `K` → pestaña siguiente
    
- `d` → cerrar pestaña actual
    
- `u` → reabrir pestaña cerrada
    
- `g0` → ir a primera pestaña
    
- `g$` → ir a última pestaña
    

## Navegación

- `H` → atrás
    
- `L` → adelante
    
- `R` → recargar
    
- `r` → recargar sin caché
    
- `gg` → inicio de página
    
- `G` → final de página
    

## Abrir cosas

- `o` → abrir URL
    
- `O` → editar URL actual
    
- `t` → nueva pestaña
    
- `:open URL`
    
- `:tab-close`
    
- `:quit`
    

## Buscar

- `/texto` → buscar en página
    
- `n` → siguiente coincidencia
    
- `N` → coincidencia anterior
    

## Modo hints (super útil)

- `f` → mostrar letras para abrir links
    
- `F` → abrir link en nueva pestaña
    
- `;d` → descargar algo
    
- `;h` → hover sobre elemento
    

## Zoom

- `+`
    
- `-`
    
- `=`
    

---

# Comandos importantes

Entrás comandos usando `:` igual que en Vim.

Ejemplos:

```bash
:tab-close
:tab-only
:open https://youtube.com
:set tabs.position left
```

---

# Cómo instalar AdBlock en qutebrowser

qutebrowser ya soporta bloqueo de anuncios.

## Método recomendado: usar listas tipo uBlock

Primero instalá dependencias.

En Arch:

```bash
sudo pacman -S python-adblock
```

En Debian/Ubuntu:

```bash
sudo apt install python3-adblock
```

---

# Activar bloqueo

Abrí qutebrowser y ejecutá:

```bash
:set content.blocking.enabled true
```

Después:

```bash
:adblock-update
```

Eso descarga listas tipo EasyList.

---

# Configuración permanente

Editá:

```bash
~/.config/qutebrowser/config.py
```

Agregá:

```python
c.content.blocking.enabled = True
```

---

# Listas personalizadas

Podés agregar listas extra:

```python
c.content.blocking.adblock.lists = [
    "https://easylist.to/easylist/easylist.txt",
    "https://easylist.to/easylist/easyprivacy.txt"
]
```

---

# Bloquear JavaScript en sitios específicos

Ejemplo:

```python
config.set('content.javascript.enabled', False, 'https://facebook.com/*')
```

---

# Comandos MUY útiles de power user

## Ver bindings

```bash
:bind
```

## Editar config

```bash
:config-edit
```

## Reiniciar config

```bash
:config-source
```

## Abrir downloads

```bash
:download
```

## Abrir inspector devtools

```bash
:devtools
```

---

# Integración con Vim

Muchos usuarios usan:

- `hjkl` para mover
    
- `yy` copiar URL
    
- `pp` abrir URL copiada
    
- `Ctrl+Shift+p` command palette
    

---

# Copiar URL

- `yy` → copiar URL actual
    
- `yt` → copiar título
    
- `yY` → copiar URL en formato markdown
    

---

# Configuración recomendada minimalista

En `config.py`:

```python
c.tabs.show = 'multiple'
c.auto_save.session = True
c.content.blocking.enabled = True
c.scrolling.smooth = True
c.tabs.position = "top"
```

---

# Tema oscuro

```bash
:set colors.webpage.darkmode.enabled true
```

Permanente:

```python
c.colors.webpage.darkmode.enabled = True
```

---

# Ayuda integrada

```bash
:help
```

o:

```bash
:help settings
```

Documentación oficial:

[qutebrowser docs](https://qutebrowser.org/doc/help/?utm_source=chatgpt.com)



----


comandos YANK 
En qutebrowser y en editores tipo Vim, **“yank”** significa:

> **copiar al portapapeles**.

Viene de la terminología de Vim.

---

# Diferencia rápida

- `y` → yank → copiar
    
- `d` → delete → cortar/eliminar
    
- `p` → paste → pegar
    

---

# Ejemplos en qutebrowser

## Copiar URL actual

```text
yy
```

Hace “yank” de la URL actual.

---

## Copiar título

```text
yt
```

---

## Copiar URL en markdown

```text
yY
```

Ejemplo:

```markdown
[Google](https://google.com)
```

---

# En Vim

## Copiar línea

```vim
yy
```

## Copiar 5 líneas

```vim
5yy
```

## Pegar

```vim
p
```

---

# Registros (registers)

Vim y qutebrowser usan “registers” internos para guardar cosas copiadas.

Ejemplo:

```vim
"ayy
```

Copia al registro `a`.

---

# Clipboard del sistema

En Linux hay dos tipos comunes:

- PRIMARY → selección con mouse
    
- CLIPBOARD → Ctrl+C / Ctrl+V
    

qutebrowser normalmente usa CLIPBOARD.

---

# Ver qué copiaste

En Linux:

```bash
xclip -o
```

o:

```bash
wl-paste
```

(depende si usás X11 o Wayland)

---

# Curiosidad

El término “yank” existe desde los editores Unix viejos:

- vi
    
- vim
    
- neovim
    

Por eso muchos programas inspirados en Vim usan:

- yank
    
- registers
    
- motions
    
- operators
    

como lenguaje propio. 