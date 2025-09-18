# PMGPY
# Mini demo de `pgmpy` (diagnóstico respiratorio)

Este proyecto muestra un flujo de trabajo **completo** con [pgmpy](https://pgmpy.org) usando un ejemplo sencillo de **diagnóstico respiratorio**. Incluye:

- Definición de un **modelo de referencia** (red bayesiana) con CPDs tabulares.
- **Simulación** de datos sintéticos desde el modelo.
- **Aprendizaje de estructura** (Hill-Climb + BIC) a partir de datos.
- **Aprendizaje de parámetros** (Máxima Verosimilitud).
- **Inferencia exacta** (Variable Elimination) y **MAP**.
- **Muestreo** de nuevos datos desde el modelo aprendido.
- **Persistencia** del modelo (guardar/cargar en BIF).
- Comparación de **BIC** (estructura aprendida vs. verdadera) en validación.
- Ejemplo de **intervención** al estilo `do(X=x)` (causalidad) implementada con una utilidad simple.

> **Objetivo pedagógico:** que veas un flujo end-to-end con muchas piezas de `pgmpy` sin que el código sea abrumador.

---

## 1) Escenario del caso

Variables binarias (0/1):

- `S`: fuma (smoker)
- `V`: temporada viral (viral season)
- `D`: enfermedad respiratoria
- `F`: fiebre
- `C`: tos
- `X`: test positivo

Interpretación rápida:

- Fumar (`S=1`) y la temporada viral (`V=1`) **aumentan** el riesgo de enfermedad (`D=1`).
- Si hay enfermedad (`D=1`), sube la probabilidad de fiebre (`F=1`), tos (`C=1`) y test positivo (`X=1`).
- La tos (`C=1`) además **incrementa** la probabilidad de test positivo (`X=1`) por sensibilidad del test.

---

## 2) Requisitos e instalación

Crea un entorno virtual y instala dependencias:

```bash
# Windows PowerShell
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# macOS/Linux
python3 -m venv .venv
source .venv/bin/activate

pip install --upgrade pip wheel
pip install pgmpy pandas numpy

Estructura **verdadera** (DAG):

