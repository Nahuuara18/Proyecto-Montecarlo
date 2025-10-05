# Orbital and Asteroid Impact Simulator

## 📜 Introduction

This project is an interactive web application that combines a solar system orbital simulator with a detailed simulator for the effects of an asteroid impact on Earth. The application allows users to visualize the orbits of planets and asteroids in 3D, load data of real Near-Earth Objects (NEOs) from the NASA API, create their own simulated asteroids, and, most importantly, calculate and visualize the consequences of a potential collision.

The main interface is based on a 3D visualization built with **Three.js**, while the impact simulation module uses an interactive **Leaflet.js** map to display the effects geographically.

## ✨ Key Features

* **3D Solar System Visualization**: Representation of the Sun, inner and outer planets with their orbits.
* **Time Control**: Pause, resume, accelerate, and rewind the simulation time, or synchronize it with the current live time.
* **Real Data Integration**: Load and visualize the orbits of near-Earth asteroids using the **NASA NEO Web Service** API.
* **Asteroid Creation**: Launch simulated asteroids by defining their Keplerian orbital parameters (semi-major axis, eccentricity, inclination, etc.) and their size.
* **Hazard Analysis**: Calculates a "Danger Index D" for simulated asteroids based on their orbit and size for a simplified risk assessment.
* **Collision Prediction**: A tool analyzes an asteroid's orbit on a specific date to determine if it intersects with Earth's "critical sphere of influence" and calculates the potential geographical impact coordinates.
* **Advanced Impact Simulator**:
    * Models **atmospheric entry**, including asteroid fragmentation.
    * Calculates impact effects on land or water:
        * **Crater** diameter.
        * **Shock wave** radius (overpressure in PSI).
        * **Thermal radiation** range (1st, 2nd, and 3rd-degree burns).
        * Equivalent seismic magnitude on the **Richter scale**.
        * **Tsunami** generation for ocean impacts.
    * Estimates the number of **casualties** (fatalities and injuries) based on the area's population density.
* **Interactive Map**: Visualizes all effects on a satellite world map, allowing the user to select any point on the planet to run "what-if" scenarios.

---

## 🚀 How to Use the Application

### Basic Navigation and Controls

* **Rotate the camera**: Click and drag with the left mouse button.
* **Zoom**: Use the mouse wheel.
* **Select an object**: Click on a planet or asteroid in the 3D view. Its information will appear in the top-left panel. You can also use the dropdown menu in the top-right corner.
* **Time controls**: Use the buttons in the bottom-center panel to control the flow of time in the simulation.

### Simulating an Asteroid Impact

This is the main functionality that merges both programs.

1.  **Open the Analysis Panel**: In the left-side panel, find the "**🔭 Calculate Future Position**" section.
2.  **Select Data**:
    * Choose a **date and time** for the analysis.
    * From the dropdown menu, **select an asteroid**. This menu will only show asteroids with complete orbital data (loaded from NASA or created by you).
3.  **Calculate Position**: Click the **`Calculate Positions`** button.
4.  **Analyze Results**: The results panel will update with the following information:
    * The calculated positions of the asteroid and Earth.
    * The distance between them.
    * An **orbital impact analysis**. The simulator will draw a red sphere around the Earth (its critical sphere of influence) and trace the asteroid's orbit to see if they intersect.
5.  **Launch the Impact Simulation**:
    * **If a collision is detected**: The panel will display the impact coordinates (latitude/longitude) and a red button will appear: **`Simulate Impact at Collision Point`**. Clicking it will open the map and automatically run the simulation at that point.
    * **If no collision is detected**: The panel will indicate this, but a button will still appear: **`Simulate Impact (Manual Mode)`**. This allows you to explore the impact scenario anyway.
6.  **Interact with the Map**:
    * Once in the map view, the impact effects will be drawn with animated circles.
    * The right-side panel will show a **detailed summary** of all calculated effects.
    * You can use the "Visualize Effect" dropdown to filter the layers on the map (crater, shockwave, etc.).
    * **Click anywhere else on the map** to re-run the simulation at a new location of your choice.
    * To return to the 3D orbital simulator, click the **`X`** in the top-right corner of the map.

