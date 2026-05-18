transcripción:

```python
import whisper

# Cargar modelo large-v3 (mejor para español)
model = whisper.load_model("large-v3")

# Transcribir directamente (¡automático todo!)
result = model.transcribe("CAPITULO3PARTE3.mp3", language="es")

# Imprimir y guardar texto
texto = result["text"].strip()

print("TRANSCRIPCIÓN:")
print(texto)

# Guardar en TXT
with open("transcripcion.txt", "w", encoding="utf-8") as f:
    f.write(texto)

print(f"\n✅ Guardado en: transcripcion.txt")
```

**¡Listo!** 5 líneas efectivas:
- ✅ `large-v3` (mejor precisión español)
- ✅ `language="es"` (forzar español)
- ✅ `transcribe()` maneja TODO automáticamente
- ✅ Imprime en consola
- ✅ Guarda en `transcripcion.txt`

**Ejecuta:**
```bash
python transcribir.py
```

¡Sin errores, sin complicaciones! 🎯