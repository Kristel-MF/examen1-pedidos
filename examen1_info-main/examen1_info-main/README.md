# Examen Práctico
## Process Mining + Machine Learning

Este repositorio contiene el material base para el desarrollo del examen práctico integrador del curso.

El trabajo se desarrollará **en parejas** y se dividirá en tres cortes:

1. **Corte 1 – Selección y justificación del caso**
2. **Corte 2 – Process Mining**
3. **Corte 3 – Machine Learning e integración final**

Cada pareja deberá analizar los seis casos disponibles, seleccionar uno y desarrollar todo el proyecto sobre ese mismo caso.

## Fechas de entrega

| Corte | Fecha límite | Peso |
|---|---|---|
| Corte 1 – Selección y justificación del caso | **jueves 8 de octubre de 2026, 11:00 a.m.** | 20 % |
| Corte 2 – Process Mining | **martes 13 de octubre de 2026, 23:59** | 35 % |
| Corte 3 – Machine Learning e integración final | **jueves 15 de octubre de 2026, 23:59** | 45 % |

Los criterios de calificación están en [recursos/rubrica.md](recursos/rubrica.md).

## Datos

Cada caso tiene un **event log sintético** en `datos/raw/`. El diccionario de datos de los seis archivos está en [datos/README.md](datos/README.md).

Los datos no están limpios a propósito: la detección y el tratamiento de problemas de calidad forman parte del trabajo.

## Entorno de trabajo: Google Colab

