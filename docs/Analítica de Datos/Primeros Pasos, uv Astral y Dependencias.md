Primeros pasos: UV Astral y dependencias
## ¿Qué es UV Astral?
UV es una herramienta moderna, súper rápida (escrita en Rust), que sirve para gestionar todo lo relacionado con Python en un proyecto: instalar versiones de Python, crear entornos virtuales, instalar librerías y manejar dependencias.
Antes existían varias herramientas separadas para hacer todo eso (pip, venv, pyenv, poetry, etc.). UV las reemplaza y unifica todas en una sola y, además, lo hace entre 10 y 100 veces más rápido.

### Analogía: 
UV es como la navaja suiza moderna del desarrollador Python. Antes había que cargar con un cuchillo, unas tijeras, un destornillador y un sacacorchos por separado. Ahora hay una sola herramienta compacta que hace todo y mejor.

## ¿Qué es un entorno virtual?
Un entorno virtual (environment) es como una caja independiente y aislada para cada proyecto. Dentro de cada caja se puede tener:

Una versión específica de Python.
Las librerías que ese proyecto necesita.
Las versiones exactas de cada librería.

**Lo importante:** lo que pasa dentro de una caja no afecta a las otras cajas ni al sistema general de la computadora.
Visualización
Computadora
```
├── Entorno Proyecto A (caja 1)
│   ├── Python 3.10
│   ├── Pandas 1.5
│   └── Matplotlib 3.5
│
├── Entorno Proyecto B (caja 2)
│   ├── Python 3.12
│   ├── Pandas 2.0
│   └── NumPy 2.0
│
└── Entorno Proyecto C (caja 3)
    ├── Python 3.11
    └── otras librerías...
```

## ¿Por qué cada proyecto debe tener su propio entorno?
Porque en el mundo real, cada proyecto puede necesitar versiones diferentes de las mismas librerías.
Ejemplo práctico

**Proyecto A:** análisis de ventas que usa Pandas 1.5.  
**Proyecto B:** proyecto nuevo que usa funciones que solo existen en Pandas 2.0.

Si todo se instala globalmente, solo se puede tener UNA versión de Pandas a la vez. Si se instala la 2.0, se rompe el Proyecto A. Si se queda con la 1.5, no funciona el Proyecto B.
Las 3 ventajas de usar entornos

### Aislamiento:  
Lo que se instala en un proyecto no rompe los otros. Se puede experimentar tranquilo.
Reproducibilidad: el proyecto se puede replicar en otra computadora o servidor con el entorno exacto. El código funciona igual en cualquier parte.
Estándar profesional: en empresas, esto es obligatorio. Ningún equipo profesional trabaja sin entornos.


### Analogía: 
Los entornos son como las maletas individuales que se arman para cada viaje. No se mete ropa de invierno y traje de baño en la misma maleta si se va a destinos distintos. Cada viaje (proyecto), su maleta (entorno).


## Tabla completa de comandos UV Astral
Comando¿Para qué sirve?uv --versionVerificar que UV está instalado correctamenteuv python installInstalar la última versión de Pythonuv python install 3.12Instalar una versión específica de Pythonuv python listVer qué versiones de Python están disponiblesuv initInicializar un nuevo proyecto en la carpeta actual (crea pyproject.toml, main.py, README.md)uv init nombre-proyectoCrear un proyecto nuevo dentro de una carpeta con ese nombreuv venv .venvCrear el entorno virtual del proyecto (en una carpeta llamada .venv)source .venv/bin/activateActivar el entorno virtual (Linux/Mac).venv\Scripts\activateActivar el entorno virtual (Windows)deactivateDesactivar el entorno virtual cuando se termine de trabajaruv add pandasAgregar una librería al proyecto (ejemplo: pandas)uv add pandas matplotlib numpyAgregar varias librerías a la vezuv remove pandasQuitar una librería del proyectouv syncInstalar todas las dependencias listadas en pyproject.toml (útil al clonar un proyecto)uv run main.pyEjecutar un archivo Python usando el entorno del proyectouv pip listVer qué librerías están instaladas en el entorno actual

Flujo típico de trabajo
Cuando se empieza un proyecto nuevo desde cero, el orden lógico de los comandos es:
bash# 1. Abrir terminal en la carpeta donde se va a trabajar

| Comando | ¿Para qué sirve? |
|---------|------------------|
| `uv --version` | Verificar que UV está instalado correctamente |
| `uv python install` | Instalar la última versión de Python |
| `uv python install 3.12` | Instalar una versión específica de Python |
| `uv python list` | Ver qué versiones de Python están disponibles |
| `uv init` | Inicializar un nuevo proyecto en la carpeta actual (crea `pyproject.toml`, `main.py`, `README.md`) |
| `uv init nombre-proyecto` | Crear un proyecto nuevo dentro de una carpeta con ese nombre |
| `uv venv .venv` | Crear el entorno virtual del proyecto (en una carpeta llamada `.venv`) |
| `source .venv/bin/activate` | Activar el entorno virtual (Linux/Mac) |
| `.venv\Scripts\activate` | Activar el entorno virtual (Windows) |
| `deactivate` | Desactivar el entorno virtual cuando se termine de trabajar |
| `uv add pandas` | Agregar una librería al proyecto (ejemplo: pandas) |
| `uv add pandas matplotlib numpy` | Agregar varias librerías a la vez |
| `uv remove pandas` | Quitar una librería del proyecto |
| `uv sync` | Instalar todas las dependencias listadas en `pyproject.toml` (útil al clonar un proyecto) |
| `uv run main.py` | Ejecutar un archivo Python usando el entorno del proyecto |
| `uv pip list` | Ver qué librerías están instaladas en el entorno actual |


Tips importantes
Cómo saber si el entorno está activado
Cuando un entorno virtual está activado, aparece (.venv) al inicio de la línea de la terminal:
(.venv) usuario@equipo:~/mi-proyecto$
Si NO aparece ese (.venv), significa que se está trabajando con el Python global del sistema, lo cual hay que evitar.
Conceptos importantes

Paquete, librería y dependencia: en la práctica diaria se usan como sinónimos. Son herramientas externas que se agregan al proyecto.
pyproject.toml: archivo donde UV guarda la configuración del proyecto y las dependencias. Es el equivalente Python del package.json de Node.js.
.venv/: carpeta donde vive el entorno virtual. Esta carpeta NO se sube a GitHub (debe estar en el .gitignore).

Resumiendo en una frase

UV Astral es la herramienta que se usa para crear y gestionar entornos virtuales, que son cajas aisladas donde cada proyecto tiene sus propias versiones de Python y librerías sin interferir entre sí.