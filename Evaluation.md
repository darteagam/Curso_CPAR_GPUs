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

Ahora que nos encontramos conectados, antes de ejecutar el algoritmo es necesario crear el archivo de compilación. Para ello crearemos el archivo "build.sh" desde un Jupyter Notebook usando el comando "writefile".

![build](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img4.png)

También es necesario crear el archivo de ejecución "run.sh". De igual forma, se utilizó un Jupyter Notebook con el comando "writefile".

![run](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img5.png)

Una vez que ambos archivos fueron creados se procede a realizar la compilación y ejecución del algoritmo. Si no se encuentra en la dirección de la carpeta Nbody, se cambia el directorio a dicha ubicación.

`cd Computacion_Paralela/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/`

El siguiente paso es realizar la compilación a través del siguiente comando:

`qsub -l nodes=1:gpu:ppn=2 -d . buil.sh`

Seguidamente se realiza la ejecución del algoritmo usando:

`qsub -l nodes=1:gpu:ppn=2 -d . run.sh`

Debido a que la ejecución de ambos comandos puede tardar un poco es aconsejable revisar el estado de ejecución empleando el comando `qstat`

![building_running](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img6.png)

También puedes verificar que la ejecución de ambos comandos terminó revisando los archivos de errores y de resultados que se producen en la ubicación actual. A continuación se muestra el resultado de la compilación del algoritmo.

![building_results](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img7.png)

El resultado de la ejecución del algoritmo es el siguiente:

![running_results](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img8.png)

## Pregunta 4: Realizar un análisis de GPU Hotspots con VTune

El primer paso es cambiar de ubicación a `build/src/` dentro del directorio actual.

`cd build/src/`

Ahora se ejecuta el comando para obtener los resultados de un análisis de GPU Hotspots con VTune:

`vtune -collect gpu-hotspots -- /home/u185961/Computacion_Paralela/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/nbody`

![vtune_hs1](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img9.png)

Cuando finaliza se muestra una pantalla similar a la siguiente:

![vtune_hs2](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img10.png)

Al finalizar se habrá creado una carpeta "r000gh" que debemos descargar para poder ver los resultados con VTune. Para descargar esta carpeta se abre una nueva pestaña en MobaXterm donde se cambia a la ubicación donde deseamos descargar el archivo y se ejecuta el siguiente comando:

`scp -r devcloud:/home/u185961/Computacion_Paralela/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/r000gh .`

También es conveniente descargar el archivo "GSimulation.cpp" que se encuentra en `/home/u185961/Computacion_Paralela/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/src` para poder inspeccionar los GPU Hotspots en las líneas de código de nuestro algoritmo.

El siguiente paso es abrir la aplicación offline de VTune y seleccionar "Open Results..." en la parte superior derecha de la pantalla. Se selecciona la ubicación del archivo "r000gh.vtune" dentro de la carpeta "r000gh" que acabamos de descargar.

Después de unos segundos se visualizará los resultados del análisis de GPU Hotspots con VTune. La sección de resumen tiene las siguientes secciones:

1. Elapsed Time: muestra el tiempo transcurrido total y el tiempo de ejecución en la GPU.
2. EU Array Stalled/Idle: métrica que indica el tiempo promedio que las unidades de ejecución (EUs) han estado paradas o inactivas.

![vtune_r1](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img11.png)

3. Bandwidth Utilization Histogram: sección para explorar la utilización de ancho de banda a lo largo del tiempo de ejecución.

![vtune_r2](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img12.png)

4. Collection and PlatformInfo: sección que provee información de la colección.

![vtune_r3](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img13.png)

Los hotspots identificados de acuerdo con esta sección de resumen son:

1. La métrica "EU Array Stalled/Idle" es alta (20.1% del tiempo transcurrido la GPU estuvo ocupada).
2. La métrica de "Occupancy", que es el 79% del valor pico, indica un valor alto de tareas de cómputo con baja "occupancy".

En la sección de "Gráficos" se muestra el diagrama de jerarquía de memoria con su flujo de datos y otros parámetros por cada tarea de cómputo. A continuación se muestra las gráficas para las tres tareas de cómputo del algoritmo Nbody.

![vtune_g1](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img14.png)

![vtune_g2](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img15.png)

![vtune_g3](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img16.png)

Tras hacer doble clic en la primera tarea de cómputo nos muestra un cuello de botella en el archivo "GSimulation.cpp".

![vtune_c](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img17.png)

## Pregunta 5: Realizar un análisis Roofline con Advisor

Para realizar este análisis se desconectó del nodo anterior por lo que se tuvo que reconectar a un nodo diferente. Repetimos los pasos de búsqueda de nodo y posterior conexión.

Una vez que nos reconectamos se ejecuta el siguiente comando para generar la carpeta de resultados del análisis Roofline con Advisor:

`advisor --collect=roofline --project-dir=./advi_results -- /home/u185961/Computacion_Paralela/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/nbody`

![advisor_1](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img18.png)

De igual manera que en el caso anterior, se necesita descargar la carpeta "advi_results". Identificamos la ubicación en la que se desea descargar la carpeta y ejecutamos el siguiente comando en otra pestaña de MobaXterm:

`scp -r devcloud:/home/u185961/Computacion_Paralela/oneAPI-samples/advi_results  .`

En la applicación offline de Advisor seleccionamos la opción "Open Project" en la barra vertical de la izquierda de la ventana. Buscamos la carpeta "advi_results" y dentro de ella el archivo con extensión ".advixeproj". Se abre el archivo y se selecciona la opción "Show Result". A continuación se muestran los siguientes resultados en la pestaña "Summary".

![advisor_2](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img19.png)

Para la ejecución en esta nuevo nodo, la pestaña "Survey & Roofline" no muestra datos de Roofline debido a que no hay datos disponibles.

![advisor_3](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img20.png)

Esto se debe a que el tiempo de ejecución ha sido muy pequeño como para que la herramienta pueda analizar algunos datos de Roofline. Esto se muestra en la siguiente figura.

![advisor_4](https://github.com/darteagam/Curso_CPAR_GPUs/blob/main/Images/img21.png)

Los hotspots que se pueden identificar con Advisor se muestran de color rojo en la pestaña "Summary".
