---
title: "Conceptos asumidos / prerequisitos (2604.14228)"
arxiv: "2604.14228"
created: "2026-04-27"
---

# Conceptos asumidos (mini-intros + links)

> Objetivo: capturar conceptos que el paper da por sentados. Para cada uno: qué es, por qué importa en agentes, y por dónde profundizar.

## 1) Tool-calling / function-calling en LLMs
**Qué es:** usar el modelo para decidir invocaciones a herramientas (shell, HTTP, DB, etc.) y luego alimentar el resultado de vuelta al modelo.  
**Por qué importa:** convierte al LLM en un “control loop” que actúa en el mundo.  
**Para profundizar:**
- OpenAI: tool calling / function calling (docs generales): https://platform.openai.com/docs

## 2) ReAct (Reason + Act)
**Qué es:** patrón de prompting donde el modelo alterna razonamiento y acciones/herramientas.  
**Por qué importa:** es el “esqueleto mental” de muchos agentes (loop de pensamiento/acción/observación).  
**Para profundizar:**
- Paper ReAct (Google): https://arxiv.org/abs/2210.03629

## 3) Permissioning / human-in-the-loop
**Qué es:** mecanismos para que el humano autorice acciones (por tipo, riesgo, o contexto).  
**Por qué importa:** reduce riesgo y mantiene autoridad humana cuando el agente tiene herramientas potentes.

## 4) Context window, compaction y retrieval
**Qué es:** técnicas para manejar límites de contexto: resumir, “compacting” en capas, y recuperar información bajo demanda (RAG).  
**Por qué importa:** la mayoría de agentes se rompen por contexto (olvidos, drift, saturación) más que por “inteligencia”.  
**Para profundizar:**
- Concepto general de RAG: https://arxiv.org/abs/2005.11401

## 5) MCP (Model Context Protocol)
**Qué es:** un protocolo para conectar agentes/modelos con herramientas/recursos externos de forma estándar.  
**Por qué importa:** habilita un ecosistema de “capabilities” reutilizables sin acoplar todo al core.

## 6) Worktrees / aislamiento de cambios en repos
**Qué es:** trabajar en ramas/worktrees separados para evitar mezclar experimentos con el estado estable.  
**Por qué importa:** en agentes que editan código, reduce el blast radius y hace más fácil auditar/revertir.

## 7) Append-only session storage
**Qué es:** registrar sesiones como eventos/append logs (en vez de “estado mutable”).  
**Por qué importa:** auditoría, debugging, reproducibilidad y compacción/replay.

## 8) Gateway vs CLI deployment
**Qué es:** diferencias entre un agente como CLI local (usuario en terminal) vs un gateway siempre activo (multi-canal, multi-integración).  
**Por qué importa:** cambia el modelo de seguridad, la exposición de herramientas, y la observabilidad.
