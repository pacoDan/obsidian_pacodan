https://hub.docker.com/r/ollama/ollama#:~:text=language%20models%20locally.-,CPU%20only,-docker%20run%20%2Dd
## 🧱 1. Crear y levantar el contenedor de Ollama

Este va primero (porque si no, no hay servidor corriendo):

```bash
docker run -d \
  -v ollama:/root/.ollama \
  -p 11434:11434 \
  --name ollama \
  ollama/ollama
```

👉 Esto:

- crea el contenedor  
    
- guarda modelos en el volumen `ollama`  
    
- expone la API en `localhost:11434`  
    

---

## ⏳ 2. Esperar a que arranque

Podés chequear:

```bash
docker logs -f ollama
```

Cuando veas que ya está listo, seguís.

---

## 📥 3. Descargar y ejecutar DeepSeek R1

Ahora sí, corrés el modelo dentro del contenedor:

```bash
docker exec -it ollama ollama run deepseek-r1
```

👉 Esto:

- descarga el modelo (la primera vez)  
    
- lo ejecuta en modo interactivo  
    

---

## ⚡ Opcional: elegir versión liviana

Si querés algo más ligero (recomendado):

```bash
docker exec -it ollama ollama run deepseek-r1:1.5b
```

o un balance mejor:

```bash
docker exec -it ollama ollama run deepseek-r1:7b
```

---

## 🔁 Comandos útiles extra

### Ver modelos instalados

```bash
docker exec -it ollama ollama list
```

### Parar contenedor

```bash
docker stop ollama
```

### Volver a iniciarlo

```bash
docker start ollama
```

---

## 💡 Resumen corto

Orden correcto:

1. `docker run ... ollama/ollama`
2. esperar que arranque
3. `docker exec -it ollama ollama run deepseek-r1`

---

Si querés, en el siguiente paso te armo:

- cómo usarlo desde una app (OpenWebUI)  
    
- o cómo pegarle por API tipo ChatGPT local 👀


----
----

para verificar que ollama esta corriendo :
```sh
curl -s http://localhost:11434/api/tags | jq '.models[].name'
```
suponiendo que esta `mistral` y no `deepseek-r1`:
```sh
# Probar generación con Ollama directamente
curl -X POST http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mistral",
    "prompt": "Hola, ¿cómo estás?",
    "stream": false
  }'
```




----



LOCAL

Si buscas una alternativa a Handy para tener una conversación por voz con un modelo local en Ollama (DeepSeek, Mistral, Llama, Qwen, etc.), necesitas básicamente tres componentes:

1. **STT (Speech-to-Text)** → Convierte tu voz en texto.
    
2. **LLM (Ollama)** → Procesa la consulta.
    
3. **TTS (Text-to-Speech)** → Convierte la respuesta en audio.
    

Algunas alternativas interesantes:

## 1. [Open WebUI](https://openwebui.com/?utm_source=chatgpt.com)

Es probablemente la opción más completa para Ollama.

Características:

- Integración nativa con Ollama.
    
- Soporte para STT.
    
- Soporte para TTS.
    
- Interfaz web moderna.
    
- Conversaciones por voz.
    

Puede conectarse a:

- Whisper
    
- Faster-Whisper
    
- OpenAI-compatible APIs
    
- Piper TTS
    

Instalación rápida:

```bash
docker run -d \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main
```

---

## 2. [LibreChat](https://www.librechat.ai/?utm_source=chatgpt.com)

Soporta:

- Ollama
    
- OpenAI
    
- Anthropic
    
- Gemini
    
- Mistral
    

Y tiene:

- Entrada por voz.
    
- Plugins.
    
- Agentes.
    

Es más orientado a "ChatGPT local".

---

## 3. [Voice Chat AI (Ollama Voice) GitHub Search](https://github.com/search?q=ollama+voice+chat&type=repositories&utm_source=chatgpt.com)

Existen muchos proyectos especializados en voz para Ollama.

Algunos combinan:

```text
Micrófono
   ↓
Whisper
   ↓
Ollama
   ↓
Piper TTS
   ↓
Altavoz
```

Esta arquitectura suele tener menos de 1 segundo de latencia en una PC moderna.

---

## 4. [Piper TTS](https://github.com/rhasspy/piper?utm_source=chatgpt.com)

Para la salida de voz.

Ventajas:

- Corre localmente.
    
- Muy rápido.
    
- Consumo bajo de CPU.
    
- Voces en español.
    

Ejemplo:

```bash
echo "Hola mundo" | piper \
  --model es_AR-mls_10246-medium.onnx \
  --output_file salida.wav
```

---

## 5. [Faster-Whisper](https://github.com/SYSTRAN/faster-whisper?utm_source=chatgpt.com)

Para reconocimiento de voz local.

Mucho más rápido que Whisper original.

Ejemplo:

```python
from faster_whisper import WhisperModel

model = WhisperModel("small")

segments, _ = model.transcribe("audio.wav")

for segment in segments:
    print(segment.text)
```

---

## 6. [Home Assistant Assist](https://www.home-assistant.io/voice_control/?utm_source=chatgpt.com)

Si quieres algo tipo asistente de voz permanente.

Puede integrar:

```text
Micrófono
 ↓
Whisper
 ↓
Ollama
 ↓
Piper
 ↓
Parlantes
```

y escuchar continuamente.

---

## 7. Solución DIY (la que suele dar mejores resultados)

Mucha gente arma:

```text
Micrófono
 ↓
Faster-Whisper
 ↓
DeepSeek-R1 (Ollama)
 ↓
Piper TTS
 ↓
PulseAudio/PipeWire
 ↓
Altavoz
```

Todo local.

Stack típico:

```bash
ollama run deepseek-r1:8b

pip install faster-whisper

git clone https://github.com/rhasspy/piper
```

Con un script Python de unas 100 líneas puedes lograr conversación full duplex.

---

## Mi recomendación

Si buscas algo que funcione ya mismo:

1. Instala [Open WebUI](https://openwebui.com/?utm_source=chatgpt.com)
    
2. Usa Ollama con:
    
    - DeepSeek-R1 o
        
    - Mistral
        
3. Añade:
    
    - Faster-Whisper para STT
        
    - Piper para TTS
        

Obtendrás un asistente completamente local, sin depender de APIs externas y con buena latencia incluso en hardware modesto.