# Perceptrón Multicapa (MLP) para clasificación — Experimentación controlada

**Curso:** Deep Learning (DLY0100) — Sección 012V
**Evaluación:** Evaluación Parcial 1
**Integrantes:** Luis Muñoz, Aran Opazo
**Fecha:** Por definir

## Origen del proyecto

Este proyecto toma como punto de partida el MLP desarrollado por el mismo equipo en la asignatura Técnicas Avanzadas de Machine Learning (TLY1102), disponible en el repositorio [red_neuronal](https://github.com/LmunozL87/red_neuronal). Para esta evaluación se adaptó a un nuevo dataset y se amplió con experimentación controlada, comparación de funciones de activación y pérdida, regularización y selección del modelo basada exclusivamente en validación.

## Descripción del proyecto

Este proyecto implementa y evalúa un Perceptrón Multicapa (MLP) para clasificar imágenes de paisajes naturales y urbanos en 3 categorías. El objetivo no es solo obtener una métrica alta, sino comprender el flujo completo de trabajo en aprendizaje supervisado: carga de datos, preprocesamiento, diseño de arquitectura, comparación de configuraciones, entrenamiento, validación y análisis crítico de resultados, incluyendo las limitaciones propias de un MLP al aplicarse sobre imágenes.

## Objetivo del modelo

Entrenar una red MLP capaz de clasificar correctamente una imagen dentro de una de 3 categorías elegidas (de un total de 6 disponibles en el dataset original), comparando distintas arquitecturas (cantidad de capas y neuronas) y optimizadores (Adam vs SGD) para analizar su efecto en el desempeño.

## Dataset

- **Fuente:** [Intel Image Classification Dataset](https://www.kaggle.com/datasets/puneet6060/intel-image-classification) (Kaggle)
- **Contenido original:** ~25.000 imágenes, 150×150 px, 6 clases (buildings, forest, glacier, mountain, sea, street)
- **Variante del proyecto:** se trabajó con 3 de las 6 clases — `buildings`, `forest` y `glacier` — elegidas por combinar una categoría urbana con dos naturales que comparten elementos visuales con ella (cielo, horizonte), generando un desafío real de clasificación.
- **Distribución usada:** 5.494 imágenes de entrenamiento, 1.372 de validación, 1.464 de test, razonablemente balanceadas entre clases.

## Qué analiza el notebook

1. **Descarga del dataset** desde Kaggle.
2. **Comprensión y exploración:** distribución de clases, ejemplos visuales, variabilidad de tamaños de imagen.
3. **Preparación y preprocesamiento:** redimensionamiento a 64×64 px, normalización (rescale 1/255), generadores de train/validación/test.
4. **Diseño del Perceptrón Multicapa:** comparación de 3 arquitecturas candidatas, justificación de la elegida (4 capas ocultas × 10 neuronas, ReLU, salida softmax).
5. **Entrenamiento del modelo:** comparación de optimizadores (Adam vs SGD) y de tamaños de batch (32, 64, 128), con EarlyStopping.
6. **Evaluación del desempeño:** Accuracy, Precision, Recall, F1-score (macro) y matriz de confusión sobre el set de test; gap train-test como medida de sobreajuste.
7. **Análisis de resultados y errores:** ejemplos correctos e incorrectos, desglose de errores por clase, discusión de las limitaciones del MLP frente a imágenes.

## Resumen de resultados

- **Modelo final:** MLP de 4 capas ocultas × 10 neuronas (ReLU), optimizador **SGD** (`learning_rate=0.001`), `IMG_SIZE=64`, `BATCH_SIZE=32`.
- **Accuracy en test:** 78,5% (F1 macro: 0,782), superando a la alternativa entrenada con Adam (76,9%, F1 macro: 0,762).
- **Gap train-test:** 8,0 puntos porcentuales (86,5% train vs 78,5% test), señal de sobreajuste moderado.
- **Reproducibilidad:** se probaron tres semillas (SEED=1, 42 y 80); SEED=42 entregó el mejor resultado y es la que se reporta como configuración final.

## Principales hallazgos y limitaciones

- La clase **`buildings`** es consistentemente la más difícil de clasificar (F1 0,68, ~30% de error), por compartir fondo de cielo y a veces vegetación con `forest` y `glacier`.
- El MLP pierde información espacial al aplanar la imagen (`Flatten`), lo que constituye una limitación estructural para tareas de clasificación de imágenes, no resoluble solo ajustando hiperparámetros.
- Como siguiente paso se propone una **CNN pequeña**, con **data augmentation** y reporte sistemático de resultados sobre varias semillas.

## Estructura del repositorio

```
├── data/         # Dataset descargado desde Kaggle (intel_images/)
├── notebooks/    # Notebook principal del proyecto
├── models/       # Modelo final entrenado (modelo_mlp.keras)
├── images/       # Gráficos generados (distribución de clases, curvas, matrices de confusión, ejemplos)
├── requirements.txt
└── README.md
```

## Requisitos previos

Este notebook detecta automáticamente si se ejecuta en Google Colab o en un entorno local (VS Code, Jupyter), y ajusta la forma de obtener las credenciales de Kaggle según corresponda.

### Si se ejecuta en Google Colab

1. Crear cuenta en kaggle.com (si no tiene).
2. Ir a Settings → API → "Create New Token" → "Copiar API Token".
3. En Colab, ir al ícono de llave 🔑 (Secretos) en el panel izquierdo.
4. Agregar un nuevo secreto con nombre `KAGGLE_TOKEN` y como valor el token copiado.
5. Activar el acceso al notebook para ese secreto.
6. Ejecutar el notebook normalmente — la celda de descarga funcionará sola.

### Si se ejecuta localmente (VS Code / Jupyter)

1. Crear cuenta en kaggle.com (si no tiene).
2. Ir a Settings → API → "Create New Token" → "Copiar API Token".
3. En una terminal, ejecutar:
   ```
   mkdir -p ~/.kaggle
   echo "SU_TOKEN_AQUI" > ~/.kaggle/access_token
   chmod 600 ~/.kaggle/access_token
   ```
4. Instalar las dependencias del proyecto:
   ```
   pip install -r requirements.txt
   ```
5. Ejecutar el notebook normalmente — la celda de descarga funcionará sola.
