# Ejercicio 1: Sistema Experto para Diagnóstico de Fallas en un Vehículo 🚗⚙️

## 📌 Descripción del problema
Un mecánico recibe vehículos con problemas y necesita un asistente experto que sugiera diagnósticos a partir de los síntomas observados. 

Además, el sistema debe registrar el progreso de las reparaciones eliminando síntomas que ya han sido resueltos.

La tarea es implementar un **sistema experto basado en reglas** que pueda diagnosticar fallas y proponer acciones de reparación.

---

## 🧩 Definición de hechos
- **Symptom(tipo=str):** Representa un síntoma en el vehículo.  
- **CarState(estado=str):** Representa el estado general del vehículo.  
- **Diagnosis(resultado=str):** Representa la conclusión del sistema.  
- **RepairAction(tipo=str):** Acción de reparación específica.  
- **VehicleStatus(estado=str):** Estado general del proceso de reparación.

---

## 📝 Reglas de diagnóstico
1. Si existe la falla grave `Symptom(tipo='humo_blanco')` y `Symptom(tipo='luz_aceite')` →  
   `Diagnosis(resultado='Posible junta de cabeza mala, motor en riesgo')`  

2. Si existe la falla moderada `Symptom(tipo='ruido_metalico')` →  
   `Diagnosis(resultado='Revisar sistema de frenos')`  

3. Si existe la falla moderada `Symptom(tipo='fuga_liquido')` y `CarState(estado='motor_caliente')` →  
   `Diagnosis(resultado='Pérdida de refrigerante, posible sobrecalentamiento')`  

4. Si no hay síntomas críticos →  
   `Diagnosis(resultado='Revisión general recomendada')`  

---

## 🔧 Reglas de reparación
- Si `RepairAction(tipo='reparar_frenos')` y existe `Symptom(tipo='ruido_metalico')` → eliminar síntoma y declarar `VehicleStatus(estado='verificar_reparacion')`.  

- Si `RepairAction(tipo='reparar_motor')` y existe `Symptom(tipo='humo_blanco')` → eliminar síntoma, cambiar `CarState(estado='en_reparacion')` y declarar `VehicleStatus(estado='verificar_reparacion')`.  

- Si `RepairAction(tipo='reparar_motor')` y existe `Symptom(tipo='luz_aceite')` → eliminar síntoma y declarar `VehicleStatus(estado='verificar_reparacion')`.  

---

## ⚡ Uso de prioridades (salience)
- Fallas graves → **100**  
- Fallas moderadas → **50**  
- Reparaciones → **150**  
- Revisión general → **10**  
- Verificación final → **5**  

---

## ❓ Preguntas a responder
1. ¿Qué resultado se obtiene si se agrega un nuevo síntoma como `ruido_metalico` en la entrada?  
2. ¿Qué sucede si se cambia la saliencia de `motor_grave_dano` de 100 a 40?  
3. ¿Qué sucede si eliminamos salience de todas las reglas?  
4. ¿Qué ocurre si se activan múltiples reglas que declaran hechos del mismo tipo `Diagnosis` pero con valores distintos?  
5. ¿Cómo cambia el comportamiento del sistema cuando se ejecuta nuevamente después de eliminar algunos síntomas con `retract`?  
6. ¿Qué ventajas presenta el uso de un hecho intermedio (`RepairAction`) para gestionar las reparaciones?  

---

📂 Este reto se debe implementar en Python o Google Colab (Archivo .ipynb) utilizando la librería **experta**.  
