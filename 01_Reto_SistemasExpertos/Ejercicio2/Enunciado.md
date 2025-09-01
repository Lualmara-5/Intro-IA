# Reto 2: Sistema Experto para Árboles Genealógicos 👨‍👩‍👧‍👦

## 📌 Descripción del problema
El estudio de los lazos familiares es fundamental en genealogía, derecho de herencias, biología y sociología.  

Muchas veces se cuenta solo con información parcial (padres, madres, hijos), pero se requiere deducir relaciones más complejas como abuelos, tíos, primos y hermanos.  

La tarea es implementar un **sistema experto basado en reglas** que permita inferir automáticamente nuevas relaciones familiares a partir de los hechos básicos proporcionados.

---

## 🧩 Definición de hechos

### Hechos básicos (se proporcionan en la base de conocimiento):
- **Hombre(nombre=str):** Representa a un hombre en la familia.  
- **Mujer(nombre=str):** Representa a una mujer en la familia.  
- **Padre(padre=str, hijo=str):** Relación de paternidad.  
- **Madre(madre=str, hijo=str):** Relación de maternidad.  

### Hechos derivados (deben inferirse mediante reglas):
- **Progenitor(progenitor=str, hijo=str)** → Se deduce de Padre y Madre.  
- **Abuelo(abuelo=str, nieto=str)** → Se deduce si X es progenitor del padre o madre de alguien y es Hombre.  
- **Abuela(abuela=str, nieto=str)** → Similar a Abuelo, pero si X es Mujer.  
- **Hermano(hermano=str, hermano_de=str)** → Se deduce si dos personas tienen al menos un progenitor en común y X es Hombre.  
- **Hermana(hermana=str, hermana_de=str)** → Similar a Hermano, pero si X es Mujer.  
- **Tio(tio=str, sobrino=str)** → Se deduce si X es Hermano de un Progenitor.  
- **Tia(tia=str, sobrino=str)** → Similar a Tio, pero si X es Hermana.  
- **Primo(primo=str, primo_de=str)** → Se deduce si los progenitores de X y Y son Hermanos o Hermanas.  

---

## 📝 Reglas a implementar
1. **Regla de progenitores**
   - Si `Padre(padre=X, hijo=Y)` → `Progenitor(progenitor=X, hijo=Y)`  
   - Si `Madre(madre=X, hijo=Y)` → `Progenitor(progenitor=X, hijo=Y)`  

2. **Regla de abuelos y abuelas**
   - Si `Progenitor(X, Y)` y `Progenitor(Y, Z)` →  
     - `Abuelo(abuelo=X, nieto=Z)` si X es Hombre  
     - `Abuela(abuela=X, nieto=Z)` si X es Mujer  

3. **Regla de hermanos y hermanas**
   - Si X e Y tienen al menos un progenitor en común:  
     - `Hermano(hermano=X, hermano_de=Y)` si X es Hombre  
     - `Hermana(hermana=X, hermana_de=Y)` si X es Mujer  

4. **Regla de tíos y tías**
   - Si X es Hermano de un Progenitor de Y → `Tio(tio=X, sobrino=Y)`  
   - Si X es Hermana de un Progenitor de Y → `Tia(tia=X, sobrino=Y)`  

5. **Regla de primos**
   - Si X y Y son hijos de progenitores que son Hermanos o Hermanas → `Primo(primo=X, primo_de=Y)`  

---

## ⚡ Control de prioridades (salience)
Para que las reglas se ejecuten en orden correcto:  
1. Progenitores → **100**  
2. Abuelos y abuelas → **80**  
3. Hermanos y hermanas → **60**  
4. Tíos y tías → **40**  
5. Primos → **20**  

---

## 📂 Base de conocimiento (ejemplo)
```python
# Hombres
motor.declare(Hombre(nombre='juan'))
motor.declare(Hombre(nombre='pedro'))
motor.declare(Hombre(nombre='carlos'))
motor.declare(Hombre(nombre='david'))
motor.declare(Hombre(nombre='luis'))

# Mujeres
motor.declare(Mujer(nombre='maria'))
motor.declare(Mujer(nombre='ana'))
motor.declare(Mujer(nombre='sofia'))
motor.declare(Mujer(nombre='laura'))
motor.declare(Mujer(nombre='carmen'))

# Relaciones padre
motor.declare(Padre(padre='juan', hijo='pedro'))
motor.declare(Padre(padre='juan', hijo='ana'))
motor.declare(Padre(padre='pedro', hijo='david'))
motor.declare(Padre(padre='pedro', hijo='sofia'))
motor.declare(Padre(padre='carlos', hijo='luis'))
motor.declare(Padre(padre='carlos', hijo='laura'))

# Relaciones madre
motor.declare(Madre(madre='maria', hijo='pedro'))
motor.declare(Madre(madre='maria', hijo='ana'))
motor.declare(Madre(madre='laura', hijo='david'))
motor.declare(Madre(madre='laura', hijo='sofia'))
motor.declare(Madre(madre='carmen', hijo='luis'))
motor.declare(Madre(madre='carmen', hijo='laura'))
```
---

## ❓ Preguntas a responder
1. ¿Cómo se podría modificar el código para inferir bisabuelos y bisabuelas?
2. ¿Por qué la inferencia de Abuelo y Abuela requiere dos niveles de Progenitor?

---

📂 Este reto se debe implementar en Python utilizando la librería **experta**.  
