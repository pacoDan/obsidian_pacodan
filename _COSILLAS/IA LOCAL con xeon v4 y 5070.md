Para correr Inteligencia Artificial (IA) local en una configuración basada en Xeon X99, estos son los detalles según las fuentes:

### Componentes necesarios

- **Procesador (CPU):** Intel Xeon **E5-2697 V4** (18 núcleos, 36 hilos, 2.3 GHz base, 3.6 GHz turbo y 45 MB de caché L3).
- **Tarjeta Gráfica (GPU):** Es el componente más importante. Se utiliza una **RTX 5070** con un mínimo de **12 GB de VRAM** (aunque 24 GB sería lo ideal).
- **Memoria RAM:** Se recomienda una cantidad considerable; en las pruebas se usaron **48 GB DDR4 a 2400 MHz**, pero se sugiere que tener hasta 128 GB ayudaría a evitar problemas cuando la VRAM se desborda.
- **Almacenamiento:** Es recomendable usar al menos **dos unidades de almacenamiento** (preferiblemente SSD), ya que los modelos de IA ("cerebros") pueden ocupar entre 20 y 50 GB o más.
- **Placa Base:** Una placa china **X99 (modelo Killiyu)**.
- **Software:** Windows (por simplicidad), controladores de NVIDIA actualizados y la aplicación **Stability Matrix** para gestionar las interfaces.

### Procedimiento de instalación y ejecución

1. **Instalación de la base:** Descargar y extraer **Stability Matrix** en la raíz de una unidad secundaria (como la unidad D:) para ahorrar espacio en la unidad principal.
2. **Configuración inicial:** Ejecutar la aplicación en **"Portable Mode"** para mantener todos los archivos ordenados en una sola carpeta.
3. **Selección de interfaces:** Instalar interfaces de usuario como **Stable Diffusion Web UI** y **ComfyUI**.
4. **Descarga de modelos:** Descargar los "checkpoints" o modelos (cerebros de la IA) según lo que se desee generar (realismo, anime, etc.).
5. **Organización de archivos:** Mover manualmente los archivos **"safe tensors"** a sus carpetas correspondientes (checkpoints, loras, etc.) dentro del árbol de directorios de ComfyUI si la aplicación no los reconoce automáticamente.
6. **Ejecución:** Iniciar la interfaz elegida desde Stability Matrix. La aplicación abrirá una terminal y proporcionará una **dirección IP local** que debes copiar en tu navegador para empezar a trabajar.

### Resultados y métricas

- **Consumo de recursos:** Al generar contenido complejo (como video), la IA consumió por completo los **12 GB de VRAM** de la tarjeta y recurrió a **24 GB adicionales de la memoria RAM** del sistema (un total de 36 GB de memoria de GPU compartida).
- **Carga de trabajo:** La CPU permanece prácticamente en segundo plano mientras que la **GPU realiza casi todo el trabajo** pesado.
- **Velocidad de procesamiento (Xeon E5-2697 V4 + RTX 5070):**
    - Tarea estándar: **10.47 segundos** por iteración.
    - Tarea pesada: **23.82 segundos** por iteración.
- **Comparativa:** En un sistema moderno con **Ryzen 9 (DDR5 6000MHz)**, los tiempos se reducen a **6 y 18 segundos** respectivamente.

### Conclusión

La plataforma **Xeon X99 sigue siendo muy válida** para correr IA local en la actualidad. Aunque un procesador moderno como el Ryzen 9 es más rápido, la **diferencia de rendimiento (de 10s a 6s)** no siempre justifica la enorme diferencia de precio entre ambas plataformas, ya que el factor determinante es tener una **tarjeta gráfica potente con suficiente VRAM**. Se recomienda no obsesionarse con tener el procesador más nuevo, sino invertir en una GPU de calidad y suficiente memoria RAM para gestionar el desbordamiento de datos.