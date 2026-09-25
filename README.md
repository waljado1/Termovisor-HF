title: Termovisoremoji: 🔥colorFrom: redcolorTo: yellowsdk: dockerapp_port: 7860pinned: false
🔥 Termovisor — Inspección Termográfica de Fachadas con IA
Aplicación web para el análisis de imágenes térmicas FLIR (radiométricas)y DJI (R-JPEG de dron): detección de puentes térmicos, zonas frías yhumedades, IA de isotermas (K-means), segmentación interactiva (SAM) ydetección de huecos (ventanas/puertas) con evaluación de aislamiento.

⚠️ Los detectores son exploratorios y no constituyen por sí solos undiagnóstico de patología. Toda detección debe verificarse visualmente.

✨ Capacidades
Módulo	Descripción
📷 Radiometría	Matriz real de temperaturas (°C) por píxel — FLIR T5xx y DJI R-JPEG
🌉 Puentes térmicos	Gradientes Sobel en °C/m físicos (GSD real) + severidad CRÍTICO/ALTO/MEDIO/BAJO
💧 Zonas frías	Detección bajo media−k·σ con probabilidad de humedad
🤖 K-means isotermas	Clustering de zonas térmicas homogéneas + clúster frío (candidato humedad) con área en m²
🎯 SAM (MobileSAM)	Clic → segmento exacto → área m², T media, ΔT vs resto, clasificación
🪟 Huecos	YOLO-World: ventanas/puertas sobre RGB embebida → ΔT hueco vs pared
🌡️ Termómetro interactivo	Cursor en vivo + fijado por clic + medición 3×3
🗺️ Mapa interactivo	Heatmap Plotly con zoom y hover en °C reales
📥 Descargas	JSON, CSV, imágenes procesadas y ZIP por imagen
🚀 Ejecución
Local
python -m venv venv312.\venv312\Scripts\Activate.ps1pip install -r requirements.txtpython main.py# → http://127.0.0.1:7860
Requisito externo: exiftool instalado (Linux: apt install libimage-exiftool-perl;Windows: exiftool.exe junto a main.py).

Nube (Hugging Face Space)
SDK: Docker · puerto 7860 · CPU basic 16 GB
La sincronización GitHub → Space es automática (GitHub Actions, sync-hf.yml)
🏗️ Arquitectura
main.py            → arranque (rutas estáticas + ui.run, puerto PORT/7860)config.py          → rutas, límites, umbrales y colorescore/  metadata.py      → ExifTool (localizar + leer JSON EXIF/FLIR/DJI)  extractor.py     → motores FLIR (flirimageextractor) y DJI (SDK)  imaging.py       → normalización, colormaps, colorbar, podado  detectors.py     → puentes térmicos (Sobel °C/m) y zonas frías (k·σ)  ai_analysis.py   → IA: K-means, SAM, huecos (YOLO-World)  pipeline.py      → orquestador: procesar() y crear_zip()ui/  components.py    → mira (leer/fijar/actuar), tablas, mapa, termómetro, SAM  pages.py         → página principal (estado → handlers → UI).github/workflows/sync-hf.yml → sincronización automática con el Space
🔄 Flujo de datos
upload → metadata (exiftool) → motor (FLIR/DJI) → matriz °C       → detectores + IA → PNGs + JSON/CSV → UI web → descargas
⚙️ Parámetros ajustables (UI)
Parámetro	Default	Efecto
Percentil gradiente	85	Umbral de puentes térmicos
Área mín. puentes (px)	50	Filtra ruido
Área mín. zonas frías (px)	80	Filtra ruido
k desviaciones (frío)	0.8	Sensibilidad de zonas frías
k (K-means)	5	Nº de zonas isotermas
📸 Buenas prácticas de captura
Hora óptima: madrugada (04:00–06:00) o tras el atardecer — máximo contraste
Emisividad: 0.95 (materiales de construcción)
Formato: JPG radiométrico original de la cámara (no WhatsApp/capturas)
Condiciones: viento < 10 km/h, sin lluvia reciente, ΔT interior/exterior ≥ 10 °C para puentes térmicos
⚠️ Si la cámara marca fechas tipo 2000-xx-xx → reloj sin configurar
🧪 Verificación de entorno
python diagnostico.py
Todas las librerías deben reportar OK (PIL, cv2, numpy, pandas, plotly,matplotlib, sklearn, nicegui, flirimageextractor).

📄 Licencia
MIT