---

## 🏗️ Code Structure

The project is contained within a single `HTML` file, organized into three main parts:

### 1. HTML

* **Main Interface**: Contains the `div` elements for the control panels (`#left-controls`), information display (`#top-left-hud`), time controls (`#bottom-center-hud`), and the NASA asteroid panel (`#nasa-panel`).
* **Impact Simulator Container**: The `div` with the id `#map-container`. It is hidden by default and is displayed when an impact simulation is initiated. It contains the map and its own control and result panels.

### 2. CSS

* **Base Styles**: The original styles from `montecarlo_orbital.html`, which define the general appearance of the 3D interface.
* **Impact Simulator Styles**: The styles from `asteroidefinal.html`. These have been carefully **isolated and scoped** to apply only to elements within `#map-container`. This was done to prevent style conflicts and ensure that the map panels look exactly as in the original design, without affecting the main interface.

### 3. JavaScript (within `<script type="module">`)

The code is logically divided into several sections:

* **Impact Simulation Logic**:
    * **Physical Constants**: `ASTEROID_DENSITY`, `INITIAL_VELOCITY`, etc.
    * **`runAtmosphericEntrySim()`**: A key function that simulates the asteroid's passage through the atmosphere.
    * **Effect Calculation Functions**: `calculateCraterDiameter()`, `calculateAirblastRadius()`, `calculateThermalRadius()`, `calculateTsunamiIsolines()`, `estimateCasualties()`.
    * **`onMapClick()`**: The main event handler for the map. It's triggered on click and executes the entire chain of effect calculation and visualization.
    * **`launchImpactSimulator()`**: The "bridge" function called from the main interface to hide the 3D view, display the map, and pass the asteroid's parameters.

* **Main Orbital Simulator Logic**:
    * **Three.js Setup**: Initialization of the scene, camera, renderer, lights, and orbital controls.
    * **Celestial Body Creation**: Functions to create the Sun, planets, and their orbits.
    * **`animate()`**: The main application loop. It updates the positions of planets, Earth's rotation, asteroid positions, and renders the scene on each frame.
    * **Orbital Mechanics and Prediction**:
        * `calculateOrbitalPosition()`: Calculates the position of a body at a given time from its Keplerian elements.
        * `analizarInterseccionOrbitaEsfera()`: The function that determines if an orbit intersects Earth's sphere of influence and calculates the impact coordinates.
    * **NASA API Integration**:
        * `fetchNasaData()`: Gets the list of nearby NEOs.
        * `loadNeoDetails()`: Loads the detailed orbital data for a specific asteroid.
    * **User Interface Handling**: All the code that manages button clicks, slider changes, and updates to the information panels.

---

## 📚 Dependencies and External APIs

* **Three.js**: A library for creating and rendering 3D graphics in the browser.
* **Leaflet.js**: A library for creating interactive maps.
* **NASA Near Earth Object Web Service**: The API used to obtain data on real asteroids.
* **Is On Water API**: A simple API used to determine if a pair of geographic coordinates is on land or water, to decide whether to calculate a tsunami.

---
---

# README (Español)

## 📜 Introducción

Este proyecto es una aplicación web interactiva que combina un simulador orbital del sistema solar con un detallado simulador de los efectos de impacto de un asteroide en la Tierra. La aplicación permite a los usuarios visualizar las órbitas de planetas y asteroides en 3D, cargar datos de Objetos Cercanos a la Tierra (NEOs) reales desde la API de la NASA, crear sus propios asteroides simulados y, lo más importante, calcular y visualizar las consecuencias de una posible colisión.

La interfaz principal se basa en una visualización 3D construida con **Three.js**, mientras que el módulo de simulación de impacto utiliza un mapa interactivo de **Leaflet.js** para mostrar los efectos geográficamente.

## ✨ Características Principales

