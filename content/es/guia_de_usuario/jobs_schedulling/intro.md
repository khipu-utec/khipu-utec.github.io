---
title: "Introducción"
date: 2022-03-18T10:54:30-05:00
weight: 41
---

{{< toc >}}

Dado que Khipu es un recurso compartido, hacemos uso de un gestor de recursos como [Slurm](https://slurm.schedmd.com/documentation.html) que nos permitirá  enviar, cancelar y revisar cargas de trabajo o *jobs*.


## Slurm
Simple Linux Utility of Resource Mangement (Slurm), es el encargado de coordinar los recursos de todos los nodos del cluster y asignarlos de acuerdo la prioridad de los jobs, cantidad de recursos solicitados y cantidad de recursos disponibles. [Slurm](https://slurm.schedmd.com/documentation.html) tiene un reparto de prioridad justo, donde cada job tiene una prioridad que depende de: **a)** los recursos usados por el usuario o grupo, **b)** la contribución del grupo al clúster y **c)** el tiempo en fila. Por favor, revisar [grupos en Khipu](/cuentas/cuentas_del_cluster/) para más detalles de recursos.

## Ciclo de Vida de un Job


Enviar un job involucra especificar los recursos necesarios y la secuencia de comandos que deseamos ejecutar en los nodos de cómputo. Los pedidos se realizan a grupos de nodos de cómputo llamadas particiones. Las particiones, posen sus límites y propósitos de uso. En Khipu tenemos particiones especificas para cada [grupo de usuarios](/cuentas/cuentas_del_cluster/). Una vez enviados, los jobs esperan en una cola y están sujetos a factores extra como sus prioridades de schedulling. Cuando el proceso de schedulling de un job inicia, los comandos o aplicaciones especificados son ejecutados en los nodos de computo que el scheduller asigna para satisfacer las necesidades de recurso solicitadas. El resultado de la ejecución será impreso en pantalla o escrito en un archivo dependiendo de la forma como se envío el job.

## Tipos de Jobs

En Slurm podemos diferenciar los siguientes tipos de *jobs*: batch jobs y jobs interactivos.

### Batch Jobs

Son aquellos jobs que se envían a través de un bash script y que son ejecutada de manera no-interactiva (no existe interacción con los I/O). Los scripts de envió se conforman de tres partes:

1. Un primera linea [hashbang](https://en.wikipedia.org/wiki/Shebang_(Unix)) la cual especifica el programa que va a ejecutar el script. Por lo general es `#!/bin/bash`.
2. Una lista de directivas con los requisitos del job. Estas lineas tienen que estar antes de cualquier comando o definición de variables de ambiente, caso contrario serán ignoradas.
3. Los comandos o aplicaciones que se van ejecutar en el job.

A continuación se muestra un ejemplo básico de un script.

```bash
#!/bin/bash

## Directivas

# Job name:
#SBATCH --job-name=simple_example 

# Nombre del archivo de salida:
#SBATCH --out="slurm-%j.out"

# Tiempo limite de ejecución:
#SBATCH --time=01:00

# Cantidad de recursos
#SBATCH --nodes=1 --ntasks=1 --cpus-per-task=2 

## Secuencia de comandos

echo "Iniciando en $(date)"
echo "El job fue enviado a la partición ${SLURM_JOB_PARTITION}, la partición por defecto es ${SLURM_CLUSTER_NAME}"
echo "Nombre del job: ${SLURM_JOB_NAME}, Job ID: ${SLURM_JOB_ID}"
echo "Tengo ${SLURM_CPUS_ON_NODE} CPUs en el nodo $(hostname)"
echo "Voy a dormir 10s para que me veas en la fila"
sleep 10 
```
Para probarlo, deberemos guardarlo con el siguiente formato `ej-simple.sh` y lo mandaremos ejecutando.

```shell
sbatch ej-simple.sh
```

Ejemplos más detallados de bash scripts se muestran [aquí](guia_de_usuario/ejecutando_trabajos/enviar_jobs/).

### Jobs Interactivos

Los jobs interactivos son aquellos que pueden ser usados para testear y encontrar errores en un código. Al pedir un job interactivo, se reservaran los recursos y se iniciará sesión en un shell de alguno de los nodos de cómputo. A continuación se muestra un ejemplo de job interactivo:

```shell
srun --pty -t 2:00:00 --mem=8G -p docencia bash

El comando anterior asignará un CPU y 8GiB RAM por un periodo de 2 horas. Durante ese tiempo podremos ejecutar comandos dentro de la shell de manera interactiva. Para salir de la sesión, deberemos ejecutar `exit` o presionar `Ctrl`+`d`.

```