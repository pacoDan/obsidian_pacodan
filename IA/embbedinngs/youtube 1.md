https://youtu.be/Tgtz-wkC-4E

### 1. **Word Vectors / Word2Vec (Embeddings de palabras)**

- **Objetivo:** Representar el significado de las palabras en un formato matemático continuo dentro de un espacio vectorial de baja dimensión. Esto busca superar la problemática de los vectores "one-hot" tradicionales (donde cada palabra es independiente y ortogonal, impidiendo que el sistema entienda que "hotel" y "motel" tienen significados similares).
- **Cómo se resuelve:** Se basa en la **semántica distribucional** ("conocerás una palabra por la compañía que mantiene"). El algoritmo Word2Vec recorre un gran cuerpo de texto (corpus) utilizando una ventana de contexto de tamaño fijo. Calcula la probabilidad de que una palabra del contexto aparezca cerca de una palabra central basándose en el producto punto de sus vectores. Para convertir estos números abstractos en probabilidades de 0 a 1, utiliza la función **softmax**. El sistema comienza con vectores aleatorios y, mediante **cálculo y la regla de la cadena**, calcula las derivadas de la función de pérdida (entropía cruzada negativa promedio) para ajustar los parámetros de los vectores mediante **descenso de gradiente** hasta minimizar el error.
- **Ejemplos en las fuentes:**
    - Representar palabras como _banking_ y _monetary_ con vectores que compartan signos similares en ciertas dimensiones para denotar su cercanía semántica.
    - Visualizar las agrupaciones semánticas en dos dimensiones (usando algoritmos de reducción como t-SNE) para demostrar cómo verbos similares (como _become_, _remain_ y _be_) o términos geográficos se agrupan en el mismo espacio.
    - El promedio de significados que toma una palabra polisémica como _star_ (estrella de Hollywood y estrella astronómica) o _bank_ (entidad financiera y orilla del río) en un modelo estático.

---

### 2. **Sentence Transformers (Modelos basados en BERT / MiniLM)**

- **Objetivo:** Obtener representaciones numéricas de tamaño fijo (embeddings) a partir de frases u oraciones completas de longitudes variables, preservando el contexto semántico exacto de las palabras dentro de la oración.
- **Cómo se resuelve:** A través de una red **Transformer** preentrenada que procesa los datos numéricamente. Utiliza un **mecanismo de atención** que analiza no solo las palabras individuales, sino la relación y el contexto de todas las palabras dentro de la frase completa. Al llamar a la función de codificación (`encode`), el modelo procesa la secuencia y devuelve un vector con dimensiones estandarizadas de forma independiente al largo del texto de entrada.
- **Ejemplos en las fuentes:**
    - El uso del modelo multilenguaje `Paraphrase Multilingual MiniLM L12 v2` de Hugging Face para generar vectores de 384 dimensiones a partir de frases en español.
    - Diferenciar semánticamente el significado de la palabra "banco" según su contexto: _"Debo ir al banco a retirar dinero"_ (entidad financiera) frente a _"Estoy sentado en el banco"_ (mueble para sentarse).

---

### 3. **Sistemas de Búsqueda y Respuesta Semántica (Information Retrieval & QA)**

- **Objetivo:** Transformar los motores de búsqueda tradicionales (basados en coincidencia exacta de palabras clave) en sistemas capaces de comprender una pregunta compleja, leer documentos y sintetizar una respuesta directa.
- **Cómo se resuelve:** Encadenando tres redes neuronales distintas:
    1. Una **red de recuperación (retrieval)** que busca pasajes similares a la consulta en una gran base de datos.
    2. Una **red de reordenamiento (reranking)** que refina el orden de relevancia de los pasajes encontrados.
    3. Una **red de lectura (reading)** que procesa la información de los pasajes y sintetiza la respuesta de salida.
- **Ejemplos en las fuentes:** Buscar en la web la respuesta a _"¿Cuándo salió el primer álbum de Kendrick Lamar?"_ y presentar la respuesta exacta extraída de los documentos, en lugar de una lista de enlaces con las palabras clave.

