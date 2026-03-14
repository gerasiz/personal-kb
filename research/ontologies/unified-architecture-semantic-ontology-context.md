---
title: "Arquitectura unificada: integración de capas semánticas, ontológicas y contextuales"
date: 2026-03-14
tags: [ontologies, semantic-layers, context-graphs, architecture, integration]
status: draft
---

# Arquitectura unificada: integración de capas semánticas, ontológicas y contextuales

## Resumen ejecutivo

Este documento explora opciones de diseño para integrar tres niveles de representación del conocimiento —capas semánticas (métricas y BI), ontologías formales (representación de dominio) y grafos de contexto (razonamiento operacional)— en una arquitectura coherente. No existe una solución única: las organizaciones deben elegir patrones según su madurez, dominio y necesidades de razonamiento automático. Se presentan patrones arquitectónicos concretos, pasos de implementación y errores comunes a evitar.

El artículo de Jessica Talisman en *Metadata Weekly* ([Ontologies, Context Graphs, and Semantic Layers][talisman]) establece la distinción fundamental: las capas semánticas son modelos de cálculos, las ontologías son modelos de dominios y los grafos de contexto son modelos de decisiones. Integrarlos no significa fusionarlos, sino diseñar puentes explícitos entre niveles de abstracción.

---

## Contexto: tres capas, tres propósitos

| Capa | Propósito | Ejemplo de herramienta | Pregunta que responde |
|------|-----------|------------------------|-----------------------|
| **Semántica** | Consistencia de métricas y gobernanza de reportes | dbt Semantic Layer / MetricFlow | ¿Cuál es el valor de X? |
| **Ontológica** | Representación formal del dominio con inferencia | Protégé, Ontotext GraphDB, Stardog | ¿Qué es X, cómo se relaciona con Y? |
| **Contextual** | Trazabilidad de decisiones y razonamiento operacional | Grafos de contexto ad-hoc, Palantir Foundry | ¿Por qué se permitió/ocurrió X? |

Estas capas no compiten entre sí: operan en niveles distintos de abstracción y sirven a consumidores distintos (humanos vía BI, máquinas para inferencia, agentes de IA para decisiones).

---

## Patrones de integración

> **Nota:** Estos son patrones de diseño, no prescripciones universales. Cada organización debe evaluar cuál se adapta mejor a su contexto.

### Patrón 1: Federación con vocabulario compartido

**Concepto:** Cada capa mantiene su propio almacenamiento y herramientas, pero comparten un vocabulario controlado (taxonomía o tesauro) que actúa como lingua franca.

**Implementación práctica:**

1. Definir un vocabulario central usando SKOS o una taxonomía ligera en una herramienta como [PoolParty Semantic Suite][poolparty] o [WebProtégé][webprotege].
2. Configurar los modelos semánticos de [dbt][dbt-semantic] para que las dimensiones y entidades referencien los identificadores del vocabulario compartido (por ejemplo, vía convenciones de nomenclatura o metadatos en YAML).
3. Construir la ontología de dominio en [Protégé][protege] o [Stardog Designer][stardog] importando ese mismo vocabulario como base terminológica.
4. Los grafos de contexto referencian conceptos de la ontología al capturar decisiones y justificaciones.

**Ventajas:** Bajo acoplamiento; cada equipo mantiene autonomía sobre sus herramientas.
**Riesgos:** Deriva terminológica si no hay gobernanza activa del vocabulario compartido.

### Patrón 2: Ontología como capa de mediación

**Concepto:** La ontología de dominio actúa como modelo canónico. Las capas semántica y contextual se derivan o mapean desde ella.

**Implementación práctica:**

1. Construir la ontología de dominio primero, usando OWL 2 en [Protégé][protege] con validación en [Ontotext GraphDB][graphdb] para razonamiento.
2. Generar los modelos semánticos de dbt a partir de la ontología: las clases se convierten en entidades, las propiedades de datos en dimensiones y las propiedades agregables en medidas.
3. Los grafos de contexto instancian patrones procedimentales definidos en la ontología (por ejemplo, usando la Procedural Knowledge Ontology mencionada por Talisman).
4. Usar [Stardog][stardog] como capa de virtualización para federar consultas SPARQL sobre datos que residen en distintos sistemas.

**Ventajas:** Máxima coherencia semántica; la ontología es la fuente de verdad.
**Riesgos:** Requiere madurez en ingeniería del conocimiento; la ontología se convierte en cuello de botella si no se gestiona bien.

### Patrón 3: Integración incremental bottom-up

