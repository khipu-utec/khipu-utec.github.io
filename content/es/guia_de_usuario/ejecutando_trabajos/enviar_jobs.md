---
title: "Enviar Jobs"
date: 2022-03-18T10:56:32-05:00
weight: 43
---


{{< toc >}}




1. Para mandar a ejecutar un job usando el script de ejemplo ej1.sh.

{{< expand "Mostrar ej1.sh">}}

```bash
#!/bin/bash
#SBATCH --job-name=ej1 # nombre del job
#SBATCH -c 1 # numero de cpu cores a usar
#SBATCH --mem-per-cpu=100mb # tamano de memoria del job en ejecucion.
date
sleep 10 # duerme 10s, solo para visualizar el job en la fila
```

{{< /expand >}}

```shell
[juana.perez@khipu ~]$ sbatch ej1.sh
Submitted batch job 91
```

2. Para mostrar la fila actual de jobs:

```shell
[juana.perez@khipu ~]$ squeue
JOBID      PARTITION    NAME            USER    ST  TIME NODES NODELIST(REASON)
       91    investigacion        EJ2     juana.perez      R    0:29           1  n001
```

3. Para mostrar todos los detalles del job 91:

```shell
[juana.perez@khipu ~]$ scontrol show job 91
```

4. Para cancelar la ejecución del job 91:

```shell
[juana.perez@khipu ~]$ scancel 91
```

### Directivas de jobs

{{< hint info >}}
**Importante:** verificar los recursos de [Khipu](/). Slurm regulará automáticamente en caso el usuario exceda las capacidades del Grupo de Usuario. 
{{< /hint >}}


| Directiva | Versión corta | Descripción |
| :---     | :---         | :---       |
| `--mail-user=<email_address>` | | Correo electrónico |
| `after:job_id[[+time][:jobid[+time]...]]`  | | El job se mandará a la fila luego que otro job termine su ejecución.  |
| `--mail-type=<ALL,BEGIN,END,FAIL,NONE>` | | Enviar notificaciones de job a email |
| `--gres=gpu:<cantidadGPU>`  | | Especifica la cantidad de tarjetas GPU que se necesitan.  |
| `--export=var1=valor[,var2=valor...]`  | | Especifica los valores de las variables de entorno en la ejecución del script. |
| `--nodes=<nodes> `  | `-N <nodes>`  | Nodos requeridos por job.  |
| `--job-name=<Nombrejob > `  | `-J <Nombrejob> ` |Nombre del job.  |
| `--cpus-per-task=<cpus> `  | 	`-c <cpus>`  | Núcleos CPU por tarea.  |
| `--nodes=<numeroNodos> `  | `-N <numeroNodos>`  | 	Número de nodos solicitados para la ejecución de los jobs.  |
| `--output= <rutaArchivo.log > `  | `-o <rutaArchivo.log>`  | Ruta de archivo para almacenar la salida estandar del job.  |
| `--error = <rutaArchivo.log > `  | `-e <rutaArchivo.log>`  | Ruta de archivo para almacenar errores en jobs.  |
| `--time=<tiempo> `  | `-t <tiempo>`  | Tiempo requerido.   |

### Ejemplos de job scripts

{{< tabs "uniqueid" >}}
{{< tab "Ejemplo 1" >}} 

Imprime la fecha actual. 

1. Crear el archivo ej1.sh:

```bash
#!/bin/bash
#SBATCH --job-name=ej1 # nombre del job
#SBATCH -c 1 # numero de cpu cores a usar
#SBATCH --mem-per-cpu=100mb # tamano de memoria del job en ejecucion.
date
sleep 10 # duerme 10s, solo para visualizar el job en la fila
```

2. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej1.sh
```


 {{< /tab >}}
{{< tab "Ejemplo 2" >}}

Imprime "Hello World”. 

1. Crear el archivo ej2.sh:

```bash
#!/bin/bash
#SBATCH --job-name=ej2  # nombre del job.
#SBATCH -c 1 # numero de cpu cores a usar.
#SBATCH --time=1:00 # tiempo maximo de ejecucion del job.
#SBATCH --mem-per-cpu=100mb # tamano de memoria del job en ejecucion.
echo "Hello World” # imprime Hello World
sleep 10 # duerme 10s

# End of script
```

2. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej2.sh
```

{{< /tab >}}
{{< tab "Ejemplo 3" >}} 

Muestra las variables de ambiente propias de Slurm. 

1. Crear el archivo ej3.sh:

```bash
#!/bin/bash
#SBATCH -J ej3 # nombre del job
set | grep SLURM
```

2. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej3.sh
```

{{< /tab >}}

{{< tab "Ejemplo 3" >}} 

Muestra las variables de ambiente propias de Slurm. 

1. Crear el archivo ej3.sh:

```bash
#!/bin/bash
#SBATCH -J ej3 # nombre del job
set | grep SLURM
```

2. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej3.sh
```

{{< /tab >}}
{{< /tabs >}}


### Ejemplos de job scripts con MPI, OpenMP y Python

{{< tabs "mpi-openmp" >}}
{{< tab "Ejemplo 4" >}} 

Ejecución del programa en Openmpi, prueba_mpi.c: 

```c++
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
// Initialize the MPI environment
MPI_Init(NULL, NULL);

// Get the number of processes
int world_size;
MPI_Comm_size(MPI_COMM_WORLD, &world_size);


// Get the rank of the process
int world_rank;
MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

// Get the name of the processor
char processor_name[MPI_MAX_PROCESSOR_NAME];
int name_len;
MPI_Get_processor_name(processor_name, &name_len);

// Print off a hello world message
printf("Hello world from processor %s, rank %d out of %d processors\n",
processor_name, world_rank, world_size);

// Finalize the MPI environment.
MPI_Finalize();
}
```

1. Compilar el programa mpi:

```shell
[juana.perez@khipu ~]$ module load openmpi/2.1.6
[juana.perez@khipu ~]$ mpicc prueba_mpi.c -o prueba_mpi
[juana.perez@khipu ~]$ module unload openmpi/2.1.6
```

2. Crear el archivo ej4.sh:

```bash
#!/bin/bash
#SBATCH -J ej4 # nombre del job
#SBATCH -p investigacion # nombre de la particion 
#SBATCH -N 2 # numero de nodos
#SBATCH --tasks-per-node=3 # numero de tasks por nodo

module load openmpi/2.1.6 # carga el modulo de openmpi version 2.1.6
srun prueba_mpi # siendo prueba_mpi el nombre del programa mpi
module unload openmpi/2.1.6 
```

2. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej4.sh
```

 {{< /tab >}}
{{< tab "Ejemplo 5" >}}

Ejemplo de ejecución del programa OpenMP, prueba_openmp.c:

```c++
#include <stdio.h>
#include <stdlib.h>
#include <omp.h>

void Hello(void); /* Thread function */

/*--------------------------------------------------------------------*/
int main(int argc, char* argv[]) {
int thread_count = strtol(argv[1], NULL, 10);
# pragma omp parallel num_threads(thread_count)
Hello();
return 0;
}

/* main */
/*------------------------------------------------------------------- * Function: Hello
* * Purpose: Thread function that prints message
* */
void Hello(void) {
int my_rank = omp_get_thread_num();
int thread_count = omp_get_num_threads();
printf("Hello from thread %d of %d\n", my_rank, thread_count); /* Hello */
}
```

1. Compilar el programa `prueba_openmp.c`:

```shell
[juana.perez@khipu ~]$ module load gcc/5.5.0
[juana.perez@khipu ~]$ g++ prueba_openmp.c -fopenmp -lpthread -o prueba_openmp
[juana.perez@khipu ~]$ module unload gcc/5.5.0 
```

2. Crear el archivo ej2.sh:

```bash
#!/bin/bash
#SBATCH -J ej6
#SBATCH -N 1
#SBATCH --tasks-per-node=8
#SBATCH --mem=1GB

