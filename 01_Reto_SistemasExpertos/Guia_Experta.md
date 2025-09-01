# Guía de la librería Experta

La librería **[experta](https://experta.readthedocs.io/en/latest/)** nos permite crear **sistemas expertos basados en reglas** en Python.  

Un sistema experto funciona parecido a cómo lo haría una persona con experiencia: recibe información, la compara con reglas conocidas y da una conclusión o recomendación.  

---

## 🔹 ¿Qué es un sistema experto?
Imagina que tienes un **mecánico**:  
- Tú le dices “el carro tiene humo blanco” → eso es un **hecho**.  
- El mecánico recuerda: “si hay humo blanco y además se prende la luz de aceite → entonces el motor está en riesgo” → eso es una **regla**.  
- Finalmente, él te responde: “tienes que revisar el motor” → eso es un **diagnóstico o acción**.  

La librería **experta** nos ayuda a programar exactamente este flujo: **Hechos → Reglas → Conclusiones**.  

---

## 🔹 Conceptos principales

### 1. Hechos (`Fact`)
Un **hecho** es una pieza de información.  
- Son como **tarjetas con datos** que el sistema puede usar.  
- Ejemplo: *“ruido metálico”*, *“Pedro es padre de Juan”*, *“el paciente tiene fiebre”*.  

📌 En código:
```python
from experta import Fact

class Symptom(Fact):
    """Representa un síntoma observado en el vehículo"""
    pass

# Declarar un hecho
engine.declare(Symptom(tipo='humo_blanco'))
```

---

### 2. Motor de inferencia (`KnowledgeEngine`)
Es el **cerebro del sistema.**
- Se encarga de recibir los hechos, buscar qué reglas aplican y ejecutarlas.
- Es como el **experto humano** que analiza los datos y toma decisiones.

📌 En código:
```python
from experta import KnowledgeEngine

class VehicleDiagnosis(KnowledgeEngine):
    pass
```

---

### 3. Reglas (`@Rule`)
Una regla dice: “si pasa A y B → entonces hago C”.
- Se escriben con el decorador `@Rule`.
- Puedes combinar condiciones con `AND`, `OR`, `NOT` o capturar valores con `MATCH`.

📌 En código:
```python
from experta import Rule, AND

@Rule(AND(Symptom(tipo='humo_blanco'),
          Symptom(tipo='luz_aceite')), salience=100)
def motor_grave_dano(self):
    print("⚠️ Posible daño grave en el motor")
```

---

### 4. Declarar nuevos hechos (`self.declare`)
Dentro de una regla podemos **crear hechos nuevos.**
Es como si el mecánico dijera: “si hay humo y aceite, entonces anoto: el motor está en riesgo”.

📌 En código:
```python
self.declare(Diagnosis(resultado='Motor en riesgo'))
```

---

### 5. Eliminar hechos (`self.retract`)
A veces un hecho ya no aplica (ejemplo: el carro tenía “ruido metálico”, pero ya se reparó).
En ese caso usamos `retract` para borrarlo.

📌 En código:
```python
self.retract(fact)  # Elimina un síntoma ya resuelto
```

---

### 6. Prioridades (`salience`)
Si varias reglas se cumplen al mismo tiempo, ¿cuál se ejecuta primero?
- Para eso está `salience`: valores más altos = más prioridad.
- Ejemplo:
  - **150** → Reparaciones urgentes
  - **100** → Fallas graves
  - **50** → Fallas moderadas
  - **10** → Revisión general

📌 En código:
```python
@Rule(Symptom(tipo='ruido_metalico'), salience=50)
def revisar_frenos(self):
    print("🔧 Revisar sistema de frenos")
```

---

### 7. Variables dinámicas (`MATCH`)
Nos permiten capturar valores de los hechos y usarlos dentro de la regla.

📌 En código:
```python
from experta import MATCH

@Rule(Padre(padre=MATCH.p, hijo=MATCH.h))
def padre_a_progenitor(self, p, h):
    print(f"👨 {p} es progenitor de {h}")
```

---

### 8. Ejecutar el motor
1. Se inicializa el motor.
2. Se agregan hechos con declare.
3. Se ejecutan las reglas con run().

📌 En código:
```python
if __name__ == "__main__":
    engine = VehicleDiagnosis()
    engine.reset()
    engine.declare(Symptom(tipo='ruido_metalico'))
    engine.run()
```

---

## 📖 Resumen rápido
- `Fact` → una tarjeta de información (ej: “fiebre”).
- `KnowledgeEngine` → el experto que piensa.
- `@Rule` → reglas de decisión (si pasa A → hago B).
- `declare()` → agregar información nueva.
- `retract()` → eliminar información que ya no aplica.
- `salience` → qué reglas son más urgentes.
- `MATCH` → variables para usar dentro de las reglas.
- `run()` → ejecutar el razonamiento.

---

✨ Con estas herramientas puedes entender y resolver los 3 ejercicios de esta carpeta:
1. Diagnóstico de fallas en vehículos 🚗
2. Árbol genealógico 👨‍👩‍👧‍👦
3. Diagnóstico médico 🩺