Todo el análisis se realiza en **Python**, en **[Google Colab](https://colab.research.google.com/)**: pm4py para Process Mining, pandas para preparar datos y scikit-learn para Machine Learning. No se aceptan análisis hechos con herramientas gráficas (ProM, Disco, Celonis, etc.) ni con hojas de cálculo.

Colab ya trae pandas, scikit-learn, matplotlib y Graphviz. Solo hace falta instalar pm4py, y eso lo hace la celda de configuración de cada notebook.

### Preparación (una vez por integrante)

1. Inicie sesión en Colab con su cuenta de Google.
2. Conecte Colab con GitHub: **Archivo → Abrir cuaderno → pestaña GitHub**, marque **"Incluir repositorios privados"** y autorice el acceso.
3. Cree en GitHub un **token de acceso personal** para que Colab pueda descargar los datos del repositorio privado:
   - GitHub → *Settings* → *Developer settings* → *Personal access tokens* → *Fine-grained tokens* → *Generate new token*.
   - En *Repository access*, elija solo el repositorio de la pareja.
   - En *Permissions*, dé a *Contents* el permiso **Read-only**.
4. En Colab, abra el panel **Secretos** (ícono de llave 🔑 a la izquierda) y cree un secreto llamado `GITHUB_TOKEN` con el valor del token. Active el acceso del notebook a ese secreto.

> Nunca escriba el token dentro de una celda ni lo suba al repositorio.

### Celda de configuración

**Todo notebook debe comenzar con esta celda.** Cambie `REPO` por el usuario y el nombre del repositorio de la pareja:

```python
# Configuración del entorno en Google Colab
from google.colab import userdata

REPO = "usuario/nombre-del-repositorio"
TOKEN = userdata.get("GITHUB_TOKEN")

!git clone -q https://{TOKEN}@github.com/{REPO}.git /content/repo
%cd /content/repo/scripts
!pip install -q pm4py
```

La celda descarga una copia del repositorio en Colab, se ubica en `scripts/` e instala pm4py. Las rutas `../datos/raw/` y `../datos/processed/` funcionan igual que en el repositorio.

Para comprobar que todo funciona, abra [scripts/00_inicio_pm4py.ipynb](scripts/00_inicio_pm4py.ipynb) desde Colab (pestaña GitHub) y ejecútelo completo.

### Cómo guardar el trabajo

La copia del repositorio dentro de Colab **es temporal**: se borra al cerrar la sesión.

- **Notebooks:** guárdelos en el repositorio con **Archivo → Guardar una copia en GitHub**. Elija el repositorio de la pareja, la rama `main` y una ruta dentro de `scripts/`, por ejemplo `scripts/02_preparacion_event_log.ipynb`. Escriba un mensaje de *commit* descriptivo. Cada guardado queda como un *commit* de quien lo hizo.
- **Datos procesados (`datos/processed/`):** no es necesario subirlos. Se regeneran al ejecutar los notebooks, como exige la regla de reproducibilidad.
- **Documentos de entrega (`entregas/`):** edítelos directamente en GitHub (ícono de lápiz ✏️) o súbalos con *Add file → Upload files*.
- **Imágenes para las entregas:** descárguelas desde Colab y súbalas a `entregas/<corte>/img/` en GitHub.

> Si trabajan los dos al mismo tiempo, cada integrante debe usar **su propio notebook**. Si los dos guardan el mismo archivo, el último guardado reemplaza al anterior.

### Uso fuera de Colab (opcional)

Quien prefiera trabajar en su computadora puede instalar las librerías con `pip install -r requirements.txt` y el programa [Graphviz](https://graphviz.org/download/). En ese caso, omita la celda de configuración de Colab. Los notebooks entregados deben ejecutarse correctamente en Colab.

## Cómo entregar

Cada pareja trabaja en **su propio repositorio de GitHub**:

1. Un integrante crea un repositorio **privado** con el contenido de este material.
2. Agrega como colaboradores a su compañero o compañera y al docente: usuario de GitHub **[`juangamboaabarca`](https://github.com/juangamboaabarca)**.
3. Ambos integrantes trabajan en Colab y guardan sus notebooks en el repositorio desde su propia cuenta (ver *Cómo guardar el trabajo*).
4. Cada entrega se hace en su carpeta de `entregas/`. Copie allí la plantilla del corte, con el mismo nombre, y complétela.
5. Se califica el **último commit anterior a la fecha límite**. Los cambios posteriores no se consideran.
6. Antes de la primera entrega, envíe el enlace del repositorio por el **aula virtual** del curso.

Las entregas tardías se rigen por el reglamento del curso.

---

## Objetivo general

Desarrollar un análisis integral basado en datos que permita:

- comprender una problemática realista;
- seleccionar y justificar una fuente de datos;
- preparar un Event Log;
- descubrir y analizar procesos mediante Process Mining;
- identificar patrones, variantes y posibles cuellos de botella;
- formular un problema de Machine Learning;
- construir y evaluar un modelo;
- integrar los hallazgos de Process Mining y Machine Learning;
- formular conclusiones y recomendaciones sustentadas en evidencia.

---

## Estructura del repositorio

```text
Examen_Integrador_Process_Mining_ML/
├── README.md
├── .gitignore
├── casos/
│   ├── caso_01_salud.md
│   ├── caso_02_pedidos.md
│   ├── caso_03_creditos.md
│   ├── caso_04_soporte_ti.md
│   ├── caso_05_academico.md
│   └── caso_06_mantenimiento.md
├── plantillas/
│   ├── corte_1_seleccion_caso.md
│   ├── corte_2_process_mining.md
│   └── corte_3_machine_learning.md
├── datos/
│   ├── README.md              ← diccionario de datos
│   ├── raw/                   ← un event log por caso (no modificar)
│   └── processed/             ← datos transformados por la pareja
├── entregas/
│   ├── corte_1_seleccion/
│   ├── corte_2_process_mining/
│   └── corte_3_machine_learning/
├── recursos/
│   ├── estructura_event_log.md
│   ├── criterios_generales.md
│   ├── rubrica.md
│   ├── fuga_de_informacion.md
│   └── glosario.md
├── scripts/
│   ├── README.md
│   └── 00_inicio_pm4py.ipynb  ← notebook de arranque
└── requirements.txt
```

---

# Reglas generales

- El trabajo se realiza en parejas.
- Cada pareja debe seleccionar **un único caso**.
- La selección del caso debe mantenerse durante los tres cortes.
- No se debe seleccionar un algoritmo de Machine Learning antes de comprender el proceso y los datos.
- Cada decisión técnica debe estar justificada.
- Se evaluará la interpretación de los resultados, no solamente la ejecución de herramientas.
- El repositorio de trabajo de cada pareja debe conservar evidencia del proceso realizado.
- Ambos integrantes deben participar en el desarrollo.
- Los archivos de `datos/raw/` no se modifican: toda transformación se hace con scripts o notebooks guardados en `scripts/`.
- Todo el análisis se hace en Python y debe ser reproducible.
- Se permite usar IA generativa (ChatGPT, Claude, Copilot, etc.). Cada entrega debe incluir la **declaración de uso de IA** de la última sección de la plantilla. Ambos integrantes deben poder explicar cualquier parte del trabajo cuando el docente lo solicite.

---

# Corte 1 – Selección y justificación del caso

Cada pareja debe:

1. revisar los seis casos;
2. comparar las problemáticas;
3. seleccionar un caso;
4. justificar técnicamente la elección;
5. identificar el posible proceso;
6. proponer el Case ID, Activity y Timestamp;
7. formular preguntas iniciales de análisis;
8. identificar posibles variables útiles para Process Mining;
9. proponer una posible línea futura de Machine Learning, **sin desarrollar todavía el modelo**.

La entrega se realizará utilizando la plantilla:

`plantillas/corte_1_seleccion_caso.md`

El archivo final deberá colocarse en:

`entregas/corte_1_seleccion/`

---

# Corte 2 – Process Mining

En esta etapa la pareja deberá:

- preparar los datos;
- construir o validar el Event Log;
- analizar el proceso;
- identificar variantes;
- analizar tiempos;
- detectar cuellos de botella;
- verificar el cumplimiento del proceso esperado (conformance);
- identificar comportamientos relevantes;
- documentar los hallazgos.

La entrega se realizará utilizando:

`plantillas/corte_2_process_mining.md`

El archivo final deberá colocarse en:

`entregas/corte_2_process_mining/`

---

# Corte 3 – Machine Learning

A partir de los hallazgos obtenidos mediante Process Mining, la pareja deberá:

- formular un problema de Machine Learning;
- definir la variable objetivo y el momento de predicción (ver `recursos/fuga_de_informacion.md`);
- seleccionar variables predictoras;
- preparar los datos;
- seleccionar uno o más algoritmos y compararlos contra un modelo base;
- entrenar y evaluar el modelo;
- interpretar los resultados;
- integrar Process Mining y Machine Learning;
- formular conclusiones y recomendaciones.

La entrega se realizará utilizando:

`plantillas/corte_3_machine_learning.md`

El archivo final deberá colocarse en:

`entregas/corte_3_machine_learning/`

---

# Producto final

Al finalizar el proyecto, el repositorio de cada pareja deberá permitir comprender claramente:

**Problema → Datos → Event Log → Process Mining → Hallazgos → Machine Learning → Resultados → Recomendaciones**