* **Visualización 3D del Sistema Solar**: Representación del Sol, los planetas interiores y exteriores con sus órbitas.
* **Control del Tiempo**: Pausa, reanuda, acelera y retrocede el tiempo de la simulación, o sincronízalo con la hora actual en vivo.
* **Integración de Datos Reales**: Carga y visualiza las órbitas de asteroides cercanos a la Tierra utilizando la API **NASA NEO Web Service**.
* **Creación de Asteroides**: Lanza asteroides simulados definiendo sus parámetros orbitales keplerianos (semieje mayor, excentricidad, inclinación, etc.) y su tamaño.
* **Análisis de Peligro**: Calcula un "Índice de Peligro D" para asteroides simulados basado en su órbita y tamaño para una evaluación de riesgo simplificada.
* **Predicción de Colisiones**: Una herramienta analiza la órbita de un asteroide en una fecha específica para determinar si se cruza con la "esfera de influencia crítica" de la Tierra y calcula las coordenadas geográficas del impacto potencial.
* **Simulador de Impacto Avanzado**:
    * Modela la **entrada atmosférica**, incluyendo la fragmentación del asteroide.
    * Calcula los efectos del impacto en tierra o agua:
        * Diámetro del **cráter**.
        * Radio de la **onda de choque** (sobrepresión en PSI).
        * Alcance de la **radiación térmica** (quemaduras de 1er, 2º y 3er grado).
        * Magnitud del **sismo** equivalente en la escala de Richter.
        * Generación de **Tsunamis** para impactos oceánicos.
    * Estima el número de **víctimas** (fatalidades y heridos) basado en la densidad de población de la zona.
* **Mapa Interactivo**: Visualiza todos los efectos en un mapa satelital del mundo, permitiendo al usuario seleccionar cualquier punto del planeta para realizar simulaciones "qué pasaría si".

---

## 🚀 Cómo Usar la Aplicación

### Navegación y Controles Básicos

* **Rotar la cámara**: Haz clic y arrastra con el botón izquierdo del mouse.
* **Hacer zoom**: Usa la rueda del mouse.
* **Seleccionar un objeto**: Haz clic en un planeta o asteroide en la vista 3D. Su información aparecerá en el panel superior izquierdo. También puedes usar el menú desplegable en la esquina superior derecha.
* **Controles de tiempo**: Utiliza los botones en el panel inferior central para controlar el flujo del tiempo en la simulación.

### Simulación de un Impacto de Asteroide

Esta es la funcionalidad principal que fusiona ambos programas.

1.  **Abrir el Panel de Análisis**: En el panel de la izquierda, busca la sección "**🔭 Calcular Posición Futura**".
2.  **Seleccionar Datos**:
    * Elige una **fecha y hora** para el análisis.
    * En el menú desplegable, **selecciona un asteroide**. Este menú solo mostrará asteroides que tengan datos orbitales completos (cargados desde la NASA o creados por ti).
3.  **Calcular Posición**: Haz clic en el botón **`Calcular Posiciones`**.
4.  **Analizar Resultados**: El panel de resultados se actualizará con la siguiente información:
    * Las posiciones calculadas del asteroide y la Tierra.
    * La distancia entre ellos.
    * Un **análisis de impacto orbital**. El simulador dibujará una esfera roja alrededor de la Tierra (su radio de influencia crítica) y trazará la órbita del asteroide para ver si se cruzan.
5.  **Lanzar la Simulación de Impacto**:
    * **Si se detecta una colisión**: El panel mostrará las coordenadas (latitud/longitud) del impacto y aparecerá un botón rojo: **`Simular Impacto en Punto de Colisión`**. Al hacer clic, el mapa se abrirá y ejecutará la simulación automáticamente en ese punto.
    * **Si no se detecta colisión**: El panel lo indicará, pero aun así aparecerá un botón: **`Simular Impacto (Modo Manual)`**. Esto te permite explorar el escenario de impacto de todos modos.
