## Estructura de Datasets

Los datos utilizados para el entrenamiento y validación de los modelos (GAN y Difusión) se dividen en las siguientes categorías según su clasificación clínica:

* **`NSR__procesado.pkl`**: Este dataset contiene las señales correspondientes a **Latidos Normales** (Ritmo Sinusal). Se utiliza como base para que el modelo aprenda la morfología estándar del complejo P-QRS-T.
* **`pvc_limpio.pkl`**: Este dataset contiene las señales de **Contracción Ventricular Prematura (PVC)**. Estas señales presentan una morfología distinta, caracterizada por complejos QRS anchos y la ausencia de una onda P precedente, permitiendo al modelo generar anomalías específicas.

### Especificaciones de los Datos:
- **Longitud de señal:** 3600 puntos.
- **Normalización:** Rango [-1, 1].
- **Frecuencia de muestreo:** Original de los registros de procedencia.

---