---

### 4. **Modelos de Generación de Texto / LLMs Autorregresivos (como gpt-2 / ChatGPT)**

- **Objetivo:** Generar texto coherente, fluido y contextualmente preciso a partir de una instrucción (prompt) dada por el usuario.
- **Cómo se resuelve:** Entrenando un modelo masivo que procesa el contexto previo paso a paso y calcula la probabilidad matemática de cuál es la palabra más viable para continuar la frase, generando **una palabra a la vez** de forma recursiva.
- **Ejemplos en las fuentes:**
    - La generación de una historia realista sobre materiales nucleares robados en Cincinnati a partir de una frase inicial usando GPT-2 (demostrando conocimiento implícito de geografía y de qué departamentos regulan dichos materiales).
    - La redacción de un correo electrónico formal de disculpa para un jefe utilizando Chat GPT, corrigiendo errores tipográficos en el prompt original del usuario.

---

### 5. **Modelos Fundacionales Multimodales (como DALL-E y GPT-4 Vision)**

- **Objetivo:** Operar de manera conjunta con múltiples tipos de señales (texto, imágenes, audio, código genético) bajo la misma arquitectura tecnológica de gran escala.
- **Cómo se resuelve:** Entrenando redes masivas capaces de alinear conceptos de diferentes modalidades (por ejemplo, emparejar texto con píxeles de imagen) para interpretar o generar contenido cruzado.
- **Ejemplos en las fuentes:**
    - Generar imágenes complejas (en estilos realistas, dibujos a lápiz o ilustraciones de novela gráfica) a partir de descripciones textuales detalladas, como _"un tren cruzando el puente Golden Gate con la bahía al fondo"_.
    - Interpretar imágenes cargadas por el usuario para explicar qué elementos resultan inusuales o graciosos en ellas.

---

### 6. **Similitud de Coseno (Cosine Similarity)**

- **Objetivo:** Comparar matemáticamente dos embeddings vectoriales para determinar qué tan parecidos son sus significados originales.
- **Cómo se resuelve:** Aplicando una métrica vectorial matemática (usualmente provista por bibliotecas como Scikit-learn) que mide el ángulo entre dos vectores en el espacio multidimensional. Devuelve un valor de entre -1 y 1 (donde 1 es coincidencia semántica idéntica y valores cercanos a 0 o negativos indican ausencia de relación semántica).
- **Ejemplos en las fuentes:**
    - Calcular una alta similitud (0.46) entre _"Voy al banco a retirar dinero"_ y _"El banco aprobó el préstamo"_, mientras se registra una similitud casi nula (0.05) al comparar _"Voy al banco a retirar dinero"_ con _"Nos sentamos en un banco del parque"_.

---

### 7. **Pandas (DataFrames) en Análisis Semántico**

- **Objetivo:** Estructurar matrices abstractas de similitud numérica para que los analistas puedan visualizarlas e interpretarlas con facilidad.
- **Cómo se resuelve:** Cargando las matrices de resultados numéricos en un objeto bidimensional (DataFrame), asignando las oraciones originales analizadas como índices de filas y cabeceras de columnas.
- **Ejemplos en las fuentes:** Creación de una tabla simétrica de similitudes de 4x4 etiquetada con las frases de prueba relacionadas con los diferentes significados de "banco".

---
---

A continuación se detallan las tecnologías explicadas en el material, organizadas según su **ejemplo de uso**, la **problemática** que abordan y la **solución** que aportan:

### 1. **Embeddings (Codificación Semántica de Texto)**

