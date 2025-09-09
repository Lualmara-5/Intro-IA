# 📘 Guía de Ontologías con RDFLib en Python

Este documento sirve como **guía práctica** para resolver los 10 ejercicios de ontologías propuestos en el curso. Aquí encontrarás la **explicación teórica mínima**, la **instalación de dependencias**, la **estructura base de código** y ejemplos de uso paso a paso.

---

## 🚀 1. Instalación de dependencias

Antes de empezar, asegúrate de tener Python 3.x instalado y ejecuta en tu entorno:

```bash
pip install rdflib
pip install owlrl
```

- **rdflib** → Librería principal para trabajar con RDF, OWL, grafos, nodos, triples.
- **owlrl** → Extensión para realizar razonamiento (inferencias) con reglas OWL y RDFS.

---

## 🧩 2. Conceptos básicos
### RDF (Resource Description Framework)
Es un modelo para representar información como tripletas:
```bash
(Sujeto, Predicado, Objeto)
```
Ejemplo:
```bash
"El Señor de los Anillos" -- tieneAutor --> "J.R.R. Tolkien"
```
---

### Ontologias
Una ontología define **clases, propiedades y relaciones** de un dominio.

Ejemplo:
- Clase: `Libro`
- Propiedad: `tieneAutor`
- Instancia: `Señor de los Anillos`

---

### Nodos en RDFLib

- **URIRef**: Identificadores únicos de recursos.
- **Literal**: Valores simples (números, strings, fechas).
- **BNode**: Nodos en blanco, sin nombre explícito (útiles para autores, direcciones, etc.).

---

## 🛠️ 3. Estructura base de un programa con RDFLib
```python
from rdflib import Graph, Namespace, Literal, RDF, RDFS, OWL, BNode, URIRef

# 1. Crear grafo
g = Graph()

# 2. Definir un namespace (prefijo para las URIs)
book = Namespace("http://example.org/book/")
g.bind("book", book)

# 3. Crear recursos
autor = BNode()  # nodo en blanco
g.add((book.SeñorDeLosAnillos, book.Autor, autor))
g.add((autor, book.Nombre, Literal("J.R.R. Tolkien")))
g.add((autor, book.AnioNacimiento, Literal("1892")))
```

---

## 🔎 4. Consultas SPARQL
Con RDFLib puedes hacer consultas sobre el grafo:
```python
qres = g.query(
    """
    SELECT ?nombre ?anio
    WHERE {
        ?autor book:Nombre ?nombre .
        ?autor book:AnioNacimiento ?anio .
    }
    """
)

for row in qres:
    print(f"Autor: {row.nombre}, Año: {row.anio}")
```

---

## 📤 5. Guardar y cargar grafos
```python
# Guardar en formato Turtle
g.serialize("ontologia.ttl", format="turtle")

# Leer desde archivo
g2 = Graph()
g2.parse("ontologia.ttl", format="turtle")
```

Formatos comunes:
- `"turtle"` (`.ttl`)
- `"xml"` (`.rdf`)
- `"nt"` (`.nt`)

---

## 🧠 6. Inferencias con OWLRL
```python
from owlrl import DeductiveClosure, RDFS_Semantics

# Aplicar reglas RDFS/OWL
DeductiveClosure(RDFS_Semantics).expand(g)
```
Esto permite inferir nuevas tripletas automáticamente a partir de reglas.

---

## 📝 7. Ejemplo completo
```python
from rdflib import Graph, Namespace, Literal, BNode

# Crear grafo
g = Graph()

# Namespace
book = Namespace("http://example.org/book/")
g.bind("book", book)

# Recurso libro
libro = book.SeñorDeLosAnillos
autor = BNode()

# Tripletas
g.add((libro, book.Titulo, Literal("El Señor de los Anillos")))
g.add((libro, book.Autor, autor))
g.add((autor, book.Nombre, Literal("J.R.R. Tolkien")))
g.add((autor, book.AnioNacimiento, Literal("1892")))

# Guardar en Turtle
g.serialize("ejercicio1.ttl", format="turtle")
print(g.serialize(format="turtle").decode("utf-8"))

```

---

## ✅ 8. Recomendaciones
- Siempre usar `Namespace` para organizar las URIs.
- Utilizar `BNode` para autores, direcciones o datos sin URI explícita.
- Usar SPARQL para validar que los datos se añadieron correctamente.

---

## 📑 9. Lista de Ejercicios de Ontologías

A continuación, se describen los 10 ejercicios con sus objetivos principales:

### 📝 Ejercicio 1: Formato turtle
- Objetivo: Generar una imagen del grafo usando el formato turtle y validar la sintaxis en: [RDF Grapher](https://www.ldf.fi/service/rdf-grapher)

### 📝 Ejercicio 2: Uso de `Namespace` y URIs
- Objetivo: Definir un `Namespace` personalizado y organizar las URIs de recursos para mantener consistencia en el grafo.

### 📝 Ejercicio 3: Nodos en blanco (`BNode`)
- Objetivo: Representar información compleja (ej. autor de un libro) con nodos en blanco y relacionarlos con otras entidades.

### 📝 Ejercicio 4: Literales y propiedades de datos
- Objetivo: Agregar propiedades a un recurso con `Literal` (números, cadenas, fechas) y diferenciarlas de relaciones entre recursos.

### 📝 Ejercicio 5: Declarar clases y tipos
- Objetivo: Definir clases (`Libro`, `Autor`) y usar `RDF.type` para asignar instancias a dichas clases.

### 📝 Ejercicio 6: Subclases y jerarquía
- Objetivo: Usar `RDFS.subClassOf` para establecer jerarquías entre clases (ej. `Novela` es subclase de `Libro`).

### 📝 Ejercicio 7: Propiedades entre recursos
- Objetivo: Conectar instancias entre sí (ej. un autor escribe un libro, un libro pertenece a un género).

### 📝 Ejercicio 8: Consultas SPARQL
- Objetivo: Realizar consultas sobre el grafo para recuperar información específica (ej. todos los autores con su año de nacimiento).

### 📝 Ejercicio 9: Serialización en distintos formatos
- Objetivo: Guardar el grafo en formatos RDF (`turtle`, `xml`, `nt`) y cargarlo nuevamente en RDFLib.

### 📝 Ejercicio 10: Inferencias con OWLRL
- Objetivo: Aplicar reglas de razonamiento (`owlrl`) para inferir nueva información a partir de clases, propiedades y jerarquías.

---

📌 **Tip**: Cada ejercicio se puede resolver siguiendo la estructura explicada en esta guía (crear grafo → definir namespace → añadir tripletas → consultas/inferencias → guardar).
