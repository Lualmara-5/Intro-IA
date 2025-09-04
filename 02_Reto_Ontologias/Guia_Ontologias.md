# Ontologías con RDFLib y OWLRL

Este proyecto es una introducción práctica al uso de **ontologías en Python** utilizando las librerías `rdflib` y `owlrl`.  

---

## 📦 Dependencias

Antes de ejecutar el código, asegúrate de instalar las librerías necesarias:

```bash
pip install rdflib
pip install owlrl
```

---

## 🔹 `rdflib`

- Es la librería principal para trabajar con **RDF (Resource Description Framework)** en Python.
- Permite:
  - Crear grafos RDF.
  - Leer datos desde la web o archivos locales.
  - Recorrer las tripletas RDF (sujeto, predicado, objeto).
  - Serializar grafos en diferentes formatos (Turtle, XML, JSON-LD, etc.).

## 🔹 `owlrl`

- Implementa reglas de **razonamiento OWL 2 RL**.
- Permite inferir nuevos conocimientos a partir de una ontología existente.
- Ejemplo: si un grafo dice que *“Alejandro es un humano”* y *“todo humano es un mamífero”*, `owlrl` puede inferir automáticamente que *“Alejandro es un mamífero”*.

---

## 📖 Conceptos clave
### 🔹 RDF (Resource Description Framework)

Es un modelo de datos que representa información en forma de tripletas:
```bash
Sujeto - Predicado - Objeto
```

Ejemplo:
```bash
TimBernersLee - esAutorDe - WWW
```

### 🔹 Grafo RDF
- Un grafo es una colección de tripletas RDF.
- Cada tripleta conecta recursos (URI) o valores (literales).
- Se pueden recorrer los nodos y aristas como en un grafo común.

### 🔹 Ontología
- Es una representación formal de un conjunto de conceptos y sus relaciones.
- En informática y web semántica, las ontologías permiten que las máquinas entiendan los datos.

---

## 🚀 Ejemplo de uso
```python
from rdflib import Graph

# 1. Crear un grafo vacío
g = Graph()

# 2. Leer un grafo RDF desde internet
g.parse("http://www.w3.org/People/Berners-Lee/card")

# 3. Recorrer el grafo en forma de tripletas (sujeto, predicado, objeto)
for subj, pred, obj in g:
    print(subj, '-', pred, '-', obj)

# 4. Serializar (exportar) el grafo en formato Turtle
print("\nGrafo serializado\n")
print(g.serialize(format="turtle"))
```

---

## 🔎 Paso a paso

1. Crear un grafo vacío → Graph().

2. Cargar datos RDF → descarga automáticamente la tarjeta de contacto de Tim Berners-Lee (creador de la Web).

3. Recorrer tripletas → imprime cada relación en el grafo.

4. Serializar → guarda o muestra el grafo en formato turtle.
