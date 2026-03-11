# Generación de Señales Sintéticas ECG 1D mediante GAN y Modelos de Difusión

Este repositorio contiene la implementación de modelos de aprendizaje profundo generativo para la creación de señales de Electrocardiograma (ECG) sintéticas de alta fidelidad, con una longitud de 3600 puntos (frecuencia de muestreo típica para análisis clínico).

## Características
- **Arquitectura GAN-LSTM:** Generador residual con capas densas y LSTM bidireccional para capturar dependencias temporales.
- **Métrica DTW (Dynamic Time Warping):** Validación morfológica inteligente para guardar el mejor modelo basado en la forma de la onda y no solo en la pérdida.
- **Pipeline de Normalización:** Preprocesamiento automático de señales para estabilidad numérica.
- **Módulo de Difusión (Opcional):** Implementación de DDPM para señales 1D.

## Arquitectura del Modelo (GAN)

Se encuentra en (`GAN - ECG.ipynb`)
 
### Generador
Utiliza una combinación de capas lineales para expandir el espacio latente ($z$) y bloques de **LSTM Bidireccionales** para refinar la estructura secuencial. Cuenta con una **conexión residual** final (`x1 + x2`) que facilita el aprendizaje de los complejos QRS.

### Discriminador
Una CNN 1D profunda con kernels de gran tamaño ($k=31$) diseñada para identificar irregularidades morfológicas y distinguir entre latidos reales y sintéticos.

##  Métricas de Evaluación
A diferencia de las imágenes, las señales ECG requieren una evaluación en el dominio del tiempo:
* **DTW Score:** Mide la similitud entre la señal generada y la real, permitiendo ligeros desplazamientos temporales.
* **BCE Loss:** Seguimiento del equilibrio entre el Generador y el Discriminador.

##  Arquitectura: Modelo de Difusión (DDPM 1D)

A diferencia de la GAN, que genera señales en un solo paso, el modelo de **Difusión Probabilística (DDPM)** aprende a revertir un proceso de degradación de ruido gaussiano a lo largo de un proceso de pasos de tiempo ($T$).

### 1. El Proceso Generativo
* **Forward Pass ($q$):** Se añade ruido gradualmente a una señal ECG real hasta que se convierte en ruido blanco puro.
* **Reverse Pass ($p_\theta$):** Una red neuronal aprende a predecir el ruido añadido en cada paso $t$ para reconstruir iterativamente la señal original desde el caos.



### 2. Estructura de la Red: U-Net 1D
Para procesar los 3600 puntos de la señal, se implementa una **U-Net 1D** optimizada con las siguientes capas:

* **Encoder (Downsampling):** Bloques de convolución residual (`ResNetBlock1D`) que reducen la resolución temporal mientras extraen características profundas.
* **Time Embeddings:** El paso de tiempo $t$ se inyecta en cada bloque mediante una **codificación senoidal** (Sinusoidal Positional Embeddings), permitiendo que la red sepa en qué etapa del proceso de "limpieza" se encuentra.
* **Self-Attention 1D:** Implementada en el nivel más bajo de la U-Net (bottleneck) para capturar dependencias globales de largo alcance, asegurando la coherencia entre la onda P inicial y la onda T final.
* **Decoder (Upsampling):** Capas de convolución transpuesta con **Skip Connections** hacia el encoder. Esto permite que la red recupere detalles finos de la morfología del complejo QRS que podrían haberse perdido en el submuestreo.



### 3. Ventajas Técnicas
- **Estabilidad de Convergencia:** No presenta las oscilaciones de pérdida típicas de la competencia Min-Max de las GANs.
- **Diversidad:** Al ser un modelo probabilístico, ofrece una mayor variedad de latidos (ritmos sinusales, bradicardias, etc.) evitando el *Mode Collapse*.

---

## Comparativa de Arquitecturas

| Característica | GAN (LSTM + Residual) | Difusión (U-Net 1D) |
| :--- | :--- | :--- |
| **Entrenamiento** | Rápido, pero propenso a inestabilidad | Más lento y costoso computacionalmente |
| **Generación** | Instantánea (1 paso) | Iterativa (Múltiples pasos de refinamiento) |
| **Pérdida (Loss)** | Adversarial (BCE) | Predictiva (MSE sobre el ruido) |
| **Morfología** | Capturada por celdas de memoria LSTM | Capturada por jerarquía convolucional y atención |
