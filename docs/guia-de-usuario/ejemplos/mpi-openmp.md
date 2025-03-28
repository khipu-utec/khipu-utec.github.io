---
title: "MPI y OpenMP"
weight: 36
---

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

## OpenMP de un solo thread 

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

## OpenMP de múltiples threads

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

## MPI multi-proceso

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

# Carga del modulo MPICH 4.0
module load mpich/4.0
# Ejecución del compilado
mpirun  prueba_mpi
# Descarga del módulo
module unload mpich/4.0
```

3. Enviar a ejecutar el job:

```shell
sbatch multi_process_mpi.sh
```


## MPI Y OpenMP a la vez (Híbrido)

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