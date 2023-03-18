# Evaluación Final
Alumno: Daniel Martin Arteaga Meléndez

Para realizar esta evaluación de ha hecho uso de la consola a través de MobaXterm. Primero, nos conectamos a DevCloud.

![devcloud_connection](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img0.png)

## Pregunta 0: Clonar el repositorio oneAPI-samples

Nos dirigimos a la ubicación dónde se clonará el repositorio. En este caso, dentro de la carpeta "Computacion_Paralela".

`cd Computacion_Paralela`

Clonamos el repositorio oneAPI-samples.

`git clone https://github.com/oneapi-src/oneAPI-samples.git`

## Pregunta 1: Cambiar al directorio del ejemplo Nbody

Ubicamos la carpeta Nbody en la ruta: `oneAPI-samples/tree/master/DirectProgramming/C++SYCL/N-BodyMethods/Nbody`

`cd DirectProgramming/C++SYCL/N-BodyMethods/Nbody/`

## Pregunta 2: Explicar brevemente el algoritmo de Nbody

Para explicar el algoritmo de Nbody tomaremos como referencia al archivo main.cpp y GSimulation.cpp ubicados en `oneAPI-samples/tree/master/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/src`.

Breve explicación:

1. Se importan librerías y archivos de cabecera.
2. Se definen las variables "n" y "nstep" para el número de partículas y número de pasos de integración.
3. Se define un objeto de la clase GSimulation.
4. De acuerdo a los parámetros de la función principal se usa las funciones respectivas del objeto de simulación para definir el número de partículas y número de pasos de integración de acuerdo a los valores de las variables "n" y "nstep" respectivamente.
5. Empieza la simulación.
9. Se inicializa la posición de todas las partículas aleatoriamente.
10. Se inicializa la velocidad de todas las partículas aleatoriamente.
11. Se inicializa la aceleración de todas las partículas con el valor de 0.
12. Se inicializa la masa de todas las partículas aleatoriamente.
13. Se definen variables relacionadas a la simulación de gravedad.
14. Se crea un queue para el dispositivo seleccionado.
15. Se crea un buffer para el arreglode partículas de tamaño "n".
16. Se asigna memoria para la energía del sistema de partículas.
17. Se inicia un bucle para cada paso de integración.
18. En cada paso se envía el primer kernel del dispositivo para calcular las aceleraciones de todas las partículas y el segundo kernel para calcular las velocidades y posiciones de todas las partículas.
19. En cada paso se actualiza la energía del sistema de partículas.
20. Al finalizar el bucle se calcula el tiempo de ejecución, los FLOPS y con ello el rendimiento promedio.

## Pregunta 3: Acceder en modo interactivo a un nodo de cómputo con GPUs (gen9 o gen11)

Para explicar el algoritmo de Nbody tomaremos como referencia al archivo main.cpp y GSimulation.cpp ubicados en `oneAPI-samples/tree/master/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/src`.


