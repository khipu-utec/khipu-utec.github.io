[creacion-script]: /guia-de-usuario/enviar-jobs/creacion-de-script


# Parámetros de Slurm

Las siguientes opciones modifican el tamaño, largo y el comportamiento del job que se envía. Estos pueden especificarse llamando a `srun` o `sbatch`, o dentro de un [batch job][creacion-script]. Si se especifican al mismo tiempo las opciones en los argumentos de `sbatch` y en el script del [batch job][creacion-script], las opciones pasadas al comando `sbatch` serán las que se tomarán en cuenta. Si no se especifica valor para alguna de las opciones, los valores por defecto serán los que se empleen. 


| Opción Larga | Opción Corta | Valor por Defecto | Descripción |
| --- | --- | --- | --- |
| `--job-name` | `-J` | Nombre del archivo  | Nombre de job personalizado. |
| `--output` | `-o` | `"slurm-%j.out"` | Nombre del archivo donde se guadará la salida `stdout` o `stderr`. Mayores patrones de nombre [aquí](https://slurm.schedmd.com/sbatch.html#SECTION_%3CB%3Efilename-pattern%3C/B%3E). |
| `--error` | `-e` | Se escribe en el mismo archivo del `--output` | Nombre del archivo donde se guadarán los logs de ;ps errores. |
| `--partition` | `-p` | `debug`  | Señala la partición donde se va a ejecutar el job. |
| `--time` | `-t` | Varía de acuerda a la partición y tipo de usuario | Límite de tiempo para el job en el formato `D-HH:MM:SS`. Por ejemplo,  `-t 1-` es un día de ejecución y `-t 4:00:00` son 4 horas. |
| `--nodes` | `-N` | 1 | Número total de nodos. |
| `--ntasks` | `-n` | 1 | Número de tareas (workers MPI). |
| `--ntasks-per-node` | | El scheduler lo decide | Número de tareas por nodo. |
| `--cpus-per-task` | `-c` | 1 | Número de cores de CPUs para cada tarea. Use esto para threads/cores en un job de nodo único. |
| `--mem-per-cpu` | | `5G` | Cantidad de memoria RAM requerida por CPU en MiB. Si se especifica en GiB usar `G`(ej. `10G`). |
| `--mem` | | | Memoria pedida por nodo en MiB. Si se especifica en GiB usar `G`(ej. `10G`). |
| `--mail-user` | | Tu email de UTEC | Dirección de correo a donde enviar notificaciones del job. |
| `--mail-type` | | Ninguna | Envía un mail cada vez que un job cambia de estado. Utilice la opción `ALL` para recibir notificaciones al iniciar y terminar un job. Opciones disponibles `ALL`, `BEGIN`, `END`, `FAIL`, `NONE` |
