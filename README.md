# MoodMeter — Decodificador de Expresiones Faciales con IA

**MoodMeter** es un playground interactivo y didáctico de visión por computadora diseñado para analizar emociones y microexpresiones faciales en tiempo real.

Impulsado por **FastAPI** en el backend y la librería de aprendizaje profundo **DeepFace**, el sistema traduce capturas de video o fotografías locales en distribuciones de probabilidad emocional usando una **Red Neuronal Convolucional (CNN)**, y despliega un dashboard con estética **Glassmorphism en Modo Oscuro** junto con explicaciones conceptuales integradas.

---

## ¿Cómo Decodifica la IA las Emociones?

A diferencia de las computadoras, los seres humanos percibimos expresiones faciales de forma instantánea y subconsciente. Para que una máquina replique esto, se implementa un pipeline convolucional jerárquico paso a paso:

```text
[ Entrada: Webcam o Foto ]
          │
          ▼
[ Detector de Rostro (OpenCV) ] ──> Recorta el rostro y alinea ojos y boca.
          │
          ▼
[ Capas Convolucionales de la CNN ] ──> Extraen rasgos abstractos (comisura de labios, cejas).
          │
          ▼
[ Capa Completamente Conectada ] ──> Genera 7 puntuaciones brutas (logits) de emoción.
          │
          ▼
[ Función de Activación Softmax ] ──> Normaliza los logits en probabilidades (suman 100%).
          │
          ├───────────────────────────┐
          ▼                           ▼
[ Medidores Neón en Vivo ]     [ Curva Dinámica (Mood Timeline) ]
```

### La Matemática Detrás: Función Softmax
La última capa de la red de emociones arroja puntuaciones crudas ($z_i$) para cada emoción básica. Para traducir estas puntuaciones abstractas en porcentajes legibles que sumen exactamente **100%**, la red implementa la función matemática **Softmax**:

$$\sigma(\mathbf{z})_i = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$

* La función aplica un operador exponencial ($e^{z_i}$) a cada puntuación para asegurar que todos los valores sean estrictamente positivos.
* Luego, divide cada valor por la suma de todos los exponenciales ($\sum e^{z_j}$), convirtiendo los números en una distribución de probabilidad coherente.
* La probabilidad más alta indica la **emoción dominante** calculada por la IA.

---

## Estructura del Repositorio

La arquitectura del proyecto está optimizada para ser ligera, rápida y 100% libre de bases de datos persistentes:

```text
computerV/
├── app/
│   ├── main.py                 # Servidor FastAPI con pipeline de inferencia DeepFace
│   └── templates/
│       ├── index.html          # Frontend en HTML5 con Canvas y webcam
│       └── style.css           # Estilos en modo oscuro, glassmorphism y animaciones
├── requirements.txt            # Dependencias del proyecto en Python
├── start.sh                    # Script bash opcional para arranque en macOS/Linux
└── README.md                   # Documentación e instrucciones de ejecución
```

---

## Instalación y Ejecución

Sigue estos pasos para instalar y ejecutar **MoodMeter** de forma local:

### 1. Clonar o descargar el repositorio
```bash
git clone [https://github.com/AlejandroMolinaDev/computerV.git](https://github.com/AlejandroMolinaDev/computerV.git)
cd computerV
```

### 2. Crear y activar el entorno virtual
```bash
python -m venv venv
```

* **Windows (PowerShell):**
  ```powershell
  .\venv\Scripts\activate
  ```
* **macOS / Linux:**
  ```bash
  source venv/bin/activate
  ```

### 3. Instalar dependencias
```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Iniciar el servidor FastAPI
```bash
python -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```
*(En macOS o Linux también puedes ejecutar `./start.sh` para automatizar la creación del entorno, instalación y arranque).*

### 5. Abrir la aplicación en el navegador
Una vez que la terminal muestre `Application startup complete`, abre tu navegador en:

**[http://127.0.0.1:8000](http://127.0.0.1:8000)**

> **Primera ejecución:** La primera vez que presiones "Escanear Rostro", DeepFace descargará automáticamente los pesos preentrenados del clasificador de emociones (~20 MB). Este proceso se realiza una sola vez de forma interna y tardará solo unos segundos según tu velocidad de internet.

Para detener el servidor, presiona `Ctrl + C` en la terminal.

---

## Características del Dashboard

* **Captura Multidispositivo:** Si cuentas con cámara web activa, el visor escaneará tu rostro en vivo. Si no tienes cámara web, haz clic en **Archivo** para subir cualquier fotografía local y presiona **Escanear Rostro**.
* **Recuadro de Detección Facial:** Al completar el escaneo, se dibuja un recuadro sobre las coordenadas exactas del rostro detectado.
* **Gauges de Distribución:** Muestra barras de progreso que representan el porcentaje asignado por la red a cada una de las 7 emociones básicas en tiempo real.
* **Mood Timeline (Línea de Tiempo):** Un gráfico dinámico hecho con HTML5 Canvas que conecta los puntos de tus escaneos secuenciales e ilustra la transición emocional.

---

## Tecnologías Utilizadas
* **FastAPI** — API de alta velocidad en Python.
* **DeepFace** — Framework de análisis facial basado en redes neuronales.
* **OpenCV** — Localización y procesamiento espacial del rostro.
* **HTML5 / CSS3 / JavaScript (ES6)** — Interfaz web interactiva sin dependencias externas pesadas.