
# Corte 1 – Selección y justificación del caso

> **Este documento debe ser completado y entregado por la pareja a más tardar el jueves 8 de octubre de 2026, 11:00 a.m.**

## 1. Integrantes

- Integrante 1: Kristel Muñoz Flores 
- Integrante 2: Emma Aguirre Torres

## 2. Revisión de los casos

Complete la siguiente tabla antes de seleccionar el caso.

| Caso | Problema principal identificado | ¿Permite Process Mining? ¿Por qué? | Posible uso posterior de ML | Nivel de interés para la pareja: Alto/Medio/Bajo |
|---|---|---|---|---|
| 01. Salud |Demoras en la atención y tiempos de triage en urgencias. |Sí, porque registra el paso de los pacientes por distintas áreas y estados clínicos. |Predecir hospitalización o tiempos de espera prolongados. |Medio |
| 02. Pedidos | Retrasos en la entrega de órdenes y cuellos de botella logísticos.|Sí, cuenta con una secuencia clara de estados transaccionales desde el registro hasta la entrega. |Predecir si un pedido se entregará con retraso respecto a la fecha prometida. | Alto|
| 03. Créditos |Tiempos excesivos en la aprobación y validación de solicitudes financieras. | la aprobación y validación de solicitudes financieras.	Sí, modela el flujo de análisis, montos y paso por comités.| Predecir la aprobación o rechazo de solicitudes de crédito.|Medio |
| 04. Soporte TI |Incidencias bloqueadas o mal escaladas entre niveles técnicos. |Sí, permite ver la ruta de resolución de tickets. | Predecir la criticidad o tiempo de resolución de un ticket.| Medio|
| 05. Académico |Gestiones estudiantiles con procesos repetitivos o desvíos. | Sí, pero con menor variabilidad logística comercial.|Predecir deserción o rendimiento académico. | Bajo|
| 06. Mantenimiento |Fallas en equipos y tiempos muertos por reparaciones. |Sí, registra eventos de mantenimiento correctivo y preventivo. |Predecir fallas futuras en equipos. | Bajo|

## 3. Caso seleccionado

**Número y nombre del caso:**Caso 02 – Gestión de pedidos

**Archivo de datos:** datos/raw/caso_02_pedidos.csv

Antes de responder las secciones siguientes, abra el archivo de datos del caso y revise sus columnas en [datos/README.md](../datos/README.md).

## 4. Justificación de la selección

Explique por qué la pareja seleccionó este caso.

La justificación debe considerar:

- relevancia de la problemática;
- disponibilidad de eventos;
- posibilidad de identificar casos individuales;
- disponibilidad de marcas de tiempo;
- atributos adicionales;
- potencial para descubrir variantes;
- posibilidad de realizar posteriormente un análisis mediante Machine Learning.

**Respuesta:**

Seleccionamos el Caso 02 – Gestión de pedidos debido a su alta relevancia en el sector logístico y comercial, donde el cumplimiento de los tiempos de entrega es crítico para la satisfacción del cliente. El conjunto de datos ofrece una disponibilidad óptima de eventos secuenciales con identificadores claros de instancias (case_id), marcas de tiempo precisas (timestamp) y recursos asociados. Asimismo, las variables a nivel de caso (como categoría, monto, región, transportista y canal de venta) permiten descubrir múltiples variantes de procesos, detectar cuellos de botella en el despacho o empaque, y sentar bases sumamente sólidas para implementar posteriormente un modelo predictivo de Machine Learning enfocado en predecir retrasos logísticos.

## 5. Comprensión inicial del proceso

### 5.1 ¿Cuál considera que es el inicio del proceso?

El registro y creación inicial de la orden de compra en el sistema por parte del cliente a través de los canales Web, App o Teléfono.

### 5.2 ¿Cuál considera que es el final del proceso?

La entrega efectiva del pedido al cliente o la cancelación definitiva del mismo.

### 5.3 ¿Qué actividades principales espera encontrar?
Creación o recepción del pedido.

Verificación y aprobación del pago.

Asignación y empaque de productos en bodega (picking & packing).

Despacho y entrega por parte del transportista.

### 5.4 Proponga los componentes iniciales del Event Log