**Concepto:** Empezar con la capa semántica (que la mayoría de organizaciones ya tiene) y enriquecerla gradualmente con anotaciones ontológicas y contextuales.

**Implementación práctica:**

1. Partir de los [modelos semánticos de dbt][dbt-semantic] existentes como base.
2. Anotar las entidades y dimensiones con URIs que apunten a conceptos de ontologías públicas relevantes (por ejemplo, Schema.org, ontologías de industria).
3. Usar [OntoGPT][ontogpt] para extraer relaciones ontológicas a partir de documentación interna y poblar un grafo de conocimiento inicial.
4. Implementar grafos de contexto como extensiones del linaje de datos, capturando decisiones de transformación y sus justificaciones.
5. Almacenar el grafo resultante en [GraphDB][graphdb] o [Stardog][stardog] para habilitar consultas federadas.

**Ventajas:** Menor barrera de entrada; valor incremental en cada paso.
**Riesgos:** Sin un modelo ontológico explícito, las anotaciones pueden volverse inconsistentes.

---

## Pasos de implementación recomendados

Independientemente del patrón elegido, estos pasos facilitan la integración:

1. **Auditar el estado actual.** Identificar qué capas existen hoy (¿hay una capa semántica en dbt? ¿ontologías de dominio? ¿documentación de decisiones?).
2. **Elegir un dominio piloto acotado.** No intentar modelar toda la organización. Seleccionar un área donde las tres capas aporten valor claro (por ejemplo, cumplimiento regulatorio, cadena de suministro).
3. **Establecer un vocabulario compartido mínimo.** Empezar con 50-100 términos clave del dominio piloto. Herramientas como [PoolParty][poolparty] o [WebProtégé][webprotege] facilitan la colaboración.
4. **Definir puntos de integración explícitos.** Documentar cómo fluye la información entre capas: qué identificadores se comparten, qué transformaciones ocurren, quién es responsable de cada capa.
5. **Implementar validación cruzada.** Verificar que los términos usados en modelos semánticos de dbt correspondan a conceptos definidos en la ontología. Esto puede automatizarse con scripts o CI/CD.
6. **Iterar y expandir.** Añadir dominios, profundizar la ontología y enriquecer los grafos de contexto conforme el equipo gana experiencia.

---

## Errores comunes (pitfalls)

- **Confundir las capas.** Intentar que la capa semántica de dbt haga el trabajo de una ontología lleva a modelos YAML inmanejables que no soportan inferencia real.
- **Sobre-ingeniería ontológica.** Modelar con OWL Full desde el día uno cuando RDFS o SKOS serían suficientes. Empezar ligero y formalizar según la necesidad.
- **Ignorar la gobernanza.** Sin un proceso claro de mantenimiento del vocabulario compartido, las capas divergen rápidamente.
- **Subestimar el cambio cultural.** La ingeniería del conocimiento es una disciplina distinta a la ingeniería de datos. Requiere roles, habilidades y procesos diferentes.
- **Buscar la herramienta perfecta.** Ninguna herramienta cubre las tres capas nativamente. La integración es un problema de arquitectura, no de producto.

---

## Referencias

- [talisman]: Talisman, J. (2026). *Ontologies, Context Graphs, and Semantic Layers: What AI Actually Needs in 2026*. Metadata Weekly. https://metadataweekly.substack.com/p/ontologies-context-graphs-and-semantic
- [dbt-semantic]: dbt Labs. *Semantic models*. https://docs.getdbt.com/docs/build/semantic-models
- [protege]: Stanford University. *Protégé*. https://protege.stanford.edu/
- [webprotege]: Stanford University. *WebProtégé*. https://webprotege.stanford.edu/
- [graphdb]: Ontotext. *GraphDB*. https://www.ontotext.com/products/graphdb/
- [stardog]: Stardog. *Enterprise Knowledge Graph Platform*. https://www.stardog.com/
- [poolparty]: Semantic Web Company. *PoolParty Semantic Suite*. https://www.poolparty.biz/
- [ontogpt]: Monarch Initiative. *OntoGPT*. https://github.com/monarch-initiative/ontogpt

[talisman]: https://metadataweekly.substack.com/p/ontologies-context-graphs-and-semantic
[dbt-semantic]: https://docs.getdbt.com/docs/build/semantic-models
[protege]: https://protege.stanford.edu/
[webprotege]: https://webprotege.stanford.edu/
[graphdb]: https://www.ontotext.com/products/graphdb/
[stardog]: https://www.stardog.com/
[poolparty]: https://www.poolparty.biz/
[ontogpt]: https://github.com/monarch-initiative/ontogpt