module load gcc/5.5.0
unset OMP_NUM_THREADS

./ejemplo_openmp ${SLURM_NPROCS}
module unload gcc/5.5.0
```

3. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej6.sh
```

{{< /tab >}}
{{< tab "Ejemplo 3" >}} 

Muestra las variables de ambiente propias de Slurm. 

1. Crear el archivo ej3.sh:

```bash
#!/bin/bash
#SBATCH -J ej3 # nombre del job
set | grep SLURM
```

2. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej3.sh
```

{{< /tab >}}

{{< tab "Ejemplo 6" >}} 

Ejecución del programa en Python, prueba_python.py:

```python
from math import factorial as f

print("Hola mundo")
N = 100
print("%d! = %d" %(N, f(N)))
```

1. Crear el archivo ej5.sh:

```bash
#!/bin/bash
#SBATCH -J ej5 # nombre del job
#SBATCH -p investigacion # nombre de la particion 
#SBATCH -c 1  # numero de cpu cores a usar

module load python/2.7.17 # carga el modulo de python version 2.7.17
python2.7 prueba_python.py # siendo prueba_python.py el nombre del programa python
module unload python/2.7.17 
```

2. Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej5.sh
```

{{< /tab >}}
{{< /tabs >}}

### Ejemplos de job scripts avanzados

{{< tabs "avanzados" >}}
{{< tab "Ejemplo 7" >}} 

Ejecución de dos jobs, donde uno depende del término del otro. 

1. Crear los archivos `ej7-1.sh` y `ej7-2.sh`: 

```bash
#!/bin/bash
#SBATCH -J ej7-1
#SBATCH -N 1
#SBATCH --tasks-per-node=1
#SBATCH --mem=1GB

echo "ej7-1"
date
sleep 120
date
```


```bash
#!/bin/bash
#SBATCH -J ej7-2
#SBATCH -N 1
#SBATCH --tasks-per-node=1
#SBATCH --mem=1GB

echo "ej7-2"
date
sleep 120
date
```

2. Enviar a ejecutar los jobs:

```shell
[juana.perez@khipu ~]$ n_proc=$(sbatch ej7-1.sh)
[juana.perez@khipu ~]$ sbatch -d after:${n_proc##* } ej7-2.sh
[juana.perez@khipu ~]$ squeue 
```

 {{< /tab >}}
{{< tab "Ejemplo 8" >}}

Muestra como pasar variables a los jobs, esto permite setear variables de ambiente para la ejecución dentro de nuestro script. No es recomendable usar seteos de variables direcamente como comandos Unix. Existen dos formas: 

1. Colocar el set de variables que se necesita en el script mediante el flag “#SBATCH --export=var1=valor,var2=valor…”. 

- Crear el archivo ej8-1.sh: 

```bash
#!/bin/bash
#SBATCH -J ej8-1
#SBATCH -N 1
#SBATCH --tasks-per-node=1
#SBATCH --mem=1GB
#SBATCH --export=var1=12

echo $var1 
```

- Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch ej8-1.sh
```


2. Especificar el valor de las variables al momento del envío del script. Brinda flexibilidad y es recomendable usarlo cuando solo varían pocos parámetros, como el nombre de un archivo o valor único. 

- Crear el archivo `ej8-2.sh`: 

```bash
#!/bin/bash
#SBATCH -J ej8-2
#SBATCH -N 1
#SBATCH --tasks-per-node=1
#SBATCH --mem=1GB

echo $var1 
```

- Enviar a ejecutar el job:

```shell
[juana.perez@khipu ~]$ sbatch --export=var1=15 ej8-2.sh
```

{{< /tab >}}

{{< /tabs >}}

### Información adicional

- [Tutorial de Linux](https://www.tecmint.com/free-online-linux-learning-guide-for-beginners/)
- [Documentación de Slurm](https://slurm.schedmd.com/quickstart.html)