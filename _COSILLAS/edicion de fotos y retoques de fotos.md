 Programas Open Source para Filtros de Fotos

Aquí te comparto algunas opciones de código abierto que puedes usar:

## 1. **GIMP** (Más recomendado)
- Es como un Photoshop gratuito
- Tiene filtros y ajustes rápidos (color, brillo, saturación)
- Puedes crear tus propios presets
- **Descargar**: https://www.gimp.org/downloads/

## 2. **Darktable**
-Excelente para edición no destructiva
-Tiene módulos con preajustes ähnlich a los de cámara
-Permite crear y guardar estilos
-**Descargar**: https://www.darktable.org/

## 3. **RawTherapee**
-Editor RAW gratuito
-Tiene muchos ajustes preestablecidos
-Muy buena recuperación de detalles
-**Descargar**: https://rawtherapee.com/

## 4. **ImageMagick** (Para desarrolladores)
-Si sabes programación, puedes crear scripts con filtros
-Es una herramienta de línea de comandos
-**Descargar**: https://imagemagick.org/

---

## Ejemplo simple con Python + Pillow

Si quieres crear tus propios filtros, aquí un código básico:

```python
from PIL import Image, ImageFilter, ImageEnhance

def aplicar_filtro(imagen_path, tipo_filtro):
    img = Image.open(imagen_path)
    
    if tipo_filtro == "calida":
        # Aumentar rojo y amarillo
        enhancer = ImageEnhance.Color(img)
        img = enhancer.enhance(1.2)
        enhancer = ImageEnhance.Brightness(img)
        img = enhancer.enhance(1.1)
        
    elif tipo_filtro == "vivid":
        # Aumentar saturación
        enhancer = ImageEnhance.Color(img)
        img = enhancer.enhance(1.5)
        enhancer = ImageEnhance.Contrast(img)
        img = enhancer.enhance(1.2)
        
    elif tipo_filtro == "lima":
        # Efecto lima/cálido suave
        img = img.filter(ImageFilter.SMOOTH_MORE)
        
    img.save(f"foto_{tipo_filtro}.jpg")
    print(f"Filtro {tipo_filtro} aplicado!")

# Usar:
# aplicar_filtro("mi_foto.jpg", "calida")
```

---

## ¿Cuál te recomiendo?

| Necesidad | Programa |
|----------|----------|
| Editor visual completo | **GIMP** |
| Estilos predefinidos | **Darktable** |
| Edición rápida | **RawTherapee** |
| Crear mis propios filtros | **Python + Pillow** |

¿Quieres que te ayude a instalar alguno o que profundice en el código de filtros?