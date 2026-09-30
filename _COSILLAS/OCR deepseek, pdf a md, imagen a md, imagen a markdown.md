Para tu caso, **probaría primero `deepseek-ocr`**: está específicamente orientado a OCR y su documentación de Ollama incluye directamente **“Convert the document to markdown”**. Ollama

- **DeepSeek-OCR** → mejor candidato para convertir escaneos → Markdown, tablas y estructura. Ollama  
    
- **Qwen2.5-VL 32B** → mejor alternativa si además necesitas interpretar **gráficos, diagramas y matemáticas**; Ollama destaca precisamente documentos, diagramas y gráficos. Son ~21 GB en Q4. Ollama+1  
    
- **Qwen2.5-VL 7B** → opción mucho más ligera (~6 GB) si tu hardware es limitado. Ollama  
    
- **Gemma 3 27B** → multimodal, 128K de contexto y buen razonamiento, pero no lo escogería específicamente como OCR. Ollama  
    

**Para tus PDFs de matemática**, mi arquitectura sería: **PDF → imágenes por página → DeepSeek-OCR → Markdown + LaTeX**, y usar **Qwen2.5-VL 32B** para páginas especialmente complejas con gráficos/diagramas.

Si me dices **RAM, GPU/VRAM y modelo de GPU**, puedo decirte exactamente qué versión instalar y darte un script que convierta automáticamente un PDF escaneado entero a `.md`.

---

Sí. Con esa especificación, **no hace falta montar un backend complejo**. Haría un CLI pequeño en Python con esta interfaz:

```
python ocr.py documento.pdf
python ocr.py
```

- Si recibe un PDF: procesa solamente ese PDF.
    
- Si no recibe argumentos: busca todos los `.pdf` de la raíz del proyecto.
    
- Convierte cada página a PNG.
    
- Envía cada imagen a `deepseek-ocr`.
    
- Le pide explícitamente **Markdown**, preservando fórmulas en LaTeX.
    
- Concatena las páginas en orden.
    
- Produce `documento.md`.
    
- Mantendría las imágenes temporales fuera del proyecto o las eliminaría después.
    

Esto encaja especialmente bien con DeepSeek-OCR porque Ollama documenta explícitamente el uso de `<|grounding|>Convert the document to markdown.` para convertir una imagen de documento a Markdown. El modelo oficial ocupa unos 6.7 GB en su distribución de Ollama y tiene entrada de texto + imagen. Ollama+1

### Estructura

```
proyecto/
├── ocr.py
├── documento1.pdf
├── documento2.pdf
├── documento1.md       # generado
└── documento2.md       # generado
```

### Dependencias

```
pip install ollama pymupdf
ollama pull deepseek-ocr
```

Ollama requiere actualmente una versión suficientemente reciente para DeepSeek-OCR; la página oficial indica **Ollama 0.13.0 o posterior**. Ollama

### `ocr.py`

