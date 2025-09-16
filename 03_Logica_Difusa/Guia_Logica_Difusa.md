# Guía — Lógica Difusa con `scikit-fuzzy`

**Propósito:**
Esta guía explica lo necesario para entender y extender el *notebook* sobre un **Sistema Difuso de Satisfacción en una Cafetería**. 

Incluye instalación, explicación de los conceptos principales, funciones de pertenencia, reglas, simulación, visualización y buenas prácticas para ajustar el sistema.

---

## ¿Qué es la lógica difusa? (Explicación sencilla)

La lógica difusa es una forma de representar el pensamiento humano en los sistemas. A diferencia de la lógica clásica, donde algo solo puede ser **verdadero (1)** o **falso (0)**, en lógica difusa los valores pueden estar **entre 0 y 1**.

👉 Ejemplo cotidiano:

* En lógica clásica: *El café es bueno* (1) o *El café no es bueno* (0).
* En lógica difusa: *El café es 0.7 bueno y 0.3 malo*.

Esto permite modelar situaciones reales donde no todo es blanco o negro, como la calidad del café, la atención de un barista o la rapidez del servicio. Así, las computadoras pueden tomar decisiones de manera más humana y flexible.

En tu proyecto, usamos la lógica difusa para estimar la **satisfacción del cliente** combinando varias entradas que no siempre son absolutas.

---

## Tabla de contenido

1. Resumen rápido
2. Requisitos e instalación
3. Estructura del notebook (bloques `####`)
4. Explicación detallada (línea a línea / por secciones)
5. Funciones de pertenencia comunes (qué hacen y cuándo usarlas)
6. Modificadores / hedges (concentración, dilatación, negación)
7. Reglas difusas: cómo construirlas y combinarlas
8. ControlSystem y simulación
9. Visualización (métodos `.view()` y gráficos personalizados)
10. Errores comunes y cómo depurarlos
11. Extensiones y siguientes pasos
12. Checklist rápido

---

## 1) Resumen rápido

Un sistema difuso transforma entradas (por ejemplo: calidad del café, atención, rapidez) en un valor de salida difuso (satisfacción) mediante:

* Definición de universos y variables (Antecedentes y Consecuentes)
* Definición de funciones de pertenencia (mf)
* Reglas difusas (if-then) que combinan variables
* Agregación y defuzzificación para obtener un valor crisp

Tu *notebook* está dividido en 4 bloques (señalados por `####`), que cubren exactamente los pasos anteriores más visualizaciones.

---

## 2) Requisitos e instalación

Requisitos mínimos:

* Python 3.8+ (ideal 3.10+)
* `numpy`
* `matplotlib`
* `scikit-fuzzy` (paquete `scikit-fuzzy` o `skfuzzy`)

Instalación (recomendado en un venv o conda env):

```bash
pip install -U numpy matplotlib scikit-fuzzy
# o en Jupyter (si usas celdas):
!pip install -q scikit-fuzzy
```

---

## 3) Estructura del notebook (4 bloques)

Se cuenta con un notebook en 4 bloques lógicos — aquí el resumen de cada uno:

**Bloque 1 — Instalación (línea única):**

```python
!pip install -q scikit-fuzzy
```

**Bloque 2 — Definición del sistema y funciones de pertenencia:**

* Importaciones (`numpy`, `skfuzzy`, `ctrl`, `matplotlib`).
* Declaración de Antecedents (`calidad_cafe`, `atencion_barista`, `rapidez_servicio`) y Consequent (`satisfaccion_cliente`) con sus universos.
* Definición de funciones de pertenencia con `fuzz.gaussmf`, `fuzz.gbellmf`, `fuzz.sigmf`, `fuzz.zmf`, `fuzz.smf`, `fuzz.trapmf`, etc.
* Ejemplo de aplicación de un modificador (`concentracion`) y creación de una etiqueta extra (`muy_excelente`).
* Visualización rápida: `rapidez_servicio.view()`

**Bloque 3 — Reglas y simulación:**

* Definición de reglas con `ctrl.Rule(...)` (operadores `&`, `|`).
* Creación del `ControlSystem` y `ControlSystemSimulation`.
* Inserción de valores de entrada, `compute()`, e impresión del resultado.
* Visualización por defecto del consequente con `satisfaccion_cliente.view(sim=simulador)`.

