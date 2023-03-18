# Evaluación Final
Alumno: Daniel Martin Arteaga Meléndez

Para realizar esta evaluación de ha hecho uso de la consola a través de MobaXterm. Primero, nos conectamos a DevCloud.

![devcloud_connection](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img0.png)

## Pregunta 0: Clonar el repositorio oneAPI-samples

Nos dirigimos a la ubicación dónde se clonará el repositorio. En este caso, dentro de la carpeta "Computacion_Paralela".

`cd Computacion_Paralela`

Clonamos el repositorio oneAPI-samples.

`git clone https://github.com/oneapi-src/oneAPI-samples.git`

![repo_clonning](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img1.png)

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

Antes de conectarnos a un nodo, buscaremos qué nodo se encuentra libre para poder ser usado utilizando el siguiente comando:

`pbsnodes | grep -B4 "gen11"`.

![search_available_node](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img2.png)

Observamos que el nodo s019-n022 es uno de los que se encuentran disponibles y nos conectamos utilizando el siguiente comando:

`qsub -I -L nodes=s019-n022:gpu:ppn=2 -d .`

Una vez conectados usamos el comando `sycl-ls` para verificar que el nodo cuenta con GPU.

![node_connection](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img3.png)

Ahora que nos encontramos conectados, antes de ejecutar el código es necesario crear el archivo de compilación. Para ello crearemos el archivo "build.sh" desde un Jupyter Notebook usando el comando "writefile".

![build](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img4.png)

También es necesario crear el archivo de ejecución "run.sh". De igual forma, se utilizó un Jupyter Notebook con el comando "writefile".

![run](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img5.png)

Una vez que ambos archivos fueron creados se procede a realizar la compilación y ejecución del código. Si no se encuentra en la dirección de la carpeta Nbody, se cambia el directorio a dicha ubicación.

`cd Computacion_Paralela/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/`

El siguiente paso es realizar la compilación a través del siguiente comando:

`qsub -l nodes=1:gpu:ppn=2 -d . buil.sh`

Seguidamente se realiza la ejecución del código usando:

`qsub -l nodes=1:gpu:ppn=2 -d . run.sh`

Debido a que la ejecución de ambos comandos puede tardar un poco es aconsejable revisar el estado de ejecución empleando el comando `qstat`

![building_running](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img6.png)

También puedes verificar que la ejecución de ambos comandos terminó revisando los archivos de errores y de resultados que se producen en la ubicación actual. A continuación se muestra el resultado de la compilación del código.

![building_results](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img7.png)

El resultado de la ejecución del código es el siguiente:

![running_results](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img8.png)

Para explicar el algoritmo de Nbody tomaremos como referencia al archivo main.cpp y GSimulation.cpp ubicados en `oneAPI-samples/tree/master/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/src`.