```py
from pathlib import Path
import sys
import tempfile

import fitz  # PyMuPDF
import ollama


MODEL = "deepseek-ocr"

PROMPT = r"""
<|grounding|>Convert the document to markdown.

Instructions:
- Extract all visible text accurately.
- Preserve the original document structure.
- Use Markdown headings, paragraphs, lists and tables.
- Preserve mathematical notation using LaTeX.
- Use $...$ for inline mathematics.
- Use $$...$$ for displayed mathematics.
- Preserve equations as accurately as possible.
- Preserve symbols, subscripts, superscripts and Greek letters.
- Do not explain or summarize the document.
- Do not invent missing content.
- Preserve diagrams, graphs and figures using a suitable Markdown
  representation when possible.
- If a figure contains mathematical notation, transcribe it using LaTeX.
- Return ONLY the resulting Markdown.
"""


def pdf_to_images(pdf_path: Path, output_dir: Path):
    """Render every PDF page as a PNG."""
    document = fitz.open(pdf_path)

    image_paths = []

    for page_number, page in enumerate(document, start=1):
        # 300 DPI approximately.
        matrix = fitz.Matrix(300 / 72, 300 / 72)

        pixmap = page.get_pixmap(
            matrix=matrix,
            alpha=False,
        )

        image_path = output_dir / f"page_{page_number:04d}.png"
        pixmap.save(image_path)

        image_paths.append(image_path)

    document.close()

    return image_paths


def ocr_image(image_path: Path) -> str:
    """Send one image to Ollama."""
    response = ollama.chat(
        model=MODEL,
        messages=[
            {
                "role": "user",
                "content": PROMPT,
                "images": [str(image_path)],
            }
        ],
    )

    return response.message.content.strip()


def process_pdf(pdf_path: Path):
    output_path = pdf_path.with_suffix(".md")

    print(f"\nProcesando: {pdf_path.name}")
    print(f"Salida:     {output_path.name}")

    with tempfile.TemporaryDirectory(prefix="ocr_") as temp:
        temp_dir = Path(temp)

        image_paths = pdf_to_images(
            pdf_path,
            temp_dir,
        )

        pages = []

        total = len(image_paths)

        for index, image_path in enumerate(image_paths, start=1):
            print(f"  Página {index}/{total}...", end=" ", flush=True)

            try:
                markdown = ocr_image(image_path)

                pages.append(
                    f"<!-- Página {index} -->\n\n"
                    f"{markdown}"
                )

                print("OK")

            except Exception as exc:
                print(f"ERROR: {exc}")

                pages.append(
                    f"<!-- Página {index} -->\n\n"
                    f"> [ERROR OCR: {exc}]"
                )

    output_path.write_text(
        "\n\n---\n\n".join(pages) + "\n",
        encoding="utf-8",
    )

    print(f"✓ Generado: {output_path}")


def find_pdfs():
    """Return PDFs from the project root."""
    return sorted(
        p for p in Path.cwd().iterdir()
        if p.is_file()
        and p.suffix.lower() == ".pdf"
    )


def main():
    if len(sys.argv) > 1:
        pdfs = [Path(sys.argv[1])]

        if not pdfs[0].exists():
            print(f"ERROR: no existe: {pdfs[0]}")
            sys.exit(1)

        if pdfs[0].suffix.lower() != ".pdf":
            print("ERROR: el archivo debe ser un PDF.")
            sys.exit(1)

    else:
        pdfs = find_pdfs()

        if not pdfs:
            print("No se encontraron archivos PDF en la raíz.")
            sys.exit(0)

    for pdf in pdfs:
        process_pdf(pdf)


if __name__ == "__main__":
    main()
```

## Uso

Un archivo específico:

```
python ocr.py apuntes.pdf
```

Resultado:

```
apuntes.pdf
apuntes.md
```

Sin argumento:

```
python ocr.py
```

Procesará:

```
./apuntes1.pdf
./apuntes2.pdf
./libro.pdf
```

y generará:

```
./apuntes1.md
./apuntes2.md
./libro.md
```

## Una modificación que considero importante

Para **tus PDFs de matemática**, yo no le pediría simplemente `"OCR this image"`.

El prompt debe forzar una salida orientada a **documentación matemática**, por ejemplo:

```
## Definición

Sea $G = (V,E)$ un grafo dirigido.

El grado de entrada de un vértice $v$ se define como:

$$
\deg^-(v) =
|\{u \in V : (u,v) \in E\}|
$$

Por lo tanto, para el siguiente grafo...

| Vértice | Grado |
|---|---:|
| $v_1$ | 2 |
| $v_2$ | 3 |
| $v_3$ | 1 |
```

Eso permite posteriormente renderizarlo con **MathJax/KaTeX**, convertirlo a PDF con LaTeX, utilizarlo en Obsidian, MkDocs, Quarto, Jupyter, etc.

### Y hay una segunda cuestión importante

No intentaría que **DeepSeek-OCR dibuje nuevamente un gráfico complejo en Markdown**. Para gráficos matemáticos, conviene tener una estrategia de fallback:

```
                    PDF
                     │
                     ▼
              renderizar página
                     │
                     ▼
              DeepSeek-OCR
                     │
             ┌───────┴────────┐
             │                │
          texto             gráfico
             │                │
             ▼                ▼
         Markdown        descripción/
          + LaTeX        representación
```

Por ejemplo, si la página contiene un gráfico de una función, podríamos conservarlo como:

```
![Gráfico de la función](assets/page-004-figure-01.png)

La función representada es:

$$
f(x) = x^2 - 2x + 1
$$
```

Eso es **mucho más robusto** que obligar al LLM a reconstruir visualmente el gráfico.

Además, DeepSeek-OCR tiene una capacidad explícita de analizar figuras (`Parse the figure`) además de la conversión a Markdown documentada por Ollama. Ollama

Si quieres llevarlo un paso más allá, la siguiente versión que haría es **`ocr.py` + extracción automática de figuras + LaTeX + reanudación si el proceso se interrumpe**, de manera que un libro de 500 páginas no tenga que empezar nuevamente desde la página 1 si falla Ollama en la página 327.



---
---


Sí. Con **RTX 3060 8 GB VRAM + 16 GB RAM**, tu estrategia de **PDF → imagen → OCR página por página** tiene mucho sentido, especialmente para PDFs escaneados de ~1000 páginas.

Yo ajustaría el diseño anterior en algunos puntos:

