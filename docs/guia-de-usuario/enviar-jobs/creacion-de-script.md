---
title: "Creación de Script"
weight: 32
---

Para trabajos más complejos o largos, es común escribir un script que contiene instrucciones de SLURM y las tareas que deseas ejecutar.

**Ejemplo de un script básico (mi_script.sb):**


```bash
#!/bin/bash
#SBATCH --job-name=mi_trabajo    # Nombre del trabajo
#SBATCH --output=mi_trabajo.out  # Archivo de salida
#SBATCH --error=mi_trabajo.err   # Archivo de error
#SBATCH --ntasks=1               # Número de tareas (procesos)
#SBATCH --cpus-per-task=4        # CPUs por tarea
#SBATCH --mem=8G                 # Memoria por nodo
#SBATCH --time=0-00:10:00        # Tiempo máximo de ejecución (day-hour:min:sec)
#SBATCH --partition=debug        # Partición a usar
#SBATCH --mail-type=END,FAIL     # Cuando se enviará un mail
#SBATCH --mail-user=miusuario@example.com


# Cargar módulos, si es necesario
module load python3/3.10

# Ejecutar el script o comando
python mi_script_python.py

```

**Explicación de las opciones:**

- `--job-name`: El nombre del trabajo.
- `--output`: Archivo donde se guardará la salida estándar.
- `--error`: Archivo donde se guardarán los errores.
- `--ntasks`: Número de tareas (por ejemplo, procesos a ejecutar).
- `--cpus-per-task`: Número de CPUs por tarea.
- `--mem`: Cantidad de memoria por nodo.
- `--time`: Tiempo máximo para ejecutar el trabajo.
- `--partition`: Especifica en qué partición se ejecutará el trabajo. Mayor información sobre las particiones disponibles [aquí]()
- `--mail-type`: Especifica en que estados del job enviar un correo. En el ejemplo se envía al terminar `END` o cuando falle `FAIL`.
- `--mail-user`: Correo electrónico al cual se notificará el cambio de estado del job.

A estas opciones se les conoce como *job request*. La lista de *job request* disponibles las encuentra [aquí]().

**Enviar el trabajo con sbatch:**

```shell
sbatch mi_script.sb
```

Es posible sobreescribir las opciones escritas en el script al momento de enviar el trabajo.

**Enviar el trabajo con sbatch a una particion distinta:**

```shell
sbatch -p gpu-debug mi_script.sb
```

En el ejemplo anterior se cambio la partición inicial de `debug` a `gpu-debug`.