- **Ejemplo de uso:** Permitir que los grandes modelos de lenguaje comprendan el lenguaje humano y realizar tareas avanzadas como clasificación de textos, búsqueda semántica, agrupamiento de documentos por temáticas (clustering) o modelado de tópicos.
- **Problemática:** Las redes neuronales y los modelos de machine learning solo pueden procesar datos numéricamente, no texto directo. Además, los sistemas tradicionales de procesamiento de texto no logran distinguir de manera efectiva el significado de palabras que cambian según el contexto (por ejemplo, diferenciar "banco" como entidad financiera de "banco" como un mueble para sentarse).
- **Solución:** Los embeddings convierten el texto en vectores (listas de números) que **codifican la semántica o el significado** del texto. Utilizando mecanismos de atención, el modelo evalúa la relación de todas las palabras dentro de la frase completa. Esto genera representaciones numéricas totalmente distintas para la misma palabra cuando se utiliza en contextos diferentes, preservando el significado preciso de la oración.

---

### 2. **Redes Transformer (Modelos tipo BERT / Sentence Transformers)**

- **Ejemplo de uso:** Codificar textos o frases completas directamente a embeddings para su uso en clasificación y clustering temático, o servir como pilar en el desarrollo de herramientas de generación de texto conversacional (como ChatGPT).
- **Problemática:** Los textos que introducen los seres humanos tienen longitudes muy variables. Sin embargo, el procesamiento interno en las redes neuronales requiere estructuras numéricas consistentes y de formato estandarizado para operar eficientemente.
- **Solución:** El bloque Transformer toma textos de cualquier tamaño a la entrada y, mediante un método de codificación (como `encode`), genera vectores numéricos de **tamaño fijo** (por ejemplo, una dimensión de 384 elementos en modelos MiniLM) de manera independiente de la longitud original del texto.

---

### 3. **Hugging Face (Repositorios de Modelos Preentrenados)**

- **Ejemplo de uso:** Descargar de manera sencilla modelos de lenguaje optimizados y listos para usar, como el modelo multilenguaje `Paraphrase Multilingual MiniLM L12 v2`.
- **Problemática:** Diseñar, estructurar y entrenar un gran modelo de lenguaje desde cero es un proceso extremadamente complejo que requiere miles de millones de datos y un costo computacional masivo.
- **Solución:** Actúa como una plataforma y repositorio en línea que democratiza el acceso a modelos preentrenados de alta calidad. Permite a los desarrolladores importar el modelo de su elección directamente a su entorno de desarrollo con pocas líneas de código.

---

### 4. **Scikit-learn (Algoritmo de Similitud de Coseno)**

- **Ejemplo de uso:** Comparar numéricamente dos embeddings para calcular de forma matemática qué tan parecidas son las frases de origen a nivel semántico.
- **Problemática:** Un embedding es un vector compuesto de cientos de números decimales (como 384 dimensiones) que no significan nada intuitivo para un ser humano, lo que hace imposible evaluar la afinidad entre textos a simple vista.
- **Solución:** La función `cosine_similarity` implementa una operación vectorial matemática que mide el grado de coincidencia entre dos vectores de embeddings, retornando un valor entre -1 y 1. Si la métrica se acerca a 1 indica que los textos tienen significados muy similares (como _"Voy al banco a retirar dinero"_ y _"El banco aprobó el préstamo"_), mientras que valores cercanos a 0 o negativos indican nula o nula relación semántica.

---

### 5. **Pandas (DataFrames)**

- **Ejemplo de uso:** Organizar y etiquetar los resultados numéricos obtenidos al evaluar las similitudes semánticas de diferentes pares de oraciones.
- **Problemática:** Las funciones de comparación matemática suelen devolver matrices de números puros y abstractos (como arreglos de 4x4) que resultan engorrosos de relacionar visualmente con las frases originales evaluadas.
- **Solución:** Permite transformar la matriz de similitudes en una tabla estructurada (DataFrame) con encabezados de columna y etiquetas en las filas. Esto asocia cada puntuación numérica directamente con su frase de origen, simplificando la interpretación de los datos para los usuarios.

📊 ¿Te gustaría que cree un script práctico en Python para que puedas generar tus propios embeddings y calcular la similitud semántica entre las frases que tú elijas?

