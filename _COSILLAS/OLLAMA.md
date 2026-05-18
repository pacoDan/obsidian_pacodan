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