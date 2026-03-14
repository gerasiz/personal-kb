---
title: "Ontologies, Context Graphs, and Semantic Layers: What AI Actually Needs in 2026"
source: https://metadataweekly.substack.com/p/ontologies-context-graphs-and-semantic
author: Jessica Talisman
date_published: 2026-01-22
date_accessed: 2026-03-14
tags: [ontologies, context-graphs, semantic-layers, knowledge-representation, AI]
---

# Ontologías, grafos de contexto y capas semánticas — Resumen

**Fuente:** [Ontologies, Context Graphs, and Semantic Layers: What AI Actually Needs in 2026](https://metadataweekly.substack.com/p/ontologies-context-graphs-and-semantic)
**Autora:** Jessica Talisman · 22 enero 2026
**Fecha de consulta:** 14 marzo 2026

## Visión general

El artículo plantea que la infraestructura de datos diseñada para consumo humano (capas semánticas) difiere fundamentalmente de los sistemas construidos para razonamiento automático (ontologías y grafos de contexto). Con la IA como consumidor primario de datos, las organizaciones deben repensar sus prioridades arquitectónicas. Talisman argumenta que la definición de métricas y la ingeniería de ontologías son disciplinas distintas, y que la ventaja competitiva pertenecerá a quienes construyan infraestructura de conocimiento de dominio que soporte razonamiento por parte de máquinas. El artículo usa ejemplos concretos de bioinformática, salud y cadenas de suministro para ilustrar cómo distintos niveles de representación del conocimiento resuelven problemas distintos.

## Ideas principales e insights

### Capas semánticas como herramienta limitada

- Diseñadas para consumo humano vía herramientas de BI.
- Normalizan etiquetas y definiciones de métricas para consistencia en reportes.
- Basadas en configuraciones YAML que definen métricas y modelos dimensionales.
- Responden a la pregunta "¿qué es X?" pero dependen de SQL: son modelos de cálculos, no modelos de dominio.

### Ontologías como representación formal del conocimiento

- Usan estándares como OWL y RDF para definir clases, propiedades y relaciones.
- Habilitan inferencia lógica: permiten razonar sobre el dominio más allá de la agregación de datos.
- Requieren habilidades especializadas en ingeniería del conocimiento y lógica formal.
- Compromisos de construcción que pueden tomar años.

### Grafos de contexto como capa de razonamiento operacional

- Capturan justificaciones de decisiones y lógica de negocio, no solo relaciones de datos.
- Modelan procedimientos, ejecuciones, roles y estructuras de autoridad.
- Responden a la pregunta "¿por qué se permitió X?".
- Requieren elicitación sistemática de conocimiento tácito.

### La IA cambia las reglas del juego

- Los agentes de IA necesitan representación del conocimiento que va más allá de la gobernanza de métricas.
- La mayoría de organizaciones necesitan tanto capas semánticas como arquitecturas de conocimiento más ricas.
- Las industrias que más han avanzado (salud, ciencias de la vida) tratan las ontologías como competencia central.

## Distinciones clave

| Concepto | Propósito | Pregunta que responde | Consumidor principal |
|---|---|---|---|
| **Capa semántica** | Consistencia de métricas y gobernanza de reportes | ¿Qué es X? (valor/cálculo) | Humanos vía BI |
| **Ontología** | Representación formal del dominio con inferencia | ¿Qué es X, cómo se relaciona, qué es posible? | Máquinas y humanos |
| **Grafo de contexto** | Razonamiento operacional y trazabilidad de decisiones | ¿Por qué se permitió/ocurrió X? | Agentes de IA |

Las capas semánticas son modelos de cálculos; las ontologías son modelos de dominios; los grafos de contexto son modelos de decisiones y procedimientos. No compiten entre sí — operan en niveles distintos de abstracción.

## Ejemplos destacados

- **Gene Ontology:** define genes, procesos biológicos, funciones moleculares y componentes celulares para descubrimiento de relaciones genéticas.
- **SNOMED CT:** terminología clínica con 350,000+ conceptos; permite entender que "infarto de miocardio" y "ataque al corazón" son equivalentes.
- **Palantir Foundry:** arquitectura de grafos de contexto para análisis de inteligencia, cadena de suministro y detección de fraude financiero.
- **Siemens Microgrids:** representaciones de procedimientos que explican patrones de carga mediante especificaciones formales.

## Preguntas abiertas y seguimiento

- ¿Cómo se integran en la práctica las tres capas (semántica, ontológica, contextual) en una arquitectura unificada?
- ¿Qué herramientas actuales facilitan la construcción de ontologías de dominio sin requerir expertos en lógica formal?
- Explorar el trabajo de Kurt Cagle y William Inmon sobre grafos de contexto y trazas de decisión.
- Revisar la documentación de Anthropic sobre provisión de contexto para LLMs como caso de ingeniería de contexto aplicada.
- ¿Qué tan viable es adoptar ontologías en organizaciones sin tradición en ingeniería del conocimiento?
- Investigar estándares de intercambio semántico mencionados en el artículo.
