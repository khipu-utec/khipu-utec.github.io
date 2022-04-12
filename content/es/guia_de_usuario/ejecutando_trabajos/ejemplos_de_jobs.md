---
title: "Ejemplos de Jobs Scripts"
date: 2022-03-18T10:56:32-05:00
weight: 43
---

{{< toc >}}

## Ejemplos Introductorios

### Imprimir la fecha actual

1. Crear el archivo fecha_actual.sh:

```bash
#!/bin/bash

# Nombre del job:
#SBATCH --job-name=fecha_actual 

# Cantidad de CPUs cores a usar:
#SBATCH -c 1

# Tamaño de memoria del job:
#SBATCH --mem-per-cpu=100mb

date
sleep 10 # duerme 10s, solo para visualizar el job en la fila
```

2. Enviar a ejecutar el job:

```shell
sbatch fecha_actual.sh
```

### Imprimir las variables de ambiente de Slurm

1. Crear el archivo env_vars.sh:

```bash
#!/bin/bash

# Nombre del job:
#SBATCH --job-name=env_vars 

# Comandos:
set | grep SLURM
```

2. Enviar a ejecutar el job:

```shell
sbatch env_vars.sh
```

## Ejemplos con OpenMP, MPI e Híbridos

Para los siguientes dos ejemplos con OpenMP usaremos el siguiente código  en C++ `prueba_openmp.cpp`.

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
/*------------------------------------------------------------------- 
* * Function: Hello
* * Purpose: Thread function that prints message
* */
void Hello(void) {
    int my_rank = omp_get_thread_num();
    int thread_count = omp_get_num_threads();
    printf("Hello from thread %d of %d\n", my_rank, thread_count); /* Hello */
}
```

Compilaremos el programa antes de crear y enviar el batch script.

```shell
module load gcc/5.5.0
g++ prueba_openmp.cpp -fopenmp -lpthread -o prueba_openmp
module unload gcc/5.5.0 
```

### OpenMP de un solo thread 

1. Crear el archivo `single_thread_openmp.sh`:

```bash
#!/bin/bash

# Nombre del job:
#SBATCH --job-name=single_thread_openmp

# Límite de tiempo de 10 min:
#SBATCH --time=10:00

./prueba_openmp
```

2. Enviar a ejecutar el job:

```shell
sbatch single_thread_openmp.sh
```

### OpenMP de múltiples threads

1. Crear el archivo `multi_thread_openmp.sh`:

```bash
#!/bin/bash

# Nombre del job:
#SBATCH --job-name=multi_thread_omp_job
# Nombre del archivo de salida:
#SBATCH --output=multi_thread_omp_job.txt
# Numero de tasks:
#SBATCH --ntasks=1
# Numero de CPUs por task:
#SBATCH --cpus-per-task=4
# Límite de tiempo de 10 min:
#SBATCH --time=10:00

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
./prueba_openmp
```

2. Enviar a ejecutar el job:

```shell
sbatch multi_thread_openmp.sh
```

### MPI multi-proceso

Para el siguiente ejemplo usaremos el código `prueba_mpi.c`:

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
module load mpich/4.0
mpicc prueba_mpi.c -o prueba_mpi
module unload mpich/4.0
```

2. Crear el archivo `multi_process_mpi.sh`:

```bash
#!/bin/bash

# Nombre del job:
#SBATCH -J prueba_mpi
# Nombre de la partición:
#SBATCH -p investigacion
# Número de nodos:
#SBATCH -N 2 
# Número de tasks por nodo:
#SBATCH --tasks-per-node=3 

## Carga del modulo MPICH 4.0
module load mpich/4.0
# Ejecución del compilado
mpirun  prueba_mpi
## Descarga del módulo
module unload mpich/4.0
```

3. Enviar a ejecutar el job:

```shell
sbatch multi_process_mpi.sh
```


### MPI Y OpenMP a la vez (Híbrido)

Para este ejemplo usaremos el siguiente codigo hibrido y lo guardaremos en un archivo `hibrido.c`.

```c++
#include <stdio.h>
#include <omp.h>
#include "mpi.h"

int main(int argc, char *argv[]) {
    int numprocs, rank, namelen;
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    int iam = 0, np = 1;

    MPI_Init(&argc, &argv);
    MPI_Comm_size(MPI_COMM_WORLD, &numprocs);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Get_processor_name(processor_name, &namelen);

    #pragma omp parallel default(shared) private(iam, np)
    {
    np = omp_get_num_threads();
    iam = omp_get_thread_num();
    printf("Hello from thread %d out of %d from process %d out of %d on %s\n",
            iam, np, rank, numprocs, processor_name);
    }

    MPI_Finalize();
}
```

1. Compilar el programa híbrido:

```shell
module load mpich/4.0
mpicc -fopenmp hibrido.c -o hibrido
module unload mpich/4.0
```

2. Crear el archivo `hibrido_mpi_openmp.sh`:

```bash
#!/bin/bash

# Un Job script para la ejecución de un código híbrido MPI-OpenMP

#SBATCH --job-name=hibrido_mpi_openmp
#SBATCH --output=hibrido_mpi_openmp.out
#SBATCH --ntasks=4
#SBATCH --cpus-per-task=8
#SBATCH --partition=docencia

# Cargar el modulo MPI.
module load mpich/4.0

# Configurar el valor de  OMP_NUM_THREADS con el numero de CPUs por task solicitado.
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

# Ejecutar el proceso con mpirun. Puede notar que no es necesario especificar el flag de MPI
# '-n' puesto que automaticamente utiliza el valor de la configuración de Slurm realizada

mpirun ./hibrido

module unload mpich/4.0
```

3. Enviar a ejecutar el job:

```shell
sbatch hibrido_mpi_openmp.sh
```

## Ejemplos Diversos

### Ejecución de un programa en Python

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
sbatch ej5.sh
```


## Ejemplos de Job Scripts Avanzados

### Jobs dependientes

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
n_proc=$(sbatch ej7-1.sh)
sbatch -d after:${n_proc##* } ej7-2.sh
squeue 
```

### Pasar variables de ambiente a jobs

Muestra como pasar variables a los jobs, esto permite setear variables de ambiente para la ejecución dentro de nuestro script. No es recomendable usar seteos de variables direcamente como comandos Unix. Existen dos formas: 

1. Colocar el set de variables que se necesita en el script mediante el flag `--export=var1=valor,var2=valor…`. 

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
sbatch ej8-1.sh
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
sbatch --export=var1=15 ej8-2.sh
```

<!-- ## Jobs en GPU

```bash
#!/bin/bash

#SBATCH --job-name=deep_learn
#SBATCH --output=gpu_job.txt
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=2
#SBATCH --gpus=p100:2
#SBATCH --partition=gpu
#SBATCH --time=10:00

module load CUDA
module load cuDNN
# using your anaconda environment
source activate deep-learn
python my_tensorflow.py
 -->

```
## Información adicional

- [Tutorial de Linux](https://www.tecmint.com/free-online-linux-learning-guide-for-beginners/)
- [Documentación de Slurm](https://slurm.schedmd.com/quickstart.html)