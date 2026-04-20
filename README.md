# DL Taller 2

Proyecto de clasificacion de imagenes con TensorFlow/Keras, organizado en notebooks para cubrir todo el flujo:

1. Descarga y comprension del dataset.
2. Entrenamiento de un primer modelo base.
3. Entrenamiento de un segundo modelo para comparar desempeno.

## Estructura del proyecto

```text
DL_Taller2/
├─ pyproject.toml
├─ README.md
├─ Code/
│  ├─ 01_descarga_comprension_dataset.ipynb
│  ├─ 02_primer_modelo.ipynb
│  └─ 03_segundo_modelo.ipynb
├─ data/
│  └─ artifacts/
│     ├─ class_mapping.json
│     ├─ dataset_summary.json
│     ├─ train_split.csv
│     └─ test_split.csv
└─ results/
	├─ model1_classification_report.csv
	├─ model1_confusion_matrix.npy
	├─ model1_history.csv
	├─ model1_metrics.csv
	├─ model1_model.keras
	├─ model2_classification_report.csv
	├─ model2_confusion_matrix.npy
	├─ model2_history.csv
	├─ model2_metrics.csv
	└─ model2_model.keras
```

## Requisitos

- Python 3.12 o superior.
- uv para gestionar entorno y dependencias.
- Credenciales de Kaggle para descargar el dataset (token kaggle.json).

Dependencias principales definidas en pyproject.toml:

- tensorflow
- scikit-learn
- pandas
- matplotlib
- seaborn
- kaggle y kagglehub
- pillow
- ipykernel

## Instalacion y uso con uv

### 1) Instalar uv

Si aun no tienes uv, puedes instalarlo con uno de estos comandos:

```bash
pip install uv
```

o en Windows PowerShell:

```powershell
winget install --id=astral-sh.uv -e
```

### 2) Sincronizar dependencias

Desde la raiz del proyecto:

```bash
uv sync
```

Esto crea/actualiza el entorno virtual y deja instaladas las librerias declaradas en pyproject.toml.

### 3) Abrir Jupyter con el entorno del proyecto

```bash
uv run jupyter lab
```

Tambien puedes usar VS Code y seleccionar el kernel asociado al entorno creado por uv.

## Configuracion de Kaggle

El notebook 01 usa Kaggle/KaggleHub para descargar datos. Antes de ejecutarlo:

1. Crea un API token en tu cuenta de Kaggle.
2. Guarda kaggle.json en la carpeta correspondiente:
	- Windows: %USERPROFILE%/.kaggle/kaggle.json
	- Linux/macOS: ~/.kaggle/kaggle.json
3. Asegura permisos de lectura adecuados del archivo.

Si el token no esta configurado, la descarga del dataset fallara.

## Notebooks: que hace cada uno

### 01_descarga_comprension_dataset.ipynb

Objetivo:
- Descargar y preparar el dataset.
- Explorar su estructura y distribucion.
- Generar artefactos base para entrenamiento reproducible.

Acciones principales:
- Descarga del dataset desde Kaggle.
- Construccion del mapeo de clases.
- Creacion de split train/test con semilla fija.
- Analisis exploratorio (conteos por clase, ejemplos, tamanos/modos de imagen, graficos).

Archivos que genera/actualiza:
- data/artifacts/class_mapping.json
- data/artifacts/dataset_summary.json
- data/artifacts/train_split.csv
- data/artifacts/test_split.csv

### 02_primer_modelo.ipynb

Objetivo:
- Entrenar y evaluar un primer modelo de referencia (baseline).

Acciones principales:
- Carga de train_split.csv y test_split.csv.
- Construccion de pipelines TensorFlow (tf.data) para entrenamiento y prueba.
- Definicion y entrenamiento de arquitectura inicial.
- Evaluacion con metricas y matriz de confusion.

Archivos que genera/actualiza:
- results/model1_model.keras
- results/model1_history.csv
- results/model1_metrics.csv
- results/model1_classification_report.csv
- results/model1_confusion_matrix.npy

### 03_segundo_modelo.ipynb

Objetivo:
- Entrenar una segunda configuracion/modelo y compararla contra el baseline.

Acciones principales:
- Reutiliza artifacts de data/artifacts.
- Entrena una alternativa de arquitectura o configuracion.
- Evalua con el mismo esquema de metricas para comparacion justa.

Archivos que genera/actualiza:
- results/model2_model.keras
- results/model2_history.csv
- results/model2_metrics.csv
- results/model2_classification_report.csv
- results/model2_confusion_matrix.npy

## Flujo recomendado de ejecucion

Ejecutar en este orden:

1. 01_descarga_comprension_dataset.ipynb
2. 02_primer_modelo.ipynb
3. 03_segundo_modelo.ipynb

Esto garantiza que los artifacts necesarios ya existan antes del entrenamiento.

## Como interpretar los resultados

- history.csv: evolucion por epoca (loss/accuracy en train y validacion).
- metrics.csv: resumen final de metricas globales.
- classification_report.csv: precision, recall y F1 por clase.
- confusion_matrix.npy: matriz de confusion para analisis detallado de errores.
- model.keras: modelo serializado listo para cargar e inferir.

## Reproducibilidad

- Los notebooks usan una semilla fija para reducir variaciones entre ejecuciones.
- Para comparaciones consistentes entre modelos, evita modificar el split entre corridas.

## Solucion de problemas comunes

- Error de descarga de Kaggle: validar ubicacion y formato de kaggle.json.
- Kernel sin paquetes: ejecutar uv sync y volver a seleccionar el kernel correcto.
- Entrenamiento lento: confirmar uso de GPU y revisar tamano de batch/epocas en el notebook.