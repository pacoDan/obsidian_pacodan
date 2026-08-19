Para lograr que el frontend escuche de manera inteligente, detecte el volumen de tu voz y corte automáticamente la grabación tras 3 segundos de silencio, el secreto está en usar la API nativa de audio del navegador (`Web Audio API`).

A través de un nodo analizador, el navegador mide los decibelios (el nivel sonoro) del micrófono en tiempo real. Si el volumen cae por debajo de un umbral preestablecido de manera continua durante 3 segundos, detiene la grabación, envía el archivo al backend de Faster-Whisper, y se vuelve a encender de inmediato para recibir la siguiente frase. [1, 2, 3, 4]

Aquí tienes el sistema optimizado y listo para desplegar en tu entorno de Miniconda.

---

## 1. El Servidor Backend Actualizado (`app.py`)

Mantenemos la potencia de tu GPU NVIDIA, pero cambiamos el formato de salida a texto simple JSON para que el frontend lo procese limpiamente.

```python
import os
import shutil
from fastapi import FastAPI, UploadFile, File
from fastapi.middleware.cors import CORSMiddleware
from fastapi.responses import HTMLResponse
from faster_whisper import WhisperModel

os.environ["KMP_DUPLICATE_LIB_OK"] = "TRUE"
os.environ["HF_HUB_DISABLE_SYMLINKS"] = "1"

app = FastAPI()

# Permite conectar celulares u otras computadoras de tu red local
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Inicializar tu modelo optimizado en GPU local
model = WhisperModel("large-v3", device="cuda", compute_type="float16", local_files_only=True)

@app.post("/transcribir")
async def transcribir_audio(file: UploadFile = File(...)):
    temp_path = f"instantaneo_{file.filename}.wav"
    with open(temp_path, "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    
    # Parámetros optimizados para discursos / oratoria corta-mediana
    segments, info = model.transcribe(
        temp_path,
        language="es",
        beam_size=5,
        vad_filter=True
    )
    
    texto_completo = " ".join([segment.text for segment in segments]).strip()
    os.remove(temp_path)
    
    return {"texto": texto_completo}

@app.get("/", response_class=HTMLResponse)
async def home():
    with open("index.html", "r", encoding="utf-8") as f:
        return f.read()

if __name__ == "__main__":
    import uvicorn
    # Se ejecuta en el puerto 8000 para toda tu red
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

---

## 2. El Frontend Inteligente con Detección de Silencio (`index.html`)

Reemplaza tu archivo `index.html` con este código. Cuenta con un script que analiza las frecuencias de audio del micrófono y maneja el temporizador automático de 3 segundos. [1, 3]

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Whisper Manos Libres</title>
    <script src="https://tailwindcss.com"></script>
</head>
<body class="bg-slate-950 text-white min-h-screen flex flex-col items-center justify-center p-4">

    <div class="max-w-xl w-full bg-slate-900 border border-slate-800 rounded-3xl p-6 shadow-2xl text-center">
        <h1 class="text-2xl font-bold mb-2 text-emerald-400">🎙️ Modo Manos Libres</h1>
        <p class="text-sm text-slate-400 mb-6">Habla de forma continua. El sistema cortará el audio y transcribirá tras 3 segundos de silencio.</p>

        <!-- Botón Central Activo -->
        <button id="btnModo" class="w-40 h-40 bg-emerald-600 hover:bg-emerald-700 active:scale-95 rounded-full font-bold text-sm tracking-wide shadow-lg shadow-emerald-900/30 transition-all border-4 border-slate-800 uppercase">
            Iniciar Escucha
        </button>

        <!-- Barra de nivel de audio visual -->
        <div class="w-full bg-slate-950 h-3 rounded-full mt-6 overflow-hidden border border-slate-800">
            <div id="vumetro" class="bg-emerald-500 h-full w-0 transition-all duration-75"></div>
        </div>

        <p id="estado" class="text-slate-500 mt-3 text-xs font-semibold uppercase tracking-widest">Sistema Inactivo</p>

        <!-- Bloque de resultados acumulativos -->
        <div class="mt-6 text-left">
            <label class="text-xs font-bold text-slate-400 uppercase tracking-wider block mb-2">Historial de Transcripción:</label>
            <div id="historial" class="w-full bg-slate-950 p-4 rounded-2xl min-h-[180px] max-h-[300px] overflow-y-auto text-slate-200 border border-slate-800 font-mono text-sm leading-relaxed">
                <span class="text-slate-600 italic">Las transcripciones aparecerán aquí de forma automática...</span>
            </div>
        </div>
    </div>

    <script>
        let audioContext, analyser, microphone, scriptProcessor;
        let mediaRecorder;
        let audioChunks = [];
        let estaEscuchando = false;
        
        // Parámetros de Silencio Ajustables
        const UMBRAL_SILENCIO = -60; // Decibelios (Menos de -50 / -60 se suele considerar silencio)
        const TIEMPO_SILENCIO_MS = 3000; // 3 segundos automáticos
        let inicioSilencio = null;

        const btnModo = document.getElementById('btnModo');
        const vumetro = document.getElementById('vumetro');
        const estado = document.getElementById('estado');
        const historial = document.getElementById('historial');

        btnModo.addEventListener('click', toggleModo);

        async function toggleModo() {
            if (!estaEscuchando) {
                await iniciarEcosistemaAudio();
            } else {
                detenerEcosistemaAudio();
            }
        }

        async function iniciarEcosistemaAudio() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
                estaEscuchando = true;
                
                btnModo.innerText = "Detener Escucha";
                btnModo.className = btnModo.className.replace('bg-emerald-600 hover:bg-emerald-700', 'bg-rose-600 hover:bg-rose-700');
                estado.innerText = "🟢 Escuchando voz...";

                // Configurar Web Audio API para medir volumen
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                analyser = audioContext.createAnalyser();
                analyser.fftSize = 256;
                microphone = audioContext.createMediaStreamSource(stream);
                scriptProcessor = audioContext.createScriptProcessor(2048, 1, 1);

                microphone.connect(analyser);
                analyser.connect(scriptProcessor);
                scriptProcessor.connect(audioContext.destination);

                // Configurar MediaRecorder para guardar el buffer físico del audio
                configurarMediaRecorder(stream);

                scriptProcessor.onaudioprocess = () => {
                    const array = new Float32Array(analyser.frequencyBinCount);
                    analyser.getFloatTimeDomainData(array);
                    
                    // Calcular el volumen en decibelios (RMS)
                    let suma = 0;
                    for (let i = 0; i < array.length; i++) {
                        suma += array[i] * array[i];
                    }
                    let rms = Math.sqrt(suma / array.length);
                    let db = 20 * Math.log10(rms);
                    if (db === -Infinity) db = -100;

                    // Actualizar Barra Visual (Vúmetro)
                    let porcentajeVolumen = Math.max(0, Math.min(100, (db + 100) * 1.5));
                    vumetro.style.width = `${porcentajeVolumen}%`;

                    // Lógica del Silencio
                    if (db < UMBRAL_SILENCIO) {
                        if (!inicioSilencio) inicioSilencio = Date.now();
                        
                        let tiempoTranscurrido = Date.now() - inicioSilencio;
                        if (tiempoTranscurrido >= TIEMPO_SILENCIO_MS && mediaRecorder.state === "recording") {
                            estado.innerText = "⏳ 3s de silencio detectados. Enviando...";
                            mediaRecorder.stop(); // Detener gatilla el envío al Backend
                            inicioSilencio = null; 
                        }
                    } else {
                        inicioSilencio = null; // Reiniciar si el orador volvió a hablar
                        if(mediaRecorder.state === "recording") {
                            estado.innerText = "🟢 Escuchando voz...";
                        }
                    }
                };

            } catch (err) {
                alert("Error al acceder al micrófono. Asegúrate de dar permisos o usar HTTPS.");
                console.error(err);
            }
        }

        function configurarMediaRecorder(stream) {
            mediaRecorder = new MediaRecorder(stream);
            audioChunks = [];

            mediaRecorder.ondataavailable = e => audioChunks.push(e.data);
            
            mediaRecorder.onstop = async () => {
                const audioBlob = new Blob(audioChunks, { type: 'audio/wav' });
                audioChunks = []; // Limpiar para el siguiente bloque

                const formData = new FormData();
                formData.append('file', audioBlob, 'bloque_oratoria.wav');

                try {
                    const response = await fetch('/transcribir', { method: 'POST', body: formData });
                    const data = await response.json();
                    
                    if (data.texto) {
                        // Agregar texto al historial sin borrar lo anterior
                        if (historial.innerHTML.includes("italic")) historial.innerHTML = "";
                        historial.innerHTML += `<p class="mb-2 p-1 bg-slate-900 border-l-2 border-emerald-500 pl-2">🗣️ ${data.texto}</p>`;
                        historial.scrollTop = historial.scrollHeight; // Auto-scroll
                    }
                } catch (err) {
                    console.error("Error enviando al backend:", err);
                }

                // BUCLE AUTOMÁTICO: Si el usuario no detuvo el modo global, vuelve a grabar al instante
                if (estaEscuchando) {
                    mediaRecorder.start();
                    inicioSilencio = null;
                }
            };

            mediaRecorder.start();
        }

        function detenerEcosistemaAudio() {
            estaEscuchando = false;
            btnModo.innerText = "Iniciar Escucha";
            btnModo.className = btnModo.className.replace('bg-rose-600 hover:bg-rose-700', 'bg-emerald-600 hover:bg-emerald-700');
            estado.innerText = "Sistema Inactivo";
            vumetro.style.width = "0%";

            if (mediaRecorder && mediaRecorder.state !== "inactive") {
                mediaRecorder.stop();
            }
            if (microphone) microphone.disconnect();
            if (scriptProcessor) scriptProcessor.disconnect();
            if (audioContext) audioContext.close();
        }
    </script>
</body>
</html>
```