**Bloque 4 — Gráficos personalizados:**

* `plot_membership()` para dibujar todas las mf de una variable (usa `var.terms` y `var[label].mf`).
* Gráfico personalizado de la salida con una línea vertical que marca el resultado calculado.

---

## 4) Explicación detallada (línea a línea / por secciones)

> Aquí se amplía lo que hace cada fragmento importante (resumen práctico, no se repite todo el código):

### a) Definir universos

```python
calidad_cafe = ctrl.Antecedent(np.arange(0, 11, 0.1), 'calidad_cafe')
```

* `np.arange(0,11,0.1)` define el rango numérico del dominio (0..10) con resolución 0.1. Elegir la resolución dependerá de cuánto detalle quieras en la representación.
* Nombres legibles (`'calidad_cafe'`) ayudan a las visualizaciones y mensajes de error.

### b) Funciones de pertenencia

* `fuzz.gaussmf(x, mean, sigma)` → campana Gaussiana (parámetros: media y sigma).
* `fuzz.gbellmf(x, a, b, c)` → campana generalizada (a: ancho, b: pendiente/forma, c: centro).
* `fuzz.sigmf(x, b, c)` → sigmoide (b: pendiente, c: centro) — sirve para bordes "graduales".
* `fuzz.trapmf(x, [a, b, c, d])` → trapecio (útil como 'baja' y 'alta' con extremos planos).
* `fuzz.zmf` y `fuzz.smf` → funcionan como funciones de transición rápida a izquierda/derecha.

**Consejo:** revisa la firma en la doc si dudas del orden de los parámetros (algunas funciones usan orden `(x, a, b, c)` mientras otras `(x, mean, sigma)`).

### c) Modificadores (hedges)

* **Concentración** (`muy`): elevar la mf a una potencia > 1 (p.ej. `mf**2`) hace la pertenencia más estricta.
* **Dilatación** (`mas o menos`): usar una potencia entre 0 y 1 (p.ej. `mf**0.5`) hace la pertenencia más laxa.
* **Negación** (`no X`): `1 - mf`.

En el código: `calidad_cafe_muy_excelente = concentracion(fuzz.sigmf(...))` y luego `calidad_cafe['muy_excelente'] = ...`.

### d) Reglas

* Una regla típica:

```python
regla1 = ctrl.Rule(calidad_cafe['excelente'] & atencion_barista['excelente'], satisfaccion_cliente['alta'])
```

* Operadores: `&` (AND), `|` (OR), y negación con `~` si lo necesitas.
* Puedes encadenar condiciones más complejas: p.ej. `A & (B | C)`.

**Nota:** las reglas se evalúan por el sistema y contribuyen a la agregación final del consecuente.

### e) Simulación

```python
sistema = ctrl.ControlSystem([regla1, regla2, ...])
simulador = ctrl.ControlSystemSimulation(sistema)
simulador.input['calidad_cafe'] = 8.5
simulador.compute()
print(simulador.output['satisfaccion_cliente'])
```

* `simulador.compute()` realiza la inferencia y la defuzzificación (por defecto se suele usar el método centroide).
* `simulador.output[...]` devuelve el valor crisp.

---

## 5) Funciones de pertenencia comunes — ¿cuándo usar cada una?

* **Gaussiana (`gaussmf`)**: casos donde la variable tiene un pico bien definido y simétrico.
* **Campana generalizada (`gbellmf`)**: similar a gaussiana pero con control extra sobre la forma y la pendiente.
* **Sigmoide (`sigmf`)**: cuando quieres una transición suave de 0→1 (o 1→0) a partir de un punto central.
* **Trapecio (`trapmf`)**: cuando quieres rangos planos (p.ej. "bajo" hasta X, luego decrece).
* **Z y S (`zmf`, `smf`)**: funciones que definen transiciones más abruptas en un extremo.

---

## 6) Modificadores / Hedges (práctico)

