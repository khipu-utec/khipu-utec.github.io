---
title: "Ejecutar y Revisar Jobs"
date: 2022-03-18T10:54:30-05:00
weight: 41
---

## Sobre Slurm

[Slurm](https://slurm.schedmd.com/documentation.html) es un gestor de filas de trabajos (jobs) que permite organizar de forma eficiente el uso de recursos compartido del cluster. [Slurm](https://slurm.schedmd.com/documentation.html) asigna recursos a los jobs de acuerdo con la prioridad del trabajo y la cantidad de recursos solicitados versus disponibles. [Slurm](https://slurm.schedmd.com/documentation.html) tiene un reparto de prioridad justo, donde cada job tiene una prioridad que depende de: a) los recursos usados por el usuario o grupo, b) la contribución del grupo al clúster y c) el tiempo en fila. Por favor, revisar [grupos en Khipu](/) para más detalles de recursos.
Los comandos más usados son: 

- **sbatch:** permite adicionar jobs a la fila mediante un script. [Link del manual de sbatch](https://slurm.schedmd.com/quickstart.html).
- **srun:** permite adicionar jobs en modo interactivo lo que permite probar comandos que se usarán en el script así como depurarlo. [Link del manual de srun](https://slurm.schedmd.com/srun.html).
- **scancel:** termina la ejecución de un determinado job, independientemente si está o no en ejecución. [Link del manual de scancel](https://slurm.schedmd.com/scancel.html).
- **squeue:** permite visualizar el estado actual de la fila de jobs. [Link del manual de squeue](https://slurm.schedmd.com/squeue.html).