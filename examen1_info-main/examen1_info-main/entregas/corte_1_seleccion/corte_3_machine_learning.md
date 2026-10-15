# Entrega Corte 3: Machine Learning e Integración Final

## 1. Integrantes
- Kristel Yiliana Muñoz Flores
- Emma Aguirre Torres

## 2. Caso seleccionado
*Caso:* `caso_02_pedidos.csv` (Gestión, procesamiento y seguimiento de pedidos logísticos).

## 3. Hallazgo de Process Mining que motiva el modelo
Del análisis de procesos del Corte 2 se identificó que la **Variante 3** incluye la actividad crítica de "Falta de stock" y su respectivo reabastecimiento, además de que el proceso presenta variaciones considerables en los tiempos de ciclo y casos que experimentan re-trabajos. Esto motiva la necesidad de crear un modelo predictivo capaz de anticipar, desde el momento del registro del pedido, si este sufrirá un retraso crítico respecto a la `fecha_prometida`.

## 4. Problema de Machine Learning
* **Pregunta:** ¿Es posible predecir si un pedido superará su fecha de entrega prometida basándonos únicamente en sus atributos iniciales y características operativas del proceso?
* **Variable objetivo:** `atrasado` (1 si la fecha del último evento superó la `fecha_prometida`, 0 en caso contrario).
* **Tipo de problema:** 
  - [x] Clasificación
  - [ ] Regresión
  - [ ] Agrupamiento
  - [ ] Otro
* **Justificación:** Es un problema de clasificación binaria que permite a la empresa identificar de forma anticipada el riesgo de incumplimiento en las entregas para aplicar medidas preventivas.

## 5. Variables predictoras
| Variable | Tipo | Origen | Justificación |
| --- | --- | --- | --- |
| `monto` | Numérica | Dataset original | Valor monetario del pedido. |
| `region` | Categórica | Dataset original | Zona geográfica de destino. |
| `metodo_pago` | Categórica | Dataset original | Forma de pago utilizada por el cliente. |
| `categoria` | Categórica | Dataset original | Tipo de producto solicitado. |
| `canal_venta` | Categórica | Dataset original | Canal por donde ingresó la orden. |
| `cantidad_productos` | Numérica | Dataset original | Número de ítems incluidos en la orden. |
| `total_eventos` | Numérica | Derivada de Process Mining | Conteo total de actividades registradas por caso. |
| `tuvo_falta_stock` | Binaria (0/1) | Derivada de Process Mining | Indicador de si el caso experimentó problemas de inventario. |

*Nota: Se incluyeron variables derivadas del proceso para enriquecer el modelo con información de cuellos de botella.*

## 6. Preparación de datos
* **Valores faltantes:** Se verificó la integridad de las columnas clave y se manejaron adecuadamente.
* **Codificación:** Se aplicó `One-HotEncoder` mediante un `ColumnTransformer` para transformar las variables categóricas en representaciones numéricas de manera limpia.
* **Escalamiento:** No fue necesario escalar las variables numéricas al utilizar un algoritmo basado en árboles (*Random Forest*).
* **Selección de características:** Se excluyeron del conjunto de entrenamiento los tiempos de ciclo totales y las marcas de tiempo posteriores al registro para cumplir con las reglas de prevención de fuga de información.
* **Balance de clases:** La variable objetivo presentó una distribución equilibrada (~50% a tiempo y ~49.5% atrasados).
* **Separación entrenamiento/prueba:** Se utilizó una división estratificada del **80% para entrenamiento y 20% para prueba** (`train_test_split` con `stratify=y` y `random_state=42`).
* **Momento de predicción:** Al registrar el caso (utilizando exclusivamente atributos iniciales y variables de control del proceso sin usar datos del futuro).
* **Variables descartadas por fuga:** Se descartaron la duración total del ciclo, marcas de tiempo de entrega final y conteos acumulados al cierre.

## 7. Modelo o modelos utilizados
### 7.1 Modelo base (baseline)
* **Descripción:** Se entrenó un modelo inicial de *Random Forest* utilizando únicamente las características estáticas iniciales (sin variables de Process Mining).
* **Métrica inicial:** Accuracy de **56.75%**.

