Sí. Con esa aclaración, yo cambiaría el diseño anterior en un punto importante: **no necesitamos inventar un `SpeakerEncoder + AcousticModel + Vocoder` desde cero**. Para conseguir que la salida conserve la voz de la persona, conviene partir de un modelo de voice cloning/TTS ya entrenado y hacer fine-tuning sobre **el conjunto de pares `audioX + audioX.txt`**.

Para este caso, **XTTS-v2** es una base especialmente adecuada: soporta español, voice cloning, múltiples audios de referencia y fine-tuning; además trabaja a 24 kHz. La documentación oficial indica que el fine-tuning se centra en el entrenamiento del encoder GPT de XTTS. ([Coqui AI Docs](https://docs.coqui.ai/en/latest/models/xtts.html?utm_source=chatgpt.com "ⓍTTS - TTS 0.22.0 documentation"))

> Asumo que cuando decís `.egg` te referís a `.ogg`. Podemos soportar ambos formatos de entrada si realmente existen.

---

# 1. Objetivo exacto

Queremos conseguir esto:

```text
dataset/
│
├── audio1.mp3
├── audio1.txt
│
├── audio2.ogg
├── audio2.txt
│
├── audio3.mp3
├── audio3.txt
│
└── ...
```

Por ejemplo:

```text
audio1.mp3
```

contiene:

> "Hola, mi nombre es Carlos y este es un ejemplo."

y:

```text
audio1.txt
```

contiene exactamente:

```text
Hola, mi nombre es Carlos y este es un ejemplo.
```

Todos los audios son de **la misma persona**.

Después del fine-tuning queremos:

```text
"Hoy es un día excelente para aprender inteligencia artificial."
                         │
                         ▼
                  MODELO ENTRENADO
                         │
                         ▼
                  output.wav
```

donde `output.wav` sea pronunciado **con la voz de la persona del dataset**.

---

# 2. La arquitectura real

Con XTTS no necesitamos construir manualmente:

```text
Text Encoder
Speaker Encoder
Acoustic Model
Vocoder
```

porque XTTS ya tiene esos componentes y mecanismos de conditioning.

Conceptualmente tenemos:

```text
                  ┌──────────────────────┐
                  │      XTTS-v2         │
                  │   PREENTRENADO       │
                  └──────────┬───────────┘
                             │
                             ▼
                       FINE-TUNING
                             ▲
                             │
             ┌───────────────┴───────────────┐
             │                               │
          audio1.mp3                    audio1.txt
          audio2.ogg                    audio2.txt
          audio3.mp3                    audio3.txt
             │                               │
             └───────────────┬───────────────┘
                             ▼
                       MODELO ADAPTADO
                             │
                             ▼
                       VOZ DE PERSONA
```

El modelo ya tiene conocimiento general de:

- lenguaje;
    
- pronunciación;
    
- estructura del habla;
    
- acústica;
    
- generación de audio;
    
- representación de hablantes.
    

El fine-tuning adapta el comportamiento a nuestro dataset. La documentación de Coqui señala precisamente que el fine-tuning permite aprovechar un modelo preentrenado para converger más rápido y trabajar con datasets más pequeños. ([Coqui AI Docs](https://docs.coqui.ai/en/latest/finetuning.html?utm_source=chatgpt.com "Fine-tuning a 🐸 TTS model - TTS 0.22.0 documentation"))

---

# 3. Lo primero: nuestro Dataset Loader

No quiero que dependamos de `metadata.csv`, porque **tu convención de nombres ya es suficiente**.

Creamos:

```text
src/dataset/loader.py
```

```python
from pathlib import Path


AUDIO_EXTENSIONS = {
    ".wav",
    ".mp3",
    ".ogg",
    ".flac",
    ".m4a",
}


class VoiceDataset:

    def __init__(self, dataset_dir: str):

        self.dataset_dir = Path(dataset_dir)

        if not self.dataset_dir.exists():
            raise FileNotFoundError(
                f"Dataset no encontrado: {dataset_dir}"
            )

        self.samples = []

        self._load_samples()

    def _load_samples(self):

        for audio_path in sorted(
            self.dataset_dir.iterdir()
        ):

            if not audio_path.is_file():
                continue

            if audio_path.suffix.lower() not in AUDIO_EXTENSIONS:
                continue

            text_path = audio_path.with_suffix(".txt")

            if not text_path.exists():

                print(
                    f"[WARNING] "
                    f"Falta transcripción para: "
                    f"{audio_path.name}"
                )

                continue

            text = text_path.read_text(
                encoding="utf-8"
            ).strip()

            if not text:

                print(
                    f"[WARNING] "
                    f"Transcripción vacía: "
                    f"{text_path.name}"
                )

                continue

            self.samples.append({
                "audio": audio_path,
                "text": text,
            })

    def __len__(self):

        return len(self.samples)

    def __getitem__(self, index):

        return self.samples[index]
```

Ahora:

```python
dataset = VoiceDataset(
    "dataset"
)
```

encontrará automáticamente:

```text
audio1.mp3 + audio1.txt
audio2.ogg + audio2.txt
audio3.mp3 + audio3.txt
```

---

# 4. Validación

Antes de entrenar debemos ser estrictos.

Creamos:

```text
src/dataset/validator.py
```

```python
from pathlib import Path


AUDIO_EXTENSIONS = {
    ".wav",
    ".mp3",
    ".ogg",
    ".flac",
    ".m4a",
}


def validate_dataset(dataset_dir: str):

    dataset_dir = Path(dataset_dir)

    errors = []
    valid = 0

    for audio_path in sorted(
        dataset_dir.iterdir()
    ):

        if (
            not audio_path.is_file()
            or audio_path.suffix.lower()
            not in AUDIO_EXTENSIONS
        ):
            continue

        text_path = audio_path.with_suffix(".txt")

        # Debe existir la transcripción
        if not text_path.exists():

            errors.append(
                f"{audio_path.name}: "
                f"no existe {text_path.name}"
            )

            continue

        text = text_path.read_text(
            encoding="utf-8"
        ).strip()

        if not text:

            errors.append(
                f"{text_path.name}: "
                f"transcripción vacía"
            )

            continue

        valid += 1

    return valid, errors
```

Y:

```python
valid, errors = validate_dataset(
    "dataset"
)

print(f"Audios válidos: {valid}")

for error in errors:
    print(error)
```

---

# 5. Preprocesamiento

XTTS trabaja con sus propios parámetros de audio. En la receta oficial de XTTS-v2 aparecen, por ejemplo, audio de entrada a 22.05 kHz para ciertas etapas internas y salida a 24 kHz; por eso **no debemos elegir arbitrariamente el sample rate**, sino dejar que la configuración del modelo determine el procesamiento. ([GitHub](https://github.com/coqui-ai/TTS/blob/dev/recipes/ljspeech/xtts_v2/train_gpt_xtts.py?utm_source=chatgpt.com "TTS/recipes/ljspeech/xtts_v2/train_gpt_xtts.py at dev · coqui-ai/TTS · GitHub"))

Nuestro flujo será:

```text
MP3 / OGG / WAV
       │
       ▼
 Decodificación
       │
       ▼
 Mono
       │
       ▼
 Sample rate requerido
       │
       ▼
 Normalización
       │
       ▼
 Dataset XTTS
```

No conviene modificar permanentemente los archivos originales.

---

# 6. Convertir nuestra estructura a la estructura que espera XTTS

Acá hay una distinción importante.

Nuestro dataset:

```text
audio1.mp3
audio1.txt
```

es cómodo para nosotros.

Pero el trainer de XTTS utiliza un dataset formateado. La documentación oficial indica que el pipeline de fine-tuning hace una etapa de procesamiento del audio y genera los datos necesarios para entrenamiento. ([Coqui AI Docs](https://docs.coqui.ai/en/latest/models/xtts.html?utm_source=chatgpt.com "ⓍTTS - TTS 0.22.0 documentation"))

Por eso vamos a crear:

```text
dataset/
        │
        ▼
preprocess_dataset.py
        │
        ▼
processed/
├── wavs/
│   ├── audio1.wav
│   ├── audio2.wav
│   └── ...
│
├── metadata_train.csv
└── metadata_eval.csv
```

El nombre original se conserva:

```text
audio1.mp3
   ↓
audio1.wav
```

y:

```text
audio1.txt
```

se convierte en la entrada de metadata.

---

# 7. Preprocesamiento del dataset

```python
from pathlib import Path
import subprocess


AUDIO_EXTENSIONS = {
    ".wav",
    ".mp3",
    ".ogg",
    ".flac",
    ".m4a",
}


def convert_audio(
    source: Path,
    destination: Path
):

    destination.parent.mkdir(
        parents=True,
        exist_ok=True
    )

    command = [
        "ffmpeg",
        "-y",
        "-i",
        str(source),

        # mono
        "-ac",
        "1",

        # XTTS dataset de entrenamiento
        "-ar",
        "22050",

        # PCM
        "-sample_fmt",
        "s16",

        str(destination),
    ]

    subprocess.run(
        command,
        check=True,
        stdout=subprocess.DEVNULL,
        stderr=subprocess.PIPE,
    )
```

Entonces:

```python
convert_audio(
    Path("dataset/audio1.mp3"),
    Path("processed/wavs/audio1.wav")
)
```

genera:

```text
processed/wavs/audio1.wav
```

---

# 8. Generación de metadata

Podemos crear:

```text
processed/metadata.csv
```

con:

```text
audio1|Hola, mi nombre es Carlos...
audio2|Hoy vamos a estudiar...
audio3|Este sistema genera voz...
```

El código:

```python
def create_metadata(
    dataset_dir,
    output_file
):

    dataset_dir = Path(dataset_dir)

    lines = []

    for audio_path in sorted(
        dataset_dir.iterdir()
    ):

        if (
            not audio_path.is_file()
            or audio_path.suffix.lower()
            not in AUDIO_EXTENSIONS
        ):
            continue

        text_path = audio_path.with_suffix(".txt")

        if not text_path.exists():
            continue

        text = text_path.read_text(
            encoding="utf-8"
        ).strip()

        if not text:
            continue

        sample_id = audio_path.stem

        lines.append(
            f"{sample_id}|{text}"
        )

    Path(output_file).write_text(
        "\n".join(lines),
        encoding="utf-8"
    )
```

---

# 9. Train / Validation

No debemos entrenar con absolutamente todos los audios.

Por ejemplo:

```text
Dataset
   │
   ├── 90% train
   │
   └── 10% validation
```

Quedaría:

```text
processed/
├── wavs/
│
├── train.csv
└── eval.csv
```

Y algo importante: **no queremos que las frases del conjunto de validación sean simplemente duplicados de las de entrenamiento**.

---

# 10. El modelo

Ahora llegamos al `Modelo TTS`.

No haría esto:

```python
class MyTTS(nn.Module):
    ...
```

con una red LSTM inventada.

Porque tendríamos que entrenar:

- encoder;
    
- alignment;
    
- acoustic model;
    
- decoder;
    
- vocoder;
    
- speaker representation;
    

prácticamente desde cero.

En cambio:

```text
XTTS-v2
   │
   ├── GPT / text-to-speech component
   ├── conditioning
   ├── speaker representation
   ├── DVAE
   └── HiFi-GAN decoder
```

ya resuelve el problema general. El código de XTTS además indica que tiene un trainer dedicado y no implementa el entrenamiento mediante un `train_step()` genérico. ([GitHub](https://github.com/coqui-ai/TTS/blob/dev/TTS/tts/models/xtts.py?utm_source=chatgpt.com "TTS/TTS/tts/models/xtts.py at dev · coqui-ai/TTS · GitHub"))

---

# 11. Nuestro `TTSModel`

Podemos encapsular XTTS en:

```text
src/models/tts_model.py
```

```python
import torch

from TTS.tts.configs.xtts_config import XttsConfig
from TTS.tts.models.xtts import Xtts


class TTSModel:

    def __init__(
        self,
        config_path,
        checkpoint_path,
        vocab_path,
        dvae_path,
        device="cuda"
    ):

        self.device = device

        self.config = XttsConfig()

        self.config.load_json(
            config_path
        )

        self.model = Xtts.init_from_config(
            self.config
        )

        self.model.load_checkpoint(
            self.config,
            checkpoint_path=checkpoint_path,
            vocab_path=vocab_path,
            dvae_checkpoint=dvae_path,
        )

        self.model.to(device)

        self.model.eval()
```

La carga manual de configuración, checkpoint y vocabulario está contemplada en la implementación oficial de XTTS. ([Coqui AI Docs](https://docs.coqui.ai/en/latest/models/xtts.html?utm_source=chatgpt.com "ⓍTTS - TTS 0.22.0 documentation"))

---

# 12. ¿Dónde está la voz?

Acá está lo que responde exactamente a tu requisito.

XTTS permite proporcionar un **audio de referencia del hablante**.

Conceptualmente:

```text
reference_audio
       │
       ▼
 características de la voz
       │
       ▼
 XTTS conditioning
       │
       +
       │
       ▼
       TEXTO
       │
       ▼
     XTTS
       │
       ▼
 voz del hablante
```

Por ejemplo:

```python
outputs = model.inference(
    text="Este es un texto nuevo.",
    language="es",
    speaker_wav=[
        "dataset/audio1.wav"
    ]
)
```

El audio `audio1.wav` sirve para indicarle al modelo:

> "Quiero que esta generación tenga la identidad vocal de esta persona."

Y el fine-tuning hará que el modelo se adapte mucho mejor a esa persona.

---

# 13. Fine-tuning real de XTTS

Acá hay otra corrección importante respecto de mis respuestas anteriores.

**No debemos implementar manualmente un `speaker_loss` y un `mel_loss` sobre XTTS.**

XTTS tiene su propio esquema de entrenamiento.

La documentación oficial indica que el fine-tuning disponible está orientado al **GPT encoder de XTTS**, y proporciona tanto un demo de fine-tuning como una receta avanzada. ([Coqui AI Docs](https://docs.coqui.ai/en/latest/models/xtts.html?utm_source=chatgpt.com "ⓍTTS - TTS 0.22.0 documentation"))

La receta oficial utiliza:

```python
GPTArgs(...)
```

y:

```python
GPTTrainerConfig(...)
```

para entrenar el componente GPT de XTTS. ([GitHub](https://github.com/coqui-ai/TTS/blob/dev/recipes/ljspeech/xtts_v2/train_gpt_xtts.py?utm_source=chatgpt.com "TTS/recipes/ljspeech/xtts_v2/train_gpt_xtts.py at dev · coqui-ai/TTS · GitHub"))

---

# 14. Nuestro `FineTuningTrainer`

La estructura será:

```text
src/training/finetune.py
```

```python
class XTTSFineTuner:

    def __init__(
        self,
        dataset_dir,
        output_dir,
        pretrained_checkpoint,
        tokenizer,
        config
    ):

        self.dataset_dir = dataset_dir
        self.output_dir = output_dir

        self.pretrained_checkpoint = (
            pretrained_checkpoint
        )

        self.tokenizer = tokenizer
        self.config = config

    def prepare_dataset(self):

        # 1. Validar audioX + audioX.txt
        # 2. Convertir audio
        # 3. Generar metadata
        # 4. Separar train/eval
        pass

    def train(self):

        # Inicializar GPTTrainer
        # Cargar checkpoint XTTS
        # Ejecutar fine-tuning
        pass

    def save(self):

        # Guardar checkpoint final
        pass
```

Esto nos da una separación clara:

```text
TTSModel
    │
    └── representa el modelo XTTS

XTTSFineTuner
    │
    ├── prepara dataset
    ├── carga checkpoint
    ├── entrena
    └── guarda modelo
```

---

# 15. Parámetros de entrenamiento

La receta oficial de XTTS-v2 utiliza valores como:

```text
batch_size = 3
gradient_accumulation = 84
```

y señala que `batch_size * grad_accumulation` debería ser al menos 252 para una utilización eficiente en esa receta. Estos valores **no son universales**: dependen de la GPU y del dataset. ([GitHub](https://github.com/coqui-ai/TTS/blob/dev/recipes/ljspeech/xtts_v2/train_gpt_xtts.py?utm_source=chatgpt.com "TTS/recipes/ljspeech/xtts_v2/train_gpt_xtts.py at dev · coqui-ai/TTS · GitHub"))

También utiliza:

```text
max_conditioning_length = 6 s
min_conditioning_length = 3 s
max_wav_length ≈ 11.6 s
max_text_length = 200
```

como parte de la configuración de referencia de XTTS-v2. ([GitHub](https://github.com/coqui-ai/TTS/blob/dev/recipes/ljspeech/xtts_v2/train_gpt_xtts.py?utm_source=chatgpt.com "TTS/recipes/ljspeech/xtts_v2/train_gpt_xtts.py at dev · coqui-ai/TTS · GitHub"))

Por eso no fijaría todavía estos valores a ciegas. Primero sabemos:

```text
GPU
VRAM
cantidad de horas
duración promedio de audios
cantidad de muestras
idioma
```

y después ajustamos.

---

# 16. La configuración que vamos a manejar

Propongo:

```yaml
# configs/finetune.yaml

dataset:
  path: "./dataset"

output:
  path: "./models/persona"

model:
  language: "es"

training:
  epochs: 10
  batch_size: 2
  gradient_accumulation: 32

audio:
  sample_rate: 22050

validation:
  split: 0.10

hardware:
  device: "cuda"
```

Pero el `sample_rate`, batch y demás los vamos a alinear con la configuración exacta de XTTS y la GPU disponible.

---

# 17. Resultado del entrenamiento

El resultado debería quedar aislado:

```text
models/
└── persona/
    ├── checkpoint/
    │   ├── model.pth
    │   └── ...
    │
    ├── config.json
    ├── vocab.json
    └── ...
```

Y **no modificamos el modelo original**.

Tenemos:

```text
models/
├── xtts_base/
│
└── persona/
```

Esto es importante porque permite volver atrás.

---

# 18. Inferencia del modelo fine-tuneado

Después:

```text
                  TEXTO NUEVO
                       │
                       ▼
              MODELO FINE-TUNED
                       │
                       │
             referencia de voz
                       │
                       ▼
                     XTTS
                       │
                       ▼
                    output.wav
```

Por ejemplo:

```python
output = model.inference(
    text="""
        Hoy vamos a comenzar
        nuestra clase de inteligencia artificial.
    """,
    language="es",
    speaker_wav=[
        "processed/wavs/audio1.wav"
    ]
)
```

Y guardamos:

```python
import torchaudio

torchaudio.save(
    "output.wav",
    output["wav"].unsqueeze(0),
    24000
)
```

El punto importante es que `speaker_wav` no es el texto que se está generando. Es una **referencia de identidad vocal**.

---

# 19. ¿Podemos eliminar la referencia?

Sí, dependiendo de cómo utilicemos el modelo fine-tuneado.

Durante el desarrollo yo mantendría:

```text
text
 +
speaker_wav
```

porque permite controlar explícitamente el hablante.

Pero también podemos tener un archivo fijo:

```text
models/persona/reference.wav
```

y nuestra capa de inferencia automáticamente utiliza:

```python
speaker_wav = [
    "models/persona/reference.wav"
]
```

Entonces para quien consuma posteriormente el sistema será simplemente:

```text
texto
 ↓
modelo de PERSONA
 ↓
audio
```

El API REST no necesita conocer nada de speaker embeddings.

---

# 20. La arquitectura final

Así quedaría nuestro proyecto:

```text
voice-tts/
│
├── dataset/
│   │
│   ├── audio1.mp3
│   ├── audio1.txt
│   │
│   ├── audio2.ogg
│   ├── audio2.txt
│   │
│   ├── audio3.mp3
│   ├── audio3.txt
│   │
│   └── ...
│
├── processed/
│   ├── wavs/
│   ├── train.csv
│   └── eval.csv
│
├── src/
│   │
│   ├── dataset/
│   │   ├── loader.py
│   │   ├── validator.py
│   │   └── prepare.py
│   │
│   ├── models/
│   │   └── tts_model.py
│   │
│   ├── training/
│   │   └── finetune.py
│   │
│   └── inference/
│       └── synthesize.py
│
├── configs/
│   └── finetune.yaml
│
├── pretrained/
│   └── xtts-v2/
│
└── models/
    └── persona/
```

El flujo completo:

```text
             audio1.mp3 ─── audio1.txt
             audio2.ogg ─── audio2.txt
             audio3.mp3 ─── audio3.txt
                    │
                    ▼
             ┌──────────────┐
             │   VALIDATOR  │
             └──────┬───────┘
                    │
                    ▼
             ┌──────────────┐
             │ PREPROCESSOR │
             └──────┬───────┘
                    │
                    ▼
             ┌──────────────┐
             │ XTTS DATASET │
             └──────┬───────┘
                    │
                    ▼
          ┌─────────────────────┐
          │   XTTS-v2 BASE      │
          │   PRETRAINED        │
          └──────────┬──────────┘
                     │
                     ▼
              FINE-TUNING GPT
                     │
                     ▼
          ┌─────────────────────┐
          │ XTTS PERSONA MODEL  │
          └──────────┬──────────┘
                     │
                     ▼
              TEXTO NUEVO
                     │
                     ▼
             SPEAKER REFERENCE
                     │
                     ▼
                   XTTS
                     │
                     ▼
                output.wav
                     │
                     ▼
             VOZ DE LA PERSONA
```

## Punto clave

La diferencia fundamental con un TTS común es:

```text
TTS normal:

texto → voz genérica
```

mientras que nuestro sistema será:

```text
texto
  +
identidad vocal aprendida/referenciada
  ↓
XTTS fine-tuned
  ↓
voz de la misma persona
```

Y **la convención `audio1.mp3 ↔ audio1.txt` queda integrada directamente en nuestro `DatasetLoader`**, sin necesidad de que el usuario tenga que crear manualmente otro archivo de correspondencias.

La documentación oficial de XTTS confirma además que el modelo admite múltiples audios de referencia para clonación y que el fine-tuning tiene un flujo específico de entrenamiento del GPT encoder. ([Coqui AI Docs](https://docs.coqui.ai/en/latest/models/xtts.html?utm_source=chatgpt.com "ⓍTTS - TTS 0.22.0 documentation"))

**El siguiente desarrollo concreto que haría es `prepare_dataset.py` + `finetune.py` completos**, usando la receta oficial de XTTS-v2, de manera que podamos ejecutar algo como:

```bash
python prepare_dataset.py --dataset ./dataset
python finetune.py --dataset ./processed --output ./models/persona
```

y que esos comandos tomen automáticamente `audio1.mp3 + audio1.txt`, `audio2.ogg + audio2.txt`, etc., preparen el dataset y ejecuten el fine-tuning.

-----
-----

Sí. Lo que describís es un sistema de **Text-to-Speech (TTS) personalizado**, donde entrenás una red neuronal con audios de una persona y sus correspondientes transcripciones, y luego el modelo genera una voz sintética a partir de cualquier texto.

El flujo general es:

```text
                    ENTRENAMIENTO
┌─────────────────────────────────────────────────────┐
│                                                     │
│   audio_001.wav ──────► "Hola, ¿cómo estás?"      │
│   audio_002.wav ──────► "Bienvenido al sistema"   │
│   audio_003.wav ──────► "Este es un ejemplo..."   │
│          │                         │                │
│          └────────────┬────────────┘                │
│                       ▼                             │
│              Modelo TTS neuronal                   │
│                       │                             │
│                       ▼                             │
│                 modelo entrenado                  │
└───────────────────────┬─────────────────────────────┘
                        │
                        ▼
                    INFERENCIA

             Texto: "Hola, mundo"
                        │
                        ▼
                 Modelo TTS
                        │
                        ▼
              audio_generado.wav
```

## 1. ¿Qué tipo de modelo necesitás?

No conviene desarrollar una red neuronal TTS completamente desde cero como primera implementación. Existen arquitecturas ya desarrolladas que podés entrenar/fine-tunear con tu dataset.

Las opciones principales son:

|Modelo|Calidad|Entrenamiento|Voz personalizada|
|---|---|---|---|
|Tacotron 2|Buena|Complejo|Sí|
|FastSpeech 2|Muy buena|Medio/alto|Sí|
|VITS|Muy buena|Medio|Sí|
|XTTS|Excelente|Más sencillo|Sí|
|YourTTS|Muy buena|Medio|Sí|

Para un proyecto práctico, **VITS o XTTS** serían opciones muy interesantes.

Si el objetivo es específicamente:

> "Le doy 5/10/20 horas de grabaciones de una persona y quiero que reproduzca esa voz diciendo textos nuevos"

entonces yo plantearía el sistema alrededor de **VITS/XTTS**, en lugar de implementar Tacotron 2 desde cero.

---

# 2. Dataset

Necesitás pares:

```text
audio + transcripción
```

Por ejemplo:

```text
dataset/
├── wavs/
│   ├── 00001.wav
│   ├── 00002.wav
│   ├── 00003.wav
│   └── ...
│
└── metadata.csv
```

El `metadata.csv` podría ser:

```text
00001.wav|Hola, ¿cómo estás?
00002.wav|Bienvenido a nuestro sistema.
00003.wav|Hoy vamos a aprender machine learning.
00004.wav|Esta es una prueba de síntesis de voz.
```

Es muy importante que la transcripción corresponda **exactamente** con lo que se dice en el audio.

---

# 3. Calidad de los audios

Esto es incluso más importante de lo que parece.

Idealmente:

- una única persona;
    
- micrófono de buena calidad;
    
- poco ruido;
    
- sin música;
    
- sin eco;
    
- volumen relativamente constante;
    
- misma habitación;
    
- misma posición frente al micrófono;
    
- formato WAV;
    
- frecuencia de muestreo consistente, por ejemplo 22.05 kHz o 24 kHz;
    
- frases relativamente cortas.
    

Por ejemplo:

```text
audio 001
"Buenos días, bienvenidos a este curso."

audio 002
"En esta clase vamos a estudiar redes neuronales."

audio 003
"Primero vamos a preparar nuestro conjunto de datos."
```

No es recomendable tener:

```text
audio 001
"Hola... ehhh... bueno... este..."
```

porque el modelo puede aprender esos defectos.

---

# 4. Preprocesamiento

Antes del entrenamiento hay que procesar los audios.

Una pipeline típica sería:

```text
Audio original
      │
      ▼
Conversión a WAV
      │
      ▼
Resample
      │
      ▼
Normalización
      │
      ▼
Eliminación de silencios excesivos
      │
      ▼
Detección de ruido
      │
      ▼
Validación de transcripción
      │
      ▼
Dataset final
```

Por ejemplo:

```python
import librosa
import soundfile as sf

audio, sr = librosa.load(
    "audio_original.mp3",
    sr=24000,
    mono=True
)

sf.write(
    "audio_001.wav",
    audio,
    24000
)
```

---

# 5. ¿Qué aprende realmente la red?

Este punto es importante.

La red no aprende simplemente:

```text
texto → audio
```

internamente aprende representaciones relacionadas con:

```text
texto
 │
 ▼
fonemas / representación lingüística
 │
 ▼
características acústicas
 │
 ▼
prosodia
 │
 ▼
espectrograma / representación de audio
 │
 ▼
vocoder
 │
 ▼
onda de audio
```

Simplificando muchísimo:

```text
"Hola mundo"
      │
      ▼
Representación lingüística
      │
      ▼
Red neuronal
      │
      ▼
Representación acústica
      │
      ▼
Vocoder
      │
      ▼
.wav
```

---

# 6. Una arquitectura típica

Por ejemplo, conceptualmente:

```text
                    TEXTO
                      │
                      ▼
               Text Encoder
                      │
                      ▼
            Representación
             lingüística
                      │
                      ▼
              TTS Decoder
                      │
                      ▼
              Mel Spectrogram
                      │
                      ▼
                  Vocoder
                      │
                      ▼
                 AUDIO WAV
```

En modelos modernos como VITS, varias de estas partes están integradas en una arquitectura neuronal más compleja.

---

# 7. Entrenamiento

El dataset se divide, por ejemplo:

```text
80% → entrenamiento
10% → validación
10% → prueba
```

Durante el entrenamiento:

```text
                    TEXTO
                      │
                      ▼
                ┌───────────┐
                │   TTS     │
                │  Modelo   │
                └─────┬─────┘
                      │
                      ▼
             audio generado
                      │
                      │ comparación
                      ▼
             audio verdadero
                      │
                      ▼
                    Loss
                      │
                      ▼
              Backpropagation
                      │
                      ▼
              Actualizar pesos
```

Esto se repite miles de veces.

Por ejemplo:

```python
for epoch in range(num_epochs):

    for text, audio_real in dataloader:

        audio_pred = model(text)

        loss = criterion(
            audio_pred,
            audio_real
        )

        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
```

**Esto es conceptual**: un TTS real tiene pérdidas y etapas bastante más complejas que ese ejemplo.

---

# 8. Después del entrenamiento

Una vez entrenado el modelo:

```text
modelo/
├── config.json
├── model.pth
└── ...
```

Podés tener una API:

```text
POST /tts
```

con:

```json
{
    "text": "Hola, este es un ejemplo de voz sintética."
}
```

Y el servidor devuelve:

```text
audio.wav
```

Entonces la arquitectura completa podría ser:

```text
             ┌──────────────────┐
             │ Aplicación Web   │
             │ / Mobile / API   │
             └────────┬─────────┘
                      │
                POST /tts
                      │
                      ▼
             ┌──────────────────┐
             │ Backend Python   │
             │ FastAPI           │
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ Modelo TTS        │
             │ VITS / XTTS       │
             └────────┬─────────┘
                      │
                      ▼
             ┌──────────────────┐
             │ Vocoder / Decoder│
             └────────┬─────────┘
                      │
                      ▼
                  audio.wav
```

---

# 9. Ejemplo de API

Podrías implementarlo con **FastAPI**:

```python
from fastapi import FastAPI
from fastapi.responses import FileResponse
from pydantic import BaseModel

app = FastAPI()


class TTSRequest(BaseModel):
    text: str


@app.post("/tts")
def generate_audio(request: TTSRequest):

    text = request.text

    # Acá se carga/utiliza el modelo entrenado
    audio_path = generate_speech(text)

    return FileResponse(
        audio_path,
        media_type="audio/wav",
        filename="output.wav"
    )
```

Y desde un frontend:

```javascript
const response = await fetch("/tts", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({
        text: "Hola, este es un texto generado."
    })
});

const audioBlob = await response.blob();

const audioUrl = URL.createObjectURL(audioBlob);

const audio = new Audio(audioUrl);
audio.play();
```

Así tendrías:

```text
Usuario escribe texto
        │
        ▼
     Frontend
        │
        ▼
    POST /tts
        │
        ▼
     FastAPI
        │
        ▼
   Modelo neuronal
        │
        ▼
   Genera WAV
        │
        ▼
     Frontend
        │
        ▼
    Reproducir
```

---

# 10. ¿Cuántos audios necesitás?

Depende muchísimo de la arquitectura y del objetivo.

Como aproximación:

### Pocos minutos

Puede servir para sistemas de **voice cloning** que ya tienen un modelo preentrenado.

```text
5-30 minutos
```

podrían ser suficientes para experimentar con modelos preentrenados que soporten clonación.

### 1-3 horas

Ya podés construir un dataset bastante más interesante para fine-tuning.

### 5-20 horas

Dataset mucho más apropiado si querés entrenar/fine-tunear una voz con alta consistencia y controlar mejor pronunciación y prosodia.

### Desde cero

Entrenar un TTS completamente desde cero es considerablemente más exigente en datos, GPU y tiempo.

Por eso hay una diferencia fundamental entre:

```text
ENTRENAR DESDE CERO
```

y:

```text
MODELO PREENTRENADO
        +
FINE-TUNING
        +
TU VOZ
```

Para un proyecto real, normalmente elegiría la segunda alternativa.

---

# 11. GPU

El entrenamiento de TTS es bastante pesado.

Una arquitectura posible:

```text
Ubuntu
Python
PyTorch
CUDA
NVIDIA GPU
```

Por ejemplo:

```text
GPU
 │
 ├── VRAM
 │
 └── CUDA
       │
       ▼
     PyTorch
       │
       ▼
     VITS/XTTS
```

Para hacer pruebas, una GPU con alrededor de **12-24 GB de VRAM** puede ser un punto de partida razonable, aunque los requisitos concretos dependen mucho del modelo, batch size y resolución/configuración del audio.

Para inferencia, generalmente necesitás bastante menos que para entrenamiento.

---

# 12. Lo más interesante: fine-tuning

En vez de crear:

```text
TTS vacío
   │
   ▼
Entrenar durante muchísimo tiempo
```

podés hacer:

```text
                 Modelo TTS
                preentrenado
                     │
                     ▼
             Fine-tuning
                     │
          ┌──────────┴──────────┐
          │                     │
      tus audios          transcripciones
          │                     │
          └──────────┬──────────┘
                     ▼
               Modelo final
                     │
                     ▼
               Tu voz sintética
```

Esto reduce muchísimo la cantidad de datos y recursos necesarios.

---

# 13. Un proyecto completo podría quedar así

```text
voice-ai/
│
├── dataset/
│   ├── wavs/
│   │   ├── 00001.wav
│   │   ├── 00002.wav
│   │   └── ...
│   │
│   └── metadata.csv
│
├── preprocessing/
│   ├── clean_audio.py
│   ├── normalize.py
│   └── validate_dataset.py
│
├── training/
│   ├── train.py
│   ├── config.json
│   └── checkpoints/
│
├── inference/
│   └── synthesize.py
│
├── api/
│   └── main.py
│
├── models/
│   └── voice_model/
│
└── frontend/
    └── ...
```

Y el proceso sería:

```text
                 ┌─────────────────┐
                 │ AUDIOS + TEXTOS │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ Preprocesamiento│
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │      Dataset    │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ Fine-tuning TTS │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ Modelo de voz   │
                 └────────┬────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │       FastAPI         │
              └───────────┬───────────┘
                          │
                "Hola mundo"
                          │
                          ▼
                 ┌─────────────────┐
                 │ Modelo TTS      │
                 └────────┬────────┘
                          │
                          ▼
                     audio.wav
```

## 14. Qué te recomendaría para hacerlo de verdad

Si tu objetivo es **desarrollar el sistema vos mismo**, no empezaría implementando la red neuronal desde cero.

Haría este proyecto en cuatro etapas:

**Etapa 1 — Dataset**

Construir un programa que tome:

```text
audio + transcripción
```

y genere un dataset limpio y validado.

**Etapa 2 — Modelo**

Utilizar un modelo TTS neuronal preentrenado y hacer fine-tuning con tu dataset.

**Etapa 3 — Inferencia**

Crear:

```python
text -> modelo -> wav
```

y comprobar la calidad de la voz.

**Etapa 4 — API**

Exponer:

```http
POST /tts
```

para que cualquier aplicación pueda enviar texto y recibir el audio.

---

Hay además una decisión importante: **si querés que el modelo aprenda una única voz fija**, el problema es relativamente sencillo. Si querés que el sistema pueda hacer:

```text
texto + voz/persona → audio
```

y seleccionar entre distintas voces, entonces conviene diseñar el modelo con **speaker embeddings / speaker conditioning**, lo que cambia bastante la arquitectura.

Si querés, puedo llevar esto al siguiente nivel y darte **un proyecto completo en Python con PyTorch + VITS/XTTS**, incluyendo **estructura de carpetas, preparación del dataset, entrenamiento/fine-tuning, carga del modelo y una API `/tts` que reciba texto y devuelva el `.wav`**.