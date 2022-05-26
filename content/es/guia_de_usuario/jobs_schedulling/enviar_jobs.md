---
title: "Enviar Jobs"
date: 2022-03-18T10:54:30-05:00
weight: 42
---

{{< toc >}}

# Comandos básicos de Slurm

A continuación se muestra una lista con los comandos de Slurm más usados:

- Para enviar un batch job a la cola de slurm ejecutaremos `sbatch`.

```shell
sbatch ejemplo.sh
```

- Para listar los jobs que he enviado a la cola usaremos `squeue`.

```shell
squeue --me
```

- Para cancelar un job en ejecución usando su job ID emplearemos `scancel`.

```shell
scancel 78910
```

- Para revisar el estatus de un job usaremos `sacct` y le pasaremos un job ID.

```shell
sacct  -j 78910
```

- Para revisar cuan eficientemente un job se ejecuta, emplearemos `seff` acompañado del job ID.
  
```bash
seff 78910
```

- Para ejecutar un job de manera interactiva emplearemos `srun`

```shell
srun --pty -t 2:00:00 --mem=8G -p interactive bash
```

## Job request más comunes

Las siguientes opciones modifican el tamaño, largo y el comportamiento del job que se envía. Estos pueden especificarse llamando a `srun` o `sbatch`, o dentro de un [batch job](/guia_de_usuario/ejecutando_trabajos/slurm/#batch-jobs). Si se especifican las opciones en los argumentos de `sbatch` y en el script del [batch job](/guia_de_usuario/ejecutando_trabajos/slurm/#batch-jobs) al mismo tiempo, las opciones pasadas al comando `sbatch` serán las que se tomarán en cuenta. Si no se especifica valor para alguna de las opciones, los valores por defecto serán los que se empleen. 

<style>
.myTable {
    border-radius: 5px;
}
.myTable th {
    background-color:var(--body-font-color); 
    color: white; 
}

</style>

<div class="myTable">

| Opción Larga | Opción Corta | Valor por Defecto | Descripción |
| --- | --- | --- | --- |
| `--job-name` | `-J` | Nombre del archivo  | Nombre de job personalizado. |
| `--output` | `-o` | `"slurm-%j.out"` | Nombre del archivo donde se guadará la salida `stdout` o `stderr`. Mayores patrones de nombre [aquí](https://slurm.schedmd.com/sbatch.html#SECTION_%3CB%3Efilename-pattern%3C/B%3E). |
| `--error` | `-e` | Se escribe en el mismo archivo del `--output` | Nombre del archivo donde se guadarán los logs de ;ps errores. |
| `--partition` | `-p` | Varía de acuerdo al cluster  | Señala la partición donde se va a ejecutar el job. |
| `--account` | `-A` | El nombre de su grupo  | Especifica si se tiene acceso a múltiples particiones privadas. |
| `--time` | `-t` | Varía de acuerda a la partición  | Límite de tiempo para el job en el formato `D-HH:MM:SS`. Por ejemplo,  `-t 1-` es un día de ejecución y `-t 4:00:00` son 4 horas. |
| `--nodes` | `-N` | 1 | Número total de nodos. |
| `--ntasks` | `-n` | 1 | Número de tareas (workers MPI). |
| `--ntasks-per-node` | | El scheduler lo decide | Número de tareas por nodo. |
| `--cpus-per-task` | `-c` | 1 | Número de CPUs para cada tarea. Use esto para threads/cores en un job de nodo único. |
| `--mem-per-cpu` | | `5G` | Cantidad de memoria RAM requerida por CPU en MiB. Si se especifica en GiB usar `G`(ej. `10GB`). |
| `--mem` | | | Memoria pedida por nodo en MiB. Si se especifica en GiB usar `G`(ej. `10GB`). |
| `--gpus` | `G` | | Usado para pedir GPUs. |
| `--constraint` | `C` | | Restricciones a las características del nodo. Para limitar los tipos de nodos a ejecutarse. |
| `--mail-user` | | Tu email de UTEC | Dirección de correo a donde enviar notificaciones del job. |
| `--mail-type` | | Ninguna | Envía un mail cada vez que un job cambia de estado. Utilice la opción `ALL` para recibir notificaciones al iniciar y terminar un job. Opciones disponibles `ALL`, `BEGIN`, `END`, `FAIL`, `NONE` |
</div>