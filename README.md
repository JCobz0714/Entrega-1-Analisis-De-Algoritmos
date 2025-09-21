# Proyecto: Libreria dinamica para reconocimiento y evaluacion de secuencias de ADN usando HMM - Analisis de algoritmos


## Descripcion


Este proyecto implementa una libreria dinamica en C++, integrada a Python mediante SWIG, para el analisis de secuencias de ADN utilizando Modelos Ocultos de Markov (HMM).

El objetivo es resolver dos tareas principales:

Reconocimiento (Viterbi): identificar regiones con alta y baja probabilidad de ser codificantes dentro de una secuencia de ADN.

Evaluacion (Forward): calcular la probabilidad de que una secuencia haya sido generada a partir de un modelo HMM dado (Figura 2 del enunciado).

Finalmente, se compara el desempeño en tiempo de ejecucion de la libreria dinamica frente a una implementacion equivalente en Python.


## procedimiento


**1. Implementacion en C++**

- Se definio una clase HMM que representa el modelo oculto de Markov.

- El modelo esta parametrizado por:

    - A: matriz de transicion entre estados ocultos.

    - B: matriz de emisiones (probabilidad de observar cada nucleotido en cada estado).

    - π: distribucion inicial de estados.

- Se implementaron dos algoritmos principales:

    - Algoritmo Forward: calcula la probabilidad de la secuencia observada bajo el modelo.

    - Algoritmo de Viterbi: encuentra la secuencia de estados mas probable que explica la observacion.

- Para evitar problemas numericos en secuencias largas, todos los calculos se realizan en espacio logaritmico (log-space).

**2. Exposicion mediante SWIG**

- Se creo un archivo de interfaz hmm.i donde se declaran las clases y metodos exportados.

- Con el comando:
```bash
swig -c++ -python hmm.i
```
SWIG genera los archivos wrapper que permiten usar la libreria desde Python.

- Se compilo el código C++ junto con el wrapper en una libreria dinamica compartida.

**3. Uso desde Python**

- Desde Python se importa la libreria como un modulo (import _hmm).

- Se construyen las secuencias de ADN como vectores de enteros (A=0, C=1, G=2, T=3).

- Se pueden ejecutar:

    - hmm.forward(sequence): devuelve la probabilidad logaritmica de la secuencia.
    - hmm.viterbi(sequence): devuelve la ruta de estados mas probable.

**4. Validación y pruebas**

- Se probaron secuencias cortas, largas y generadas aleatoriamente.

- Los resultados se analizaron y se graficaron rutas Viterbi para verificar el reconocimiento de regiones codificantes/no codificantes.

**5. Comparacion de desempeño**

- Se implemento en Python puro una version naive de los algoritmos Forward y Viterbi.

- Se midio el tiempo de ejecucion en ambos enfoques para secuencias de distinta longitud.

- Los resultados muestran que la libreria dinamica en C++ es varias veces mas rapida, especialmente en secuencias largas, confirmando la eficiencia del enfoque nativo frente a la interpretacion en Python.


## Conclusiones


- La libreria dinamica en C++ implementa correctamente los algoritmos de evaluacion (Forward) y reconocimiento (Viterbi).

- La integracion con Python mediante SWIG permite aprovechar el rendimiento del codigo nativo manteniendo la facilidad de uso desde un lenguaje de alto nivel.

- El benchmark confirma la ganancia de desempeño esperada: C++ es significativamente mas rapido que Python puro, lo que valida la utilidad de este tipo de integraciones en proyectos de bioinformatica y analisis de secuencias.