- **No cargar nunca el PDF completo en memoria.**
    
- Renderizar **una sola página a la vez**.
    
- Mandar esa imagen a Ollama.
    
- Guardar inmediatamente el resultado de esa página.
    
- Si el proceso se interrumpe, **reanudar desde la última página procesada**.
    
- No generar PNG a 300 DPI indiscriminadamente: con 8 GB VRAM puede ser contraproducente. Para documentos normales probaría **200–250 DPI** y subiría a 300 DPI sólo si la letra pequeña lo necesita.
    
- Mantener las páginas individuales temporalmente o, mejor aún, permitir conservarlas opcionalmente para inspección.
    
- Hacer que el modelo entregue **Markdown + LaTeX**, no una descripción genérica del documento.
    

### Con tu hardware

Para empezar probaría:

```
ollama pull deepseek-ocr
```

y comprobaría:

```
ollama run deepseek-ocr
```

DeepSeek-OCR es particularmente interesante aquí porque no estamos intentando utilizar un VLM generalista como OCR improvisado: está diseñado específicamente para **OCR/document parsing**, y Ollama muestra precisamente el flujo de convertir una imagen de documento a Markdown. DeepSeek-OCR en Ollama

Para **8 GB de VRAM**, no intentaría empezar con Qwen2.5-VL 32B. Es demasiado grande para que tenga sentido como primera opción en esa máquina. Un VLM más pequeño puede servir como segundo paso para las páginas problemáticas, pero DeepSeek-OCR es la opción que probaría primero.

### Y cambiaría una cosa importante del código anterior

Para 1000 páginas, no conviene hacer:

```
PDF
 ↓
1000 imágenes
 ↓
1000 OCR
 ↓
concatenar
```

sino:

```
PDF
 │
 ├── página 1 → imagen → OCR → guardar
 ├── página 2 → imagen → OCR → guardar
 ├── página 3 → imagen → OCR → guardar
 │
 ...
 │
 └── página 1000 → imagen → OCR → guardar
```

Así tienes un **checkpoint natural**.

Por ejemplo:

```
output/
└── libro/
    ├── pages/
    │   ├── 0001.md
    │   ├── 0002.md
    │   ├── 0003.md
    │   └── ...
    └── libro.md
```

Si se corta en la página 643:

```
python ocr.py libro.pdf
```

detecta que:

```
0001.md ... 0643.md
```

ya existen y continúa en:

```
0644.md
```

Eso es **muy importante con libros de 1000 páginas**.

### También conservaría información de página

El Markdown final podría quedar:

```
# Libro

<!-- page: 1 -->

Contenido de la página...

---

<!-- page: 2 -->

Contenido de la página...

---

<!-- page: 3 -->

Contenido de la página...
```

Pero internamente conservaría:

```
pages/
├── 0001.md
├── 0002.md
├── 0003.md
...
```

De esa forma puedes posteriormente volver a procesar únicamente una página que salió mal.

### Sobre tu observación de MarkItDown

Sí, hay una diferencia conceptual importante.

Para un PDF **digital**, tiene mucho sentido:

```
PDF → extracción de texto → Markdown
```

Pero para un PDF que básicamente es:

```
PDF
 └── imagen escaneada
```

la representación que interesa al modelo es la **información visual de la página**:

```
PDF
 ↓
render
 ↓
imagen
 ↓
modelo multimodal/OCR
 ↓
Markdown + LaTeX
```

Esto es especialmente relevante en tu caso porque no solamente quieres texto. Tienes:

- fórmulas;
    
- símbolos matemáticos;
    
- tablas;
    
- diagramas;
    
- grafos;
    
- expresiones de álgebra;
    
- notación de matemática discreta;
    
- subíndices/superíndices;
    
- fracciones;
    
- matrices;
    
- posiblemente gráficos.
    

Ahí el OCR puramente textual puede perder información que visualmente sí está presente.

**Yo construiría el backend pensando desde el principio en que la unidad de procesamiento es la página, no el PDF.**

Y para una colección grande incluso añadiría un pequeño `manifest.json`:

```
{
  "source": "matematica_discreta.pdf",
  "pages": 1000,
  "completed": 643,
  "model": "deepseek-ocr",
  "dpi": 250,
  "status": "processing"
}
```

Así puedes saber exactamente qué pasó sin tener que inspeccionar el Markdown.

Si quieres, puedo darte ahora **la versión completa y robusta de `ocr.py` para esos 8 GB de VRAM**, incluyendo **reanudar automáticamente, páginas individuales, DPI configurable, progreso, manejo de errores, timeout de Ollama y concatenación final**, manteniendo el backend lo más sencillo posible.



