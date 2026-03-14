---
title: "Mini-curso de Ontologías: Introducción y Plan de Módulos"
date: 2026-03-14
status: in-progress
tags: [ontologías, rdf, rdfs, owl, web-semántica, curso]
---

# Mini-curso de Ontologías

## ¿Qué es una ontología y por qué importa?

Una **ontología** es una forma estructurada de describir los conceptos de un dominio y las relaciones entre ellos. Piensa en ella como un "vocabulario compartido con reglas": no solo defines qué cosas existen (personas, libros, ciudades…), sino cómo se relacionan y qué restricciones aplican.

¿Por qué dedicarle tiempo?

- **Interoperabilidad**: cuando dos sistemas usan la misma ontología, pueden intercambiar datos sin ambigüedad.
- **Razonamiento automático**: un software puede deducir hechos nuevos a partir de los que ya tienes.
- **Datos enlazados (Linked Data)**: la Web Semántica se apoya en ontologías para conectar conjuntos de datos dispersos.
- **IA y LLMs**: las ontologías proporcionan estructura y contexto que los modelos de lenguaje pueden aprovechar para extraer, validar y generar conocimiento.

Este mini-curso va paso a paso: empezamos con la pieza más básica (el *triple* RDF), subimos a vocabularios (RDFS) y llegamos a ontologías expresivas (OWL).

---

## 1. RDF — La base: triples, IRIs, literales y grafos

**RDF** (Resource Description Framework) es el modelo de datos fundamental. Todo se expresa como **triples**:

```
sujeto  predicado  objeto .
```

| Componente | ¿Qué es? | Ejemplo |
|---|---|---|
| **Sujeto** | El recurso del que hablamos | `<http://ejemplo.org/personas/ana>` |
| **Predicado** | La propiedad o relación | `<http://ejemplo.org/vocab#nombre>` |
| **Objeto** | El valor (otro recurso o un literal) | `"Ana García"` |

### Conceptos clave

- **IRI** (Internationalized Resource Identifier): identificador único global. Como una URL, pero para cualquier recurso. Ejemplo: `<http://ejemplo.org/personas/ana>`.
- **Literal**: un valor de dato (texto, número, fecha). Puede tener un tipo (`"42"^^xsd:integer`) o un idioma (`"gato"@es`).
- **Grafo**: un conjunto de triples. Puedes tener múltiples grafos con nombre dentro de un *dataset*.

### Ejemplo mínimo en Turtle

```turtle
@prefix ej: <http://ejemplo.org/> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .

ej:ana  foaf:name     "Ana García" ;
        foaf:knows    ej:luis .

ej:luis foaf:name     "Luis Pérez" .
```

Aquí tenemos tres triples: Ana tiene un nombre, Ana conoce a Luis, y Luis tiene un nombre.

---

## 2. Turtle — Sintaxis amigable para humanos

**Turtle** (Terse RDF Triple Language) es la forma más legible de escribir RDF. Puntos clave:

| Elemento | Sintaxis | Ejemplo |
|---|---|---|
| Prefijo | `@prefix alias: <IRI> .` | `@prefix foaf: <http://xmlns.com/foaf/0.1/> .` |
| Triple completo | `sujeto predicado objeto .` | `ej:ana foaf:name "Ana" .` |
| Mismo sujeto, otro predicado | `;` | `ej:ana foaf:name "Ana" ; foaf:age 30 .` |
| Mismo sujeto y predicado, otro objeto | `,` | `ej:ana foaf:knows ej:luis , ej:marta .` |
| Nodo en blanco | `[ ]` | `[ foaf:name "Anónimo" ]` |
| Comentario | `#` | `# Esto es un comentario` |

### Ejemplo con un poco más de estructura

```turtle
@prefix ej:   <http://ejemplo.org/> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

ej:ana
    a               foaf:Person ;       # 'a' es atajo para rdf:type
    foaf:name       "Ana García" ;
    foaf:age        30 ;                 # literal entero (xsd:integer)
    foaf:knows      ej:luis , ej:marta .

ej:luis
    a               foaf:Person ;
    foaf:name       "Luis Pérez"@es .    # literal con etiqueta de idioma
```

---

## 3. RDFS — Vocabularios con clase

**RDFS** (RDF Schema) añade una capa de vocabulario sobre RDF. Con RDFS puedes decir cosas como "Persona es una clase" o "conoce es una propiedad cuyo dominio es Persona".

### Elementos principales

