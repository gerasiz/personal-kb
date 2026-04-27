---
title: "Conceptos asumidos / prerequisitos (2405.07987)"
arxiv: "2405.07987"
created: "2026-04-27"
---

# Conceptos asumidos (mini-intros + links)

## 1) Embeddings / representación latente
**Qué es:** vectores (o estructuras) internas que codifican propiedades de datos para que el modelo “trabaje” con ellos.  
**Por qué importa:** toda la hipótesis gira en torno a cómo se organizan estos espacios.

## 2) Representational Similarity Analysis (RSA) / CKA / SVCCA (familia de métricas)
**Qué es:** técnicas para comparar representaciones entre capas/modelos (p.ej., medir si dos spaces latentes son similares hasta transformaciones).  
**Por qué importa:** son herramientas típicas para afirmar “estos modelos convergen”.  
**Para profundizar (punto de entrada):**
- CKA (overview informal): https://arxiv.org/abs/1905.00414

## 3) Alineación de espacios (Procrustes, linear mapping)
**Qué es:** alinear embeddings de modelos distintos mediante mapeos (a veces lineales) para compararlos/transferir.

## 4) Modalidades (visión vs lenguaje) y embeddings multimodales
**Qué es:** modelos que generan espacios latentes para imágenes, texto, audio, etc.  
**Por qué importa:** el claim más fuerte del paper es la convergencia **cross-modal**.

## 5) Geometría de espacios métricos (distancia/similitud)
**Qué es:** la idea de que “cerca/lejos” en embedding space significa similitud semántica/estructural.  
**Por qué importa:** el paper habla de que distintos modelos “miden distancia” de manera más parecida con escala.

## 6) Inductive biases y presiones de optimización
**Qué es:** supuestos/constraints implícitos de arquitectura + entrenamiento que guían qué soluciones son más probables.  
**Por qué importa:** el paper discute “selective pressures” hacia un atractor representacional.

## 7) Convergencia vs diversidad de representaciones
**Qué es:** incluso si hay convergencia parcial, distintos modelos pueden mantener “idiosincrasias” útiles.  
**Por qué importa:** para no sobre-interpretar el claim de unificación.
