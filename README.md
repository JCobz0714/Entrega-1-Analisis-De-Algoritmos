# Proyecto: Libreria dinamica con SWIG para reconocimiento de secuencias de ADN usando HMM - Analisis de algoritmos

## Descripcion general

Este proyecto implementa una libreria dinamica en C++, expuesta a Python mediante SWIG, para resolver problemas de reconocimiento y evaluacion de secuencias de ADN. El objetivo principal es comparar el desempeño de una implementacion optimizada en C++ frente a una implementacion base en Python, evidenciando las ventajas en rendimiento al usar librerias compiladas.


## Procedimientos realizados

### 1. Definicion del modelo oculto de Markov (HMM)

* Se modelan dos estados ocultos:

  * `H` = region codificante
  * `L` = region no codificante
* El alfabeto de simbolos corresponde a las bases nitrogenadas:

  * `A = 0`, `C = 1`, `G = 2`, `T = 3`
* Se definen:

  * Probabilidades iniciales (`PI`)
  * Matriz de transicion (`A`)
  * Matriz de emision (`B`)

Estas matrices se codifican de forma fija en la libreria, simulando un modelo de referencia.


### 2. Implementacion en C++

En los archivos `hmm.h` y `hmm.cpp` se implementaron dos funciones principales:

* **`evaluar(secuencia)`**
  Aplica el algoritmo Forward para calcular la probabilidad de que una secuencia haya sido generada por el modelo HMM.
  Se utiliza aritmetica logarítmica para mayor estabilidad numerica.

* **`reconocer(secuencia)`**
  Aplica el algoritmo de Viterbi, devolviendo la ruta de estados mas probable (codificante/no codificante) para cada posicion de la secuencia.


### 3. Generacion de la interfaz con SWIG

* Se define un archivo de interfaz `hmm.i` que expone las funciones de C++ a Python.
* Se compila con SWIG y `g++` para generar:

  * `hmm.py` (wrapper en Python)
  * `_hmm.so` (librería compartida en C++).

Esto permite importar el modulo con `import hmm` directamente en Python.


### 4. Uso en Python

Con la libreria compilada, se desarrollaron scripts en Python para:

* Evaluar secuencias cortas y largas.
* Imprimir la ruta mas probable y la probabilidad calculada.
* Visualizar graficamente:

  * Probabilidades de varias secuencias.
  * Ruta de estados asignada a cada base.


### 5. Comparacion de rendimiento

Se realizaron experimentos de rendimiento para comparar:

* **Version Python (naïve)**: implementacion del algoritmo Forward con bucles anidados y listas.
* **Version C++ (libreria dinamica)**: llamada a las funciones optimizadas via SWIG.

Las pruebas se ejecutaron con diferentes longitudes de secuencia (`200`, `2,000`, `20,000`, `50,000`) y distintos numeros de estados ocultos (`2`, `5`, `10`).

Los resultados muestran que la version en C++ logra un speedup de varios órdenes de magnitud frente a la version en Python, especialmente al aumentar el tamaño de las secuencias y la complejidad del modelo.


## Conclusiones

* SWIG facilita la integracion entre C++ y Python, permitiendo reutilizar codigo optimizado en entornos de alto nivel.
* Los algoritmos de Forward y Viterbi fueron implementados de manera eficiente en C++, logrando un rendimiento muy superior respecto a la versión Python.
* La diferencia de tiempo justifica la creacion de la libreria dinamica, mostrando como un mismo problema se resuelve mucho más rapido con una implementacion compilada.