| Término | Significado | Ejemplo de uso |
|---|---|---|
| `rdfs:Class` | Define una clase | `ej:Animal a rdfs:Class .` |
| `rdfs:subClassOf` | Jerarquía de clases | `ej:Perro rdfs:subClassOf ej:Animal .` |
| `rdfs:domain` | Clase válida para el sujeto de una propiedad | `ej:dueñoDe rdfs:domain ej:Persona .` |
| `rdfs:range` | Clase o tipo válido para el objeto | `ej:dueñoDe rdfs:range ej:Animal .` |
| `rdfs:label` | Nombre legible para humanos | `ej:Perro rdfs:label "Perro"@es .` |
| `rdfs:comment` | Descripción libre | `ej:Perro rdfs:comment "Un animal doméstico" .` |

### Ejemplo: jerarquía de animales

```turtle
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix ej:   <http://ejemplo.org/> .

# Clases
ej:Animal   a rdfs:Class ;
            rdfs:label "Animal"@es .

ej:Perro    a rdfs:Class ;
            rdfs:subClassOf ej:Animal ;
            rdfs:label "Perro"@es .

ej:Persona  a rdfs:Class ;
            rdfs:label "Persona"@es .

# Propiedad
ej:dueñoDe  a rdf:Property ;
            rdfs:domain ej:Persona ;
            rdfs:range  ej:Animal ;
            rdfs:label  "es dueño de"@es .

# Instancias
ej:ana      a ej:Persona ;
            ej:dueñoDe ej:firulais .

ej:firulais a ej:Perro .
```

Un razonador RDFS puede deducir que `ej:firulais` también es un `ej:Animal` (porque `Perro` es subclase de `Animal`).

---

## 4. OWL — Ontologías expresivas

**OWL** (Web Ontology Language) lleva las cosas mucho más allá. Permite expresar restricciones, equivalencias y disyunciones que RDFS no puede.

### Elementos principales

| Concepto OWL | ¿Para qué sirve? | Ejemplo rápido |
|---|---|---|
| `owl:Class` | Clase (similar a `rdfs:Class`, pero con más poder) | `ej:Mamífero a owl:Class .` |
| `owl:ObjectProperty` | Propiedad que conecta individuos | `ej:dueñoDe a owl:ObjectProperty .` |
| `owl:DatatypeProperty` | Propiedad con valor literal | `ej:edad a owl:DatatypeProperty .` |
| `owl:equivalentClass` | Dos clases son la misma | `ej:Humano owl:equivalentClass ej:Persona .` |
| `owl:disjointWith` | Dos clases no comparten individuos | `ej:Gato owl:disjointWith ej:Perro .` |
| `owl:Restriction` | Restricciones sobre propiedades | Ver ejemplo abajo |
| `owl:someValuesFrom` | "Debe tener al menos un valor de…" | Restricción existencial |
| `owl:allValuesFrom` | "Todos los valores deben ser de…" | Restricción universal |

### Ejemplo: ontología de mascotas con OWL

```turtle
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix ej:   <http://ejemplo.org/> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

# Clases
ej:Animal   a owl:Class .
ej:Perro    a owl:Class ; rdfs:subClassOf ej:Animal .
ej:Gato     a owl:Class ; rdfs:subClassOf ej:Animal .
ej:Persona  a owl:Class .

# Perros y gatos son disjuntos: nada puede ser ambos
ej:Perro    owl:disjointWith ej:Gato .

# Propiedad
ej:tieneMascota  a owl:ObjectProperty ;
                 rdfs:domain ej:Persona ;
                 rdfs:range  ej:Animal .

# DueñoDePerro = Persona que tiene al menos un Perro
ej:DueñoDePerro  a owl:Class ;
    owl:equivalentClass [
        a owl:Restriction ;
        owl:onProperty ej:tieneMascota ;
        owl:someValuesFrom ej:Perro
    ] .

# Instancias
ej:ana  a ej:Persona ;
        ej:tieneMascota ej:firulais .

ej:firulais a ej:Perro .
```

Un razonador OWL puede deducir que `ej:ana` es un `ej:DueñoDePerro` porque tiene una mascota que es `Perro`.

---

## 5. Supuesto de Mundo Abierto y Razonamiento

### Supuesto de Mundo Abierto (Open World Assumption - OWA)

A diferencia de una base de datos SQL (donde lo que no está es falso), en OWL **lo que no se dice no se asume falso: simplemente se desconoce**.

| Situación | Base de datos (mundo cerrado) | OWL (mundo abierto) |
|---|---|---|
| Ana no tiene `ej:edad` declarada | Ana no tiene edad (o es NULL) | No sabemos la edad de Ana |
| No hay triple `ej:ana a ej:Médico` | Ana NO es médico | No sabemos si Ana es médico |