6.  **Interactuar con el Mapa**:
    * Una vez en la vista del mapa, los efectos del impacto se dibujarán con círculos animados.
    * El panel de la derecha mostrará un **resumen detallado** de todos los efectos calculados.
    * Puedes usar el menú desplegable "Visualizar Efecto" para filtrar las capas en el mapa (cráter, onda de choque, etc.).
    * **Haz clic en cualquier otro lugar del mapa** para volver a ejecutar la simulación en una nueva ubicación de tu elección.
    * Para volver al simulador orbital 3D, haz clic en la **`X`** en la esquina superior derecha del mapa.

---

## 🏗️ Estructura del Código

El proyecto está contenido en un único archivo `HTML` que se organiza en tres partes principales:

### 1. HTML

* **Interfaz Principal**: Contiene los `div` para los paneles de control (`#left-controls`), información (`#top-left-hud`), controles de tiempo (`#bottom-center-hud`) y el panel de asteroides de la NASA (`#nasa-panel`).
* **Contenedor del Mapa de Impacto**: El `div` con id `#map-container`. Está oculto por defecto y se muestra cuando se inicia una simulación de impacto. Contiene el mapa y sus propios paneles de control y resultados.

### 2. CSS

* **Estilos Base**: Los estilos originales de `montecarlo_orbital.html`, que definen la apariencia general de la interfaz 3D.
* **Estilos del Simulador de Impacto**: Los estilos de `asteroidefinal.html`. Han sido cuidadosamente **aislados y adaptados** para aplicarse únicamente a los elementos dentro de `#map-container`. Esto se hizo para evitar conflictos de estilo y asegurar que los paneles del mapa se vean exactamente como en el diseño original, sin afectar la interfaz principal.

### 3. JavaScript (dentro de `<script type="module">`)

El código está dividido lógicamente en varias secciones:

* **Lógica de Simulación de Impacto**:
    * **Constantes Físicas**: `ASTEROID_DENSITY`, `INITIAL_VELOCITY`, etc.
    * **`runAtmosphericEntrySim()`**: Función clave que simula el paso del asteroide por la atmósfera.
    * **Funciones de Cálculo de Efectos**: `calculateCraterDiameter()`, `calculateAirblastRadius()`, `calculateThermalRadius()`, `calculateTsunamiIsolines()`, `estimateCasualties()`.
    * **`onMapClick()`**: El controlador de eventos principal para el mapa. Se activa al hacer clic y ejecuta toda la cadena de cálculo y visualización de efectos.
    * **`launchImpactSimulator()`**: La función "puente" que se llama desde la interfaz principal para ocultar la vista 3D, mostrar el mapa y pasar los parámetros del asteroide.

* **Lógica del Simulador Orbital (Principal)**:
    * **Configuración de Three.js**: Inicialización de la escena, cámara, renderizador, luces y controles orbitales.
    * **Creación de Cuerpos Celestes**: Funciones para crear el Sol, planetas y sus órbitas.
    * **`animate()`**: El bucle principal de la aplicación. Actualiza las posiciones de los planetas, la rotación de la Tierra, las posiciones de los asteroides y renderiza la escena en cada fotograma.
    * **Mecánica Orbital y Predicción**:
        * `calculateOrbitalPosition()`: Calcula la posición de un cuerpo en un instante de tiempo a partir de sus elementos keplerianos.
        * `analizarInterseccionOrbitaEsfera()`: La función que determina si una órbita intersecta la esfera de influencia de la Tierra y calcula las coordenadas de impacto.
    * **Integración con la API de la NASA**:
        * `fetchNasaData()`: Obtiene la lista de NEOs cercanos.
        * `loadNeoDetails()`: Carga los datos orbitales detallados de un asteroide específico.
    * **Manejo de la Interfaz de Usuario**: Todo el código que gestiona los clics en botones, cambios en los selectores y la actualización de los paneles de información.

---

## 📚 Dependencias y APIs Externas

* **Three.js**: Biblioteca para la creación y renderizado de gráficos 3D en el navegador.
* **Leaflet.js**: Biblioteca para la creación de mapas interactivos.
* **NASA Near Earth Object Web Service**: API utilizada para obtener datos de asteroides reales.
* **Is On Water API**: Una API simple utilizada para determinar si un par de coordenadas geográficas está sobre tierra o agua, para decidir si se debe calcular un tsunami.