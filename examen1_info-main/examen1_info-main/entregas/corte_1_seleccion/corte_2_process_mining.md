# Corte 2 – Process Mining

## 1. Integrantes
- Integrante 1: Kristel Yiliana Muñoz Flores
- Integrante 2: [Emma Aguirre Torres]

## 2. Caso seleccionado
*Caso:* Caso 02 – Gestión de pedidos (`caso_02_pedidos.csv`)

## 3. Descripción de los datos utilizados
- **Cantidad de registros:** 16,532 eventos registrados en el log.
- **Cantidad de casos:** 2,000 pedidos únicos (`case_id`).
- **Cantidad de actividades:** 10 actividades distintas registradas a lo largo de los flujos.
- **Rango temporal:** Desde el 2 de marzo de 2026 en adelante.
- **Variables principales:** `case_id`, `activity`, `timestamp`, `resource`, `categoria`, `monto`, `metodo_pago`, `region`, `transportista`, `cantidad_productos`, `canal_venta` y `fecha_prometida`.
- **Problemas de calidad detectados:** Variaciones en los formatos de fecha y hora, así como la necesidad de normalizar las marcas temporalmente para evitar inconsistencias en el orden de los eventos.

## 4. Preparación de datos
Documente las transformaciones realizadas.
| *Transformación* | *Motivo* | *Efecto sobre los datos* |
| ----- | ----- | ----- |
| Conversión de formato `timestamp` a datetime (`format='mixed'`) | Normalizar las marcas de tiempo para compatibilidad con la librería PM4Py | Permite ordenar cronológicamente los eventos de cada pedido sin errores de tipo. |
| Mapeo de columnas a estándar PM4Py (`case_id` a `case:concept:name`, etc.) | Requerimiento técnico de la librería de minería de procesos | Habilita la ejecución de algoritmos de descubrimiento de DFG y variantes de manera exitosa. |

## 5. Event Log final
- **Notebook o script de construcción:** Se construyó en el cuaderno `02_process_mining.ipynb` ubicado en la carpeta `scripts/`.
- **Case ID:** `case:concept:name`
- **Activity:** `concept:name`
- **Timestamp:** `time:timestamp`
- **Otros atributos utilizados:** `resource`, `categoria`, `monto`, `metodo_pago`, `region`, `transportista`.
- **Muestra representativa:** Pedidos iniciados con la actividad de recepción y validados por el sistema o agentes de ventas.

## 6. Descubrimiento del proceso
Se descubrió el modelo gráfico de tipo *Directly-Follows Graph* (DFG) que cartografía los flujos seguidos por los 2,000 pedidos en el sistema.

## Interpretación
El mapa de procesos muestra que el flujo parte consistentemente de la recepción del pedido, pasando por etapas críticas de validación y aprobación de pagos, hasta llegar a la preparación en bodega, empaque, despacho y entrega final mediante los transportistas asignados.

## 7. Variantes
- **Cantidad de variantes:** 174 variantes únicas encontradas en total.
- **Variante más frecuente:** El "camino feliz" estándar: `('Pedido recibido', 'Validación', 'Pago aprobado', 'Preparación', 'Empaque', 'Despacho', 'Transporte', 'Entrega')`.
- **Porcentaje o frecuencia:** Representa un total de **1,058 casos** del volumen analizado.
- **Principales diferencias entre variantes:** Las variantes alternativas incluyen pasos adicionales de gestión como solicitudes por falta de stock con su respectivo reabastecimiento, cobros contra entrega, recordatorios de pagos, y reintentos por entregas fallidas seguidas de reprogramaciones.

## 8. Análisis temporal
- **Duración promedio:** 10.10 días por pedido.
- **Duración mínima y máxima:** Mínimo de 0.04 días y máximo de 269.84 días en casos atípicos.
- **Actividades con mayor espera:** Las transiciones previas al despacho y la asignación logística de transporte.
- **Transiciones problemáticas:** Aquellas que involucran faltantes de stock o entregas fallidas que exigen reprogramación.
- **Posibles cuellos de botella:** Se concentran en el tiempo transcurrido entre la aprobación del pago y el inicio de la preparación en bodega, así como las demoras en las rutas de los transportistas.

## 9. Verificación de cumplimiento (conformance)
- **Referencia utilizada:** Se utilizó como referencia el flujo ideal descrito en el diseño del caso (Camino Feliz esperado).
- **Técnica aplicada:** Análisis cuantitativo de variantes y conteo de frecuencias frente al proceso estándar.
- **Desviaciones encontradas:** Se identificaron desvíos por inserción de actividades no estándar como *Falta de stock / Reabastecimiento*, *Recordatorios de pago* y *Entregas fallidas con reprogramación*.
- **Casos con desviación:** Del total de 2,000 casos, se detectaron **61 casos con re-trabajo explícito (3.05%)**, además de las rutas alternas reflejadas en las variantes secundarias.
- **Relación con atributos:** Las desviaciones de stock y retrasos se correlacionan frecuentemente con ciertas categorías de productos y regiones de destino más distantes.

## 10. Hallazgos

## Hallazgo 1
*Evidencia:* La Variante 1 concentra 1,058 de los 2,000 casos analizados siguiendo el camino directo sin interrupciones.
*Interpretación:* El proceso operativo principal es altamente estandarizado y eficiente para más de la mitad de los clientes, operando sin fricciones logísticas.

## Hallazgo 2
*Evidencia:* Presencia de la Variante 3 (113 casos) que incluye la secuencia de *Falta de stock* y *Reabastecimiento*.
*Interpretación:* Existe un desfase entre el inventario ofertado en la plataforma y la disponibilidad real en bodega, lo que detiene temporalmente el flujo normal del pedido.

## Hallazgo 3
*Evidencia:* Un porcentaje de re-trabajo del 3.05% (61 casos con repetición interna de actividades) y una duración máxima extrema de 269.84 días.
*Interpretación:* Ciertas incidencias administrativas o logísticas severas bloquean indefinidamente el cierre de los pedidos si no se gestionan activamente.

## 11. Implicaciones para Machine Learning
- **Resultado útil a predecir:** Clasificar si un pedido sufrirá retrasos críticos superando su `fecha_prometida` o si experimentará una entrega fallida.
- **Variables a utilizar:** `monto`, `region`, `transportista`, `cantidad_productos`, `categoria` y `canal_venta`.
- **Variables derivadas del proceso:** 
  - Duración acumulada parcial en etapas tempranas.
  - Cantidad total de actividades ejecutadas en el caso.
  - Número de repeticiones o re-trabajos detectados.
  - Indicador binario de presencia de actividad de *Falta de stock*.
  - ID de la variante de proceso asignada al caso.

## 12. Conclusión del corte
A través de la minería de procesos con PM4Py, se identificó que el sistema maneja un volumen masivo de pedidos con una ruta principal muy clara, pero con cuellos de botella operativos y desviaciones por inventario que impactan los tiempos de ciclo. Estos hallazgos sientan las bases cuantitativas para construir modelos predictivos robustos orientados a anticipar retrasos logísticos en el Corte 3.

## 13. Declaración de uso de IA generativa
| *Herramienta* | *Para qué se utilizó* | *En qué parte del trabajo* |
| ----- | ----- | ----- |
| Gemini | Asistencia en la estructuración de la redacción técnica basada en los resultados obtenidos del código. | Redacción y formalización del informe del Corte 2. |