Esto es fundamental: no puedes "probar" que algo es falso solo porque no aparece en tus datos. Para negar algo explícitamente, necesitas restricciones (p.ej., `owl:disjointWith`, `owl:complementOf`).

### Razonamiento (alto nivel)

Un **razonador** (Pellet, HermiT, ELK…) toma tu ontología + datos y deduce nuevos hechos:

1. **Clasificación**: organiza la jerarquía de clases (¿quién es subclase de quién?).
2. **Realización**: asigna individuos a sus clases más específicas.
3. **Consistencia**: verifica que no hay contradicciones (p.ej., algo es Perro y Gato al mismo tiempo si son disjuntos).

No necesitas escribir reglas procedurales — describes el dominio de forma declarativa y el razonador saca las conclusiones.

---

## 6. Plan de módulos del curso

A continuación, los módulos propuestos. Cada uno se convertirá en su propio documento dentro de `research/ontologies/course/`.

- [ ] **M01 — Fundamentos de RDF**
  Triples, IRIs, literales, nodos en blanco y grafos con nombre.
  Ejercicios prácticos con Turtle: crear y leer triples a mano.
  Herramientas: validar RDF online con un parser Turtle.

- [ ] **M02 — Turtle a fondo**
  Sintaxis completa: prefijos, abreviaturas, listas y colecciones.
  Buenas prácticas para organizar archivos `.ttl`.
  Comparación breve con otros formatos (N-Triples, JSON-LD, RDF/XML).

- [ ] **M03 — RDFS: vocabularios y jerarquías**
  Clases, subclases, propiedades, dominio y rango.
  Construir un vocabulario RDFS pequeño para un dominio real.
  Inferencias RDFS: qué puede deducir un razonador con solo RDFS.

- [ ] **M04 — OWL: clases, propiedades y restricciones**
  Tipos de propiedades (Object, Datatype, Annotation).
  Restricciones (someValuesFrom, allValuesFrom, cardinality).
  EquivalentClass, disjointWith y unionOf/intersectionOf.

- [ ] **M05 — Razonamiento y consistencia**
  Open World Assumption en detalle con ejemplos sorprendentes.
  Razonadores (Pellet, HermiT, ELK): qué hacen y cuándo usarlos.
  Ejercicio: detectar inconsistencias a propósito en una ontología.

- [ ] **M06 — Herramientas: Protégé y WebProtégé**
  Instalación y tour de Protégé Desktop.
  Crear una ontología desde cero en Protégé.
  Colaboración con WebProtégé.

- [ ] **M07 — SPARQL: consultar datos RDF**
  Anatomía de una consulta SPARQL (SELECT, WHERE, FILTER, OPTIONAL).
  Consultar endpoints públicos (DBpedia, Wikidata).
  CONSTRUCT y ASK: más allá de SELECT.

- [ ] **M08 — Ontologías en la práctica**
  Patrones de diseño ontológico (Ontology Design Patterns).
  Reutilizar ontologías existentes (Dublin Core, SKOS, schema.org, FOAF).
  Publicar y documentar tu ontología.

- [ ] **M09 — Ontologías + LLMs**
  Extracción de entidades y relaciones asistida por LLMs.
  OntoGPT: grounding y curación semi-automática.
  Ontologías como contexto estructurado para RAG y agentes.

- [ ] **M10 — Proyecto final**
  Diseñar, implementar y documentar una ontología de dominio completa.
  Poblarla con datos reales, razonar sobre ella y consultarla con SPARQL.
  Revisión cruzada y retroalimentación.

---

## Fuentes y referencias

- [Ontology Development 101 (Protégé Wiki)](https://protegewiki.stanford.edu/wiki/Ontology101) — guía clásica paso a paso para construir tu primera ontología.
- [Protégé](https://protege.stanford.edu/) — editor de ontologías de código abierto (Stanford).
- [WebProtégé](https://protegewiki.stanford.edu/wiki/WebProtege) — versión web colaborativa de Protégé.
- [OntoGPT](https://github.com/monarch-initiative/ontogpt) — herramienta para extracción y grounding de ontologías asistida por LLMs.
- [Ontologies, Context Graphs, and Semantic Layers (MetadataWeekly)](https://metadataweekly.substack.com/p/ontologies-context-graphs-and-semantic) — artículo introductorio que motivó esta investigación.
- [RDF 1.1 Primer (W3C)](https://www.w3.org/TR/rdf11-primer/) — introducción oficial al modelo RDF.
- [OWL 2 Web Ontology Language Primer (W3C)](https://www.w3.org/TR/owl2-primer/) — introducción oficial a OWL 2.