```python
# Concentración (muy): hacer mf más restrictiva
mf_muy = mf ** 2

# Dilatación (mas o menos): hacer mf más laxa
mf_mas_o_menos = mf ** 0.5

# Negación: complemento
mf_no = 1 - mf
```

Estos operadores se pueden guardar como una etiqueta nueva en la variable, p.ej. `atencion_barista['mas_o_menos'] = atencion_barista_mas_o_menos`.

---

## 7) Reglas difusas: mejores prácticas

* Empieza con reglas simples y claras. Añade complejidad sólo si el sistema falla en casos reales.
* Intenta que las MF se solapen razonablemente (no tanto que sean idénticas, ni tan poco que no haya transición).
* Documenta cada regla con un comentario que diga la intención humana (p.ej. `# Si el café es excelente y la atención es excelente -> alta satisfacción`).
* Si tienes datos reales, valida el sistema con ejemplos y ajusta parámetros de MF.

---

## 8) ControlSystem, simulación y defuzzificación

* `ControlSystem([...])` crea el sistema con las reglas definidas.
* `ControlSystemSimulation(sistema)` crea el objeto que acepta entradas y ejecuta `compute()`.
* **Defuzzificación:** por defecto suele usarse el centroide ("centroid"). Si quieres otro método (ej. bisector o mean-of-maxima), revisa las opciones de `skfuzzy` y pruébalas si tu salida necesita propiedades particulares.

---

## 9) Visualización

* Las variables `Antecedent` y `Consequent` tienen el método `.view()` que dibuja automáticamente las MF y, si pasas `sim=simulador`, muestra el firing de las reglas y la área agregada.
* Tu `plot_membership()` es útil para control fino y estilo; en notebooks Jupyter muestra las curvas con `matplotlib`.
* Para la salida, usar una `axvline` para marcar el valor defuzzificado ayuda a interpretar el resultado.

---

## 10) Errores comunes y cómo depurarlos

* **Parámetros mal ordenados**: revisa la firma de la función `fuzz.*` (ej. `gaussmf`, `gbellmf`, `sigmf`) si ves curvas planas o extrañas.
* **Universos insuficientes**: rango o resolución demasiado grande/pequeña produce curvas poco visibles o computación lenta.
* **Reglas inconsistentes**: si todas las reglas conducen al mismo consecuente sin matices, la salida estará poco sensible.
* **Olvidar `compute()`**: no hay salida hasta que llames `simulador.compute()`.

---

## 11) Extensiones y siguientes pasos

* **Validación con datos reales**: si tienes ejemplos (entradas → satisfacción real), compara el output y ajusta MF mediante búsqueda en grilla o técnicas de optimización.
* **Pesos en reglas**: puedes introducir pesos si algunas reglas deben tener mayor impacto (investiga la API de `ctrl.Rule` para opciones avanzadas).
* **Sistemas multi-output**: modela más de un consequente (p.ej. satisfacción y probabilidad de volver) si lo necesitas.
* **Interfaz**: montar una pequeña app con `streamlit` o `flask` para probar casos interactivos.

---

## 12) Checklist rápido

* [ ] Rango (universe) correcto para cada variable
* [ ] Resolución (paso) adecuada
* [ ] MF con solapamientos razonables
* [ ] Reglas documentadas y probadas con ejemplos
* [ ] Visualizaciones claras (incluye `view()` y gráficos custom)
* [ ] Probar distintos métodos de defuzzificación si la respuesta es inestable

---

## Ejemplo mínimo de uso (snippet)

```python
# Definir variable y MF
x = ctrl.Antecedent(np.arange(0, 11, 0.1), 'x')
x['bajo'] = fuzz.trapmf(x.universe, [0, 0, 2, 4])
x['alto'] = fuzz.trapmf(x.universe, [6, 8, 10, 10])

# Consecuente
y = ctrl.Consequent(np.arange(0, 101, 1), 'y')
y['peque'] = fuzz.trapmf(y.universe, [0, 0, 25, 50])

# Regla y simulación
r = ctrl.Rule(x['alto'], y['peque'])
sys = ctrl.ControlSystem([r])
sim = ctrl.ControlSystemSimulation(sys)
sim.input['x'] = 7.5
sim.compute()
print(sim.output['y'])
```

---


