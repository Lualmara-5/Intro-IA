# Ejercicio 3: Sistema Experto de Diagnóstico Médico 🩺🤖

## 📌 Descripción del problema
En el ámbito de la salud, la detección temprana y precisa de enfermedades es crucial para garantizar un tratamiento adecuado.  

Sin embargo, no siempre se cuenta con un médico disponible, especialmente en zonas de difícil acceso.  

Los **sistemas expertos médicos** se convierten en una herramienta útil al evaluar síntomas y proporcionar posibles diagnósticos junto con recomendaciones médicas.

La tarea es **completar un sistema experto basado en reglas** que diagnostique enfermedades y genere recomendaciones según la gravedad.

---

## 🧩 Funcionamiento del sistema
1. Cada enfermedad en la base de conocimiento tiene una lista de síntomas.  
2. Se comparan los síntomas del paciente con los de la enfermedad.  
3. Si al menos la **mitad de los síntomas coinciden**, se declara un diagnóstico.  
4. Según la gravedad (1, 2 o 3), se genera una **recomendación médica** adecuada.  

---

## 📝 Reglas de diagnóstico a implementar
- **Evaluar resfriado común** → Diagnosticar si se cumplen los síntomas del resfriado.  
- **Evaluar gripe** → Diagnosticar gripe.  
- **Evaluar COVID-19** → Diagnosticar COVID-19.  
- **Evaluar neumonía** → Diagnosticar neumonía.  

Cada diagnóstico se declara con:  
```python
self.declare(Diagnostico(enfermedad='nombre_enfermedad'))
```

---

## 📝 Reglas de recomendación médica
- **Recomendar leve (gravedad 1)** → Descanso en casa.
- **Recomendar moderada (gravedad 2)** → Consultar al médico.
- **Recomendar grave (gravedad 3)** → Buscar atención médica urgente.

Cada recomendación se declara con:
```python
self.declare(Recomendacion(enfermedad=enf, mensaje="mensaje adecuado"))
```

---

## ⚡ Consejos para la implementación
- **Comparar correctamente los síntomas ingresados con los de la base de conocimiento.**
- **Usar la condición:**
```python
coincidencias >= len(sintomas)/2
```
- **Verificar el nivel de gravedad antes de declarar la recomendación.**

---

📂 Este reto se debe implementar en Python o Google Colab (Archivo .ipynb) utilizando la librería experta.