---

## ¿Cómo probarlo ahora mismo?

1. En tu terminal de Miniconda activa (`whisper_env`), ejecuta el archivo del servidor:
    
    ```sh
    python app.py
    ```
    
2. Abre tu navegador e ingresa a `http://localhost:8000`.
3. Haz clic en Iniciar Escucha, dale permisos al micrófono y empieza a hablar. Notarás que el vúmetro reacciona. Al quedarte callado por 3 segundos precisos, el backend procesará el bloque en tu placa de video y lo pintará de manera acumulativa en pantalla, quedando listo para el siguiente fragmento. [3, 4]

---

💡 Con esta arquitectura de bucle de voz interactivo consolidada:

- ¿Deseas ajustar el umbral de decibelios (`UMBRAL_SILENCIO`) para que sea más sensible si estás en un ambiente con mucho ruido de fondo?
- ¿Te gustaría que agreguemos un botón en el frontend para descargar todo el texto acumulado en un único archivo `.txt` limpio al terminar?

  

[1] [https://stackoverflow.com](https://stackoverflow.com/questions/24515978/html-audio-recording-until-silence)

[2] [https://github.com](https://github.com/soufiiyane/streamlit-realtime-audio-recorder)

[3] [https://hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com/docs/user-guide/features/voice-mode)

[4] [https://www.linkedin.com](https://www.linkedin.com/pulse/implementing-default-recording-time-limit-web-based-audio-reinkordt-1ad7c)




----
----

Esta arquitectura modular es perfecta. Usar Astro en el Frontend te garantiza una interfaz ultraliviana que carga al instante (ideal para celulares), mientras que Flask en el Backend es el estándar clásico, ligero y confiable para exponer APIs de procesamiento en Python.

Aquí tienes la reestructuración completa del sistema adaptada a tus requerimientos tecnológicos.

## 1. El Backend: Flask (`app.py`)

Este script levanta una API limpia con Flask que procesa archivos de audio enviados mediante solicitudes HTTP POST y expone la ruta para recibir datos de tus dispositivos.

```python
import os
import shutil
from flask import Flask, request, jsonify
from flask_cors import CORS
from faster_whisper import WhisperModel

# Evitar colisión de librerías OpenMP en Windows
os.environ["KMP_DUPLICATE_LIB_OK"] = "TRUE"
os.environ["HF_HUB_DISABLE_SYMLINKS"] = "1"

app = Flask(__name__)
# Habilitar CORS para que tu frontend en Astro pueda comunicarse sin bloqueos
CORS(app)

# Inicializar Faster-Whisper optimizado en GPU NVIDIA de manera local
model = WhisperModel("large-v3", device="cuda", compute_type="float16", local_files_only=True)

@app.route("/transcribir", methods=["POST"])
def transcribir_audio():
    if "file" not in request.files:
        return jsonify({"error": "No se envió ningún archivo de audio"}), 400
        
    file = request.files["file"]
    if file.filename == "":
        return jsonify({"error": "Nombre de archivo vacío"}), 400

    # Guardar temporalmente el archivo enviado por el Frontend
    temp_path = f"flask_temp_{file.filename}.wav"
    file.save(temp_path)
    
    try:
        # Transcripción con parámetros optimizados
        segments, info = model.transcribe(
            temp_path,
            language="es",
            beam_size=5,
            vad_filter=True
        )
        
        texto_completo = " ".join([segment.text for segment in segments]).strip()
        return jsonify({"texto": texto_completo})
        
    except Exception as e:
        return jsonify({"error": str(e)}), 500
        
    finally:
        # Asegurar la limpieza del archivo temporal pase lo que pase
        if os.path.exists(temp_path):
            os.remove(temp_path)

if __name__ == "__main__":
    # Asegúrate de instalar Flask: pip install flask flask-cors
    # Escucha en el puerto 5000 para toda tu red local
    app.run(host="0.0.0.0", port=5000, debug=True)
```

---

## 2. El Frontend: Astro (`src/pages/index.astro`)

En Astro, al ser un framework enfocado en contenido estático, manejaremos la lógica interactiva del micrófono directamente dentro de una etiqueta `<script>` estándar del lado del cliente. [1]

Crea o reemplaza tu página principal en tu proyecto Astro (`src/pages/index.astro`): [2]

```astro
---
// src/pages/index.astro
---

<html lang="es">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>Whisper Manos Libres - Astro</title>
	</head>
	<body class="bg-slate-950 text-white min-h-screen flex flex-col items-center justify-center p-4 font-sans">

		<div class="max-w-xl w-full bg-slate-900 border border-slate-800 rounded-3xl p-6 shadow-2xl text-center">
			<h1 class="text-2xl font-bold mb-2 text-emerald-400">🎙️ Astro + Faster-Whisper</h1>
			<p class="text-sm text-slate-400 mb-6">Modo manos libres. Grabación automática al detectar 3 segundos de silencio.</p>

			<!-- Botón de Control -->
			<button id="btnModo" class="w-40 h-40 bg-emerald-600 hover:bg-emerald-700 active:scale-95 rounded-full font-bold text-sm tracking-wide shadow-lg transition-all border-4 border-slate-800 uppercase cursor-pointer">
				Iniciar Escucha
			</button>

			<!-- Vúmetro Visual -->
			<div class="w-full bg-slate-950 h-3 rounded-full mt-6 overflow-hidden border border-slate-800">
				<div id="vumetro" class="bg-emerald-500 h-full w-0 transition-all duration-75"></div>
			</div>

			<p id="estado" class="text-slate-500 mt-3 text-xs font-semibold uppercase tracking-widest">Sistema Inactivo</p>

			<!-- Historial Acumulativo -->
			<div class="mt-6 text-left">
				<label class="text-xs font-bold text-slate-400 uppercase tracking-wider block mb-2">Historial de Transcripción:</label>
				<div id="historial" class="w-full bg-slate-950 p-4 rounded-2xl min-h-[180px] max-h-[300px] overflow-y-auto text-slate-200 border border-slate-800 font-mono text-sm leading-relaxed">
					<span class="text-slate-600 italic">Esperando que inicies la escucha...</span>
				</div>
			</div>
		</div>

		<script>
			let audioContext, analyser, microphone, scriptProcessor;
			let mediaRecorder;
			let audioChunks = [];
			let estaEscuchando = false;
			
			// Cambia esto por la IP de tu PC si vas a conectar el celular por Wi-Fi (ej: http://192.168.1.50:5000)
			const BACKEND_URL = "http://localhost:5000/transcribir"; 
			const UMBRAL_SILENCIO = -60; 
			const TIEMPO_SILENCIO_MS = 3000; 
			let inicioSilencio = null;

			const btnModo = document.getElementById('btnModo');
			const vumetro = document.getElementById('vumetro');
			const estado = document.getElementById('estado');
			const historial = document.getElementById('historial');

			btnModo.addEventListener('click', toggleModo);

			async function toggleModo() {
				if (!estaEscuchando) {
					await iniciarAudio();
				} else {
					detenerAudio();
				}
			}

			async function iniciarAudio() {
				try {
					const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
					estaEscuchando = true;
					
					btnModo.innerText = "Detener Escucha";
					btnModo.classList.remove('bg-emerald-600', 'hover:bg-emerald-700');
					btnModo.classList.add('bg-rose-600', 'hover:bg-rose-700');
					estado.innerText = "🟢 Escuchando voz...";

					audioContext = new (window.AudioContext || window.webkitAudioContext)();
					analyser = audioContext.createAnalyser();
					analyser.fftSize = 256;
					microphone = audioContext.createMediaStreamSource(stream);
					scriptProcessor = audioContext.createScriptProcessor(2048, 1, 1);

					microphone.connect(analyser);
					analyser.connect(scriptProcessor);
					scriptProcessor.connect(audioContext.destination);

					configurarGrabador(stream);

					scriptProcessor.onaudioprocess = () => {
						const array = new Float32Array(analyser.frequencyBinCount);
						analyser.getFloatTimeDomainData(array);
						
						let suma = 0;
						for (let i = 0; i < array.length; i++) {
							suma += array[i] * array[i];
						}
						let rms = Math.sqrt(suma / array.length);
						let db = 20 * Math.log10(rms);
						if (db === -Infinity) db = -100;

						let porcentajeVolumen = Math.max(0, Math.min(100, (db + 100) * 1.5));
						if (vumetro) vumetro.style.width = `${porcentajeVolumen}%`;

						if (db < UMBRAL_SILENCIO) {
							if (!inicioSilencio) inicioSilencio = Date.now();
							let transcurrido = Date.now() - inicioSilencio;
							
							if (transcurrido >= TIEMPO_SILENCIO_MS && mediaRecorder.state === "recording") {
								estado.innerText = "⏳ Silencio detectado. Procesando...";
								mediaRecorder.stop(); 
								inicioSilencio = null; 
							}
						} else {
							inicioSilencio = null;
							if(mediaRecorder.state === "recording") {
								estado.innerText = "🟢 Escuchando voz...";
							}
						}
					};

				} catch (err) {
					alert("Permiso de micrófono denegado o error de inicialización.");
					console.error(err);
				}
			}

			function configurarGrabador(stream) {
				mediaRecorder = new MediaRecorder(stream);
				audioChunks = [];

				mediaRecorder.ondataavailable = e => audioChunks.push(e.data);
				
				mediaRecorder.onstop = async () => {
					const audioBlob = new Blob(audioChunks, { type: 'audio/wav' });
					audioChunks = [];

					const formData = new FormData();
					formData.append('file', audioBlob, 'instantaneo.wav');

					try {
						const response = await fetch(BACKEND_URL, { method: 'POST', body: formData });
						const data = await response.json();
						
						if (data.texto && historial) {
							if (historial.innerHTML.includes("italic")) historial.innerHTML = "";
							historial.innerHTML += `<p style="margin-bottom: 8px; border-left: 2px solid #10b981; padding-left: 8px;">🗣️ ${data.texto}</p>`;
							historial.scrollTop = historial.scrollHeight;
						}
					} catch (err) {
						console.error("Error al conectar con Flask:", err);
					}

					if (estaEscuchando) {
						mediaRecorder.start();
						inicioSilencio = null;
					}
				};

				mediaRecorder.start();
			}

			function detenerAudio() {
				estaEscuchando = false;
				btnModo.innerText = "Iniciar Escucha";
				btnModo.classList.remove('bg-rose-600', 'hover:bg-rose-700');
				btnModo.classList.add('bg-emerald-600', 'hover:bg-emerald-700');
				estado.innerText = "Sistema Inactivo";
				if (vumetro) vumetro.style.width = "0%";

				if (mediaRecorder && mediaRecorder.state !== "inactive") mediaRecorder.stop();
				if (microphone) microphone.disconnect();
				if (scriptProcessor) scriptProcessor.disconnect();
				if (audioContext) audioContext.close();
			}
		</script>
	</body>
</html>
```

---

## Pasos para poner en marcha el ecosistema:

1. Levantar el Backend de Flask:  
    Asegúrate de tener instaladas las extensiones necesarias en tu entorno de miniconda (`whisper_env`) y arranca el script:
    
    ```bash
    pip install flask flask-cors
    python app.py
    ```
    
2. Levantar el Frontend de Astro:  
    En la carpeta de tu proyecto Astro, inicializa el servidor de desarrollo:
    
    ```bash
    npm run dev
    ```
    
    Astro abrirá tu web por defecto en `http://localhost:4321`. [3, 4]

---

💡 Para asegurar que tu celular o dispositivos externos se conecten al instante sin errores:

- ¿Quieres que te muestre cómo configurar la constante `BACKEND_URL` de Astro dinámicamente usando las variables de entorno (.env) del framework?
- ¿Te gustaría añadir en el script de Astro una validación visual que avise si el backend de Flask se encuentra caído o inaccesible antes de empezar a hablar?

  

[1] [https://www.youtube.com](https://www.youtube.com/watch?v=CtKSWjDH_Wg)

[2] [https://www.webreactiva.com](https://www.webreactiva.com/blog/tutorial-astro)

[3] [https://docs.astro.build](https://docs.astro.build/es/develop-and-build/)

[4] [https://johnserrano.co](https://johnserrano.co/blog/astro-tutorial-paso-a-paso-crea-tu-propia-pagina-web-facilmente)