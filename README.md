---
title: Termovisor
emoji: 🔥
colorFrom: red
colorTo: yellow
sdk: docker
app_port: 7860
pinned: false
---

# Termovisor - Inspeccion Termografica de Fachadas con IA

Analisis de imagenes termicas FLIR (radiometricas) y DJI (R-JPEG de dron):
deteccion de puentes termicos, zonas frias y humedades, IA de isotermas
(K-means), segmentacion interactiva (SAM) y deteccion de huecos.

Ejecucion local: python main.py -> http://127.0.0.1:7860

Los detectores son exploratorios y no constituyen por si solos un
diagnostico de patologia. Toda deteccion debe verificarse visualmente.
