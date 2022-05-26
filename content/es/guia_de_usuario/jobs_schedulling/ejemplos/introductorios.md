---
title: "Introductorios"
date: 2022-05-25T19:46:06-05:00
weight: 43
---

{{< toc >}}

## Imprimir la fecha actual

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

## Imprimir las variables de ambiente de Slurm

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