| Componente | Variable o propuesta | Justificación |
|---|---|---|
| Case ID | case_id| (Identificador unívoco del pedido).|
| Activity |activity |(Nombre de la actividad o estado del pedido). |
| Timestamp | timestamp| (Fecha y hora exacta del registro del evento).|
| Recurso, si aplica |resource | (Persona, sistema o canal que ejecutó la acción).|
| Otros atributos |categoria, monto, metodo_pago, region, transportista, cantidad_productos, canal_venta, fecha_prometida | |

---

## 6. Preguntas iniciales para Process Mining

Formule al menos **tres preguntas** que podrían responderse mediante Process Mining.

Pregunta 1: ¿Cuál es el camino principal (happy path) que siguen los pedidos desde su creación hasta su entrega final, y qué porcentaje de las órdenes completan este flujo sin desviaciones?

Pregunta 2: ¿Qué etapas del proceso (por ejemplo, la validación de pagos o el empaque en bodega) concentran los mayores tiempos de espera y actúan como cuellos de botella operativos?

Pregunta 3: ¿Existen rutas alternativas o bucles (loops) de reproceso que afecten de manera directa el cumplimiento de la fecha prometida de entrega?

---

## 7. Hipótesis o sospechas iniciales

Sin realizar todavía el análisis formal, indique qué comportamientos considera que podrían aparecer en el proceso.

Ejemplos de elementos a considerar:

- retrasos;
- reprocesos;
- actividades repetidas;
- variantes;
- cuellos de botella;
- incumplimientos;
- rutas excepcionales.

**Respuesta:**

Se sospecha que los pedidos con montos elevados o de ciertas regiones geográficas sufren mayores retrasos debido a restricciones logísticas del transportista.

Podrían presentarse actividades repetidas o bucles en la validación de pagos cuando la transferencia o tarjeta presenta inconsistencias.

Es probable que el cuello de botella principal ocurra en la transición entre la aprobación interna y la preparación física del paquete en bodega.
## 8. Posible problema futuro de Machine Learning

En esta etapa **no se debe construir ningún modelo**.

Proponga únicamente una posible pregunta que, después del análisis de Process Mining, podría convertirse en un problema de Machine Learning.

### Posible variable objetivo

Un indicador binario que determine si el pedido resultó atrasado (1 si la entrega efectiva ocurrió después de fecha_prometida, 0 en caso contrario).
### Posibles variables predictoras

monto, cantidad_productos, metodo_pago, region, transportista, canal_venta, además de métricas de proceso derivadas del Minado de Procesos (como tiempo transcurrido en etapas previas o frecuencia de cambios de estado).
### Tipo de problema que podría resultar

- [x] Clasificación
- [ ] Regresión
- [ ] Agrupamiento
- [ ] Aún no se puede determinar

### Justificación

Se plantea como un problema de clasificación supervisada para predecir si una orden de compra caerá en categoría de retraso, permitiendo a la organización tomar acciones preventivas antes de que se incumpla el plazo acordado con el cliente.
---

## 9. Decisión final de la pareja

Explique en un párrafo por qué este caso ofrece una buena oportunidad para integrar Process Mining y Machine Learning.

El Caso 02 ofrece una oportunidad ideal para integrar Process Mining y Machine Learning porque combina un flujo transaccional con claras métricas de tiempo y rendimiento. La disponibilidad de fechas prometidas y atributos logísticos permite no solo auditar el comportamiento real de los procesos y detectar cuellos de botella mediante pm4py, sino también construir un modelo predictivo robusto en scikit-learn capaz de anticipar problemas de entrega basándose en el comportamiento operativo temprano.
---

## 10. Declaración de uso de IA generativa

Indique si utilizaron herramientas de IA generativa (ChatGPT, Claude, Copilot, Gemini u otras) en este corte.

| Herramienta | Para qué se utilizó | En qué parte del trabajo |
|---|---|---|
| Herramienta: Gemini (Google)|Apoyo en la estructuración formal, redacción y alineación del contenido con los requerimientos específicos de la plantilla del examen. |En la redacción de las justificaciones técnicas, preguntas de análisis e hipótesis iniciales del documento de entrega del Corte 1. |

Si no se utilizó ninguna, escriba: **"No se utilizaron herramientas de IA generativa."**

> Ambos integrantes deben poder explicar cualquier parte del trabajo, incluida la generada con apoyo de IA.
