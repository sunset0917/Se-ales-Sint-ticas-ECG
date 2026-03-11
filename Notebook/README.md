## Notebooks

La carpeta `notebooks/` contiene el proceso completo de desarrollo, desde el análisis inicial hasta la implementación de modelos generativos avanzados:

1. **`Exploración.ipynb` (Exploración y Preprocesamiento):**
   * Análisis exploratorio de datos (EDA) de señales normales y PVC.
   * Comparativa de técnicas de filtrado (pasa-banda, Wavelet, etc.) para la eliminación de artefactos.
   * Normalización de amplitud y segmentación a 3600 puntos.

2. **`GAN - ECG.ipynb` (Desarrollo GAN):**
   * Implementación del **Generador Residual** y el **Discriminador CNN-LSTM**.
   * Proceso de entrenamiento dinámico con monitoreo de pérdida Adversarial.
   * Uso de la métrica **DTW (Dynamic Time Warping)** para la selección automática de los mejores pesos.

3. **`Difusion.ipynb` (Modelos de Difusión):**
   * Desarrollo de la arquitectura **U-Net 1D** para síntesis de señales.
   * Implementación del proceso estocástico (Forward/Reverse Diffusion).
   * Generación iterativa de latidos sintéticos con alta diversidad morfológica.

---
