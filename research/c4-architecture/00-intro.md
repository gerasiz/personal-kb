---
title: "Modelo C4 — Introducción"
date: 2026-03-15
tags: [arquitectura, c4, diagramas, documentación]
---

# Modelo C4 — Introducción

## ¿Qué es el modelo C4?

El modelo C4, creado por Simon Brown, es un enfoque fácil de aprender y orientado a desarrolladores para la creación de diagramas de arquitectura de software. Proporciona una forma estructurada de visualizar un sistema de software en distintos niveles de detalle, utilizando cuatro abstracciones jerárquicas:

1. **Sistema de software** (*Software System*) — el nivel más alto de abstracción.
2. **Contenedor** (*Container*) — unidades desplegables dentro de un sistema (aplicaciones web, APIs, bases de datos, etc.).
3. **Componente** (*Component*) — agrupaciones lógicas dentro de un contenedor.
4. **Código** (*Code*) — detalles de implementación (clases, interfaces, funciones, etc.).

## ¿Qué problemas resuelve?

- **Falta de un vocabulario común:** establece un conjunto consistente de abstracciones que todo el equipo puede usar.
- **Diagramas desordenados o ambiguos:** proporciona niveles claros de detalle para diferentes audiencias.
- **Documentación desactualizada:** al usar niveles jerárquicos, facilita mantener los diagramas más estables (los niveles altos cambian con poca frecuencia).
- **Comunicación deficiente entre equipos técnicos y no técnicos:** los niveles superiores están diseñados para audiencias no técnicas.

## ¿Cuándo usarlo?

- Al documentar la arquitectura de cualquier sistema de software.
- Al incorporar nuevos miembros al equipo (*onboarding*).
- Al comunicar decisiones arquitectónicas a partes interesadas (*stakeholders*).
- Al planificar cambios o evoluciones del sistema.

## Independencia de notación

El modelo C4 es **independiente de la notación**. No está ligado a UML, ArchiMate ni a ninguna herramienta específica. Puedes usar:

- Pizarras y papel.
- Herramientas de diagramado (draw.io, Miro, Lucidchart).
- Diagramas como código (Structurizr, PlantUML, Mermaid).

Lo importante son las abstracciones y los niveles, no la notación visual.

## Tipos de diagramas y cómo se relacionan

### Diagramas principales (jerárquicos)

Cada nivel "hace zoom" dentro del anterior:

| Nivel | Diagrama | Alcance | Audiencia |
|-------|----------|---------|-----------|
| 0 | [System Landscape](01-system-landscape.md) | Empresa / organización | Todos |
| 1 | [System Context](02-system-context.md) | Un sistema de software | Todos |
| 2 | [Container](03-container-diagram.md) | Un sistema de software | Técnica |
| 3 | [Component](04-component-diagram.md) | Un contenedor | Técnica |
| 4 | [Code](05-code-diagram.md) | Un componente | Técnica |

### Diagramas suplementarios

| Diagrama | Propósito |
|----------|-----------|
| [Dynamic](06-dynamic-diagram.md) | Mostrar interacciones en tiempo de ejecución |
| [Deployment](07-deployment-diagram.md) | Mostrar la topología de infraestructura |

## Flujo de trabajo recomendado

1. **Empezar por el System Context** — entender el panorama general.
2. **Crear el Container Diagram** — descomponer el sistema en unidades desplegables.
3. **Crear Component Diagrams solo si aportan valor** — no para todos los contenedores.
4. **Evitar Code Diagrams estáticos** — los IDEs pueden generarlos bajo demanda.
5. **Agregar Dynamic Diagrams** solo para flujos complejos o interesantes.
6. **Documentar Deployment Diagrams** para cada entorno relevante (producción, staging).
7. **Mantener un System Landscape** si la organización gestiona múltiples sistemas.

## Referencias

- [C4 Model — Sitio oficial](https://c4model.com)
- [Abstracciones del C4](https://c4model.com/abstractions)
- [Diagramas del C4](https://c4model.com/diagrams)
