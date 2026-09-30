#### **1. Pestaña "CPU" (Procesador - Arriba a izquierda)**

- **Procesador:** Intel Core
- **Nombre de clave (Code):** Raptor Lake | **TDP:** 55.0 W
- **Zócalo (Socket):** Socket 1700 LGA
- **Tecnología (Litografía):** 10 nm | **Voltaje de núcleo:** 0.977 V
- **Especificación:** Genuine Intel(R) 0000 (ES) _(Muestra de ingeniería)_
- **Familia:** 6 | **Modelo:** 7 | **Stepping:** 1
- **Ext. Family:** 6 | **Ext. Model:** B7 | **Revision:** B0
- **Instrucciones compatibles:** MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, EM64T, VT-x, AES, AVX, AVX2, AVX-VNNI, FMA3, SHA
- **Relojes (P-Core #0):**
    - **Velocidad de núcleo:** 4800.00 MHz
    - **Multiplicador:** x 48.0 (4.0 - 48.0)
    - **Velocidad de bus:** 100.00 MHz
- **Caché:**
    - **L1 Data:** 8 x 48 KB + 16 x 32 KB
    - **L1 Inst:** 8 x 32 KB + 16 x 64 KB
    - **L2:** 8 x 2 MB + 4 x 4 MB
    - **L3:** 36 MBytes
- **Selección:** Procesador #1 | **Hilos:** 8 P-Cores + 16 E-Cores / **Total de Hilos:** 32

#### **2. Pestaña "Mainboard" (Placa Base - Arriba a derecha)**

- **Fabricante:** ASUSTeK COMPUTER INC.
- **Modelo:** ROG STRIX Z690-A GAMING WIFI D4 | **Rev:** 1.xx
- **Bus:** PCI-Express 4.0 (16.0 GT/s)
- **Chipset:** Intel Raptor Lake | **Rev:** 01
- **Southbridge:** Intel Z690 | **Rev:** 11
- **LPCIO:** Nuvoton NCT6798D-R
- **BIOS:**
    - **Marca:** American Megatrends Inc.
    - **Versión:** 3501
    - **Fecha:** 19/04/2024

#### **3. Pestaña "Memory" (Memoria - Abajo a izquierda)**

- **Tipo:** DDR4 | **Canales (Channels):** 2 x 64-bit
- **Tamaño:** 16 GBytes
- **Controlador de Memoria (Mem Controller):** 1000.0 MHz
- **Frecuencia NB (Northbridge):** 4400.0 MHz
- **Timings (Temporizadores):**
    - **DRAM Frequency:** 2000.0 MHz (FSB:DRAM = 1:20)
    - **CAS# Latency (CL):** 19.0 relojes
    - **RAS# to CAS# (tRCD):** 25 relojes
    - **RAS# Precharge (tRP):** 25 relojes
    - **Cycle Time (tRAS):** 45 relojes
    - **Row Refresh Cycle Time (tRC):** 70 relojes
    - **Command Rate (CR):** 1T

#### **4. Pestaña "Bench" (Prueba de rendimiento - Abajo a derecha)**

- **Procesador único (Single Thread):** 787.9 puntos
- **Multihilos (Multi Thread):** 14094.3 puntos
- **Relación Multihilos (Multi Thread Ratio):** 17.89
- **Versión del benchmark:** 17.01.64

### 📊 Informe Técnico: CPU y Placa Base (Hardware "Mutante" / MoDT)

Este informe detalla las características, compatibilidades y consideraciones técnicas del ensamblaje detectado en las capturas:

#### **1. Identificación del Hardware**

- **Procesador:** Intel Core i9-13950HX (Versión _Engineering Sample_ / Variante "Mutante" con sustrato adaptado de BGA a **LGA1700** conocido comercialmente como **Q1LP**). Aunque originalmente fue diseñado para portátiles de gama alta (como estaciones de trabajo y laptops de rendimiento extremo), cuenta con la misma arquitectura de silicio (_Raptor Lake_) que un procesador de escritorio de gama alta.
- **Configuración de Núcleos:** 24 núcleos en total (8 núcleos de alto rendimiento P-Cores y 16 núcleos de eficiencia E-Cores) con 32 hilos de procesamiento.
- **Placa Base:** **ASUS ROG STRIX Z690-A GAMING WIFI D4**. Es una placa base de gama media-alta orientada a entusiastas, basada en el chipset Intel Z690 y equipada con soporte para memorias **DDR4**.
- **BIOS:** Versión **3501** (con fecha del 19/04/2024), una versión clave para asegurar la inicialización correcta de microcódigos en procesadores de 13ª generación sobre placas de la serie 600.

#### **2. Rendimiento y Comportamiento Técnico**

- **Puntuación en Benchmarks:** En las pruebas de CPU-Z, muestra un rendimiento muy sólido en multihilos (~14,094 puntos), reflejando la potencia bruta de sus 24 núcleos.
- **Consumo Energético y Calor:** Al tratarse de un chip de portátiles forzado a trabajar en un entorno de escritorio con los límites liberados, es un componente altamente demandante de energía. Bajo cargas pesadas puede exigir flujos de corriente importantes, requiriendo un sistema de refrigeración de alta gama para evitar el estrangulamiento térmico (_thermal throttling_).
- **Ventaja de la plataforma DDR4:** El uso de memoria DDR4 en esta placa base ROG Strix facilita enormemente el proceso de entrenamiento de memoria (_memory training_) en comparación con las complejidades eléctricas que a veces presentan los controladores de memoria de estos chips mutantes al usar DDR5.