### 7.2 Modelos entrenados
* **Algoritmo:** `RandomForestClassifier` (Scikit-Learn).
* **Motivo de selección:** Capacidad robusta para manejar relaciones no lineales y variables tanto numéricas como categóricas codificadas.
* **Parámetros relevantes:** `random_state=42` para reproducibilidad.
* **Procedimiento:** El preprocesamiento y entrenamiento se encapsularon en un `Pipeline`, ajustando el transformador únicamente en el conjunto de entrenamiento.

## 8. Evaluación
* **Clasificación (Métricas comparativas):**
  * **Modelo Base (Solo variables estáticas):**
    * Accuracy global: **56.75%**
  * **Modelo Enriquecido (Con variables de Process Mining - `total_eventos` y `tuvo_falta_stock`):**
    * Accuracy global: **64.00%**

## 9. Interpretación del modelo
* **Resultado:** La incorporación de métricas derivadas del análisis de procesos elevó la precisión global del modelo al 64%. 
* **Variables importantes:** Las variables asociadas al comportamiento operativo (como la cantidad de eventos y la presencia de faltas de stock) resultaron determinantes para predecir si un pedido superará la fecha prometida.
* **Errores y limitaciones:** El modelo todavía presenta margen de mejora en ciertos casos donde las variables iniciales no reflejan imprevistos ocurridos a mitad del proceso.

## 10. Integración Process Mining + Machine Learning
1. **¿Qué hallazgo del proceso dio origen al problema de ML?** La presencia de variantes con cuellos de botella como la falta de stock y re-trabajos.
2. **¿Qué variables del proceso ayudaron al modelo?** El conteo total de eventos (`total_eventos`) y el indicador de falta de stock (`tuvo_falta_stock`).
3. **¿El modelo confirma o contradice lo observado mediante Process Mining?** Confirma que los bloqueos operativos y el aumento de actividades incrementan la probabilidad de retraso.
4. **¿Qué aporta ML que no era visible únicamente en el mapa del proceso?** La capacidad de estimar de manera anticipada y cuantitativa el riesgo de incumplimiento para un pedido nuevo según su perfil.
5. **¿Qué aporta Process Mining que no puede observarse solamente mediante ML?** La visualización gráfica del flujo de trabajo y la localización exacta de los cuellos de botella estructurales.

## 11. Conclusiones
1. Integrar las variables derivadas de la minería de procesos mejoró el rendimiento predictivo del modelo de Machine Learning, elevando el Accuracy del 56.75% al 64.00%.
2. Los problemas operativos críticos, en especial la falta de stock y el incremento en el número de eventos, son los principales detonantes del retraso en las entregas.
3. El cumplimiento de las reglas de prevención de fuga de información garantiza que el modelo sea útil para la toma de decisiones preventivas al registrar el pedido.

## 12. Recomendaciones
1. **Automatizar el control de inventario:** Evitar que pedidos con escasez de mercancía avancen sin stock disponible, reduciendo la activación de variantes complejas.
2. **Optimizar las rutas de despacho:** Ajustar los tiempos asignados a los transportistas en zonas con mayor congestión operativa para disminuir la variabilidad.

## 13. Resumen ejecutivo
Este proyecto abordó el análisis y optimización de la gestión de pedidos combinando minería de procesos y Machine Learning sobre el conjunto de datos `caso_02_pedidos.csv`. En el análisis de procesos se descubrieron las variantes principales y cuellos de botella. Posteriormente, se formuló un modelo predictivo para anticipar retrasos en las entregas, logrando un desempeño superior (64% de precisión) al enriquecerlo con variables operativas extraídas de la traza de eventos.

## 14. Declaración de uso de IA generativa
| Herramienta | Para qué se utilizó | En qué parte del trabajo |
| --- | --- | --- |
| Google Gemini | Asistencia en la estructura de código en Python y redacción de la plantilla del informe. | En los scripts de análisis y estructuración de la documentación del Corte 3. |
