[creacion-script]: /guia-de-usuario/enviar-jobs/comandos-basicos/


# Opciones más comunes de Slurm

A continuación se listan algunas de las opciones más comunes al momento de enviar jobs usando Slurm. Notaremos que existe una opción larga y otra corta para referirnos al mismo parámetro.

## Opciones baśicas del job

| **Opción Larga**    | **Opción Corta** | **Descripción**   | **Ejemplo** |
| :------------------ | :--------------: | :---------------- | :---------- |
| `--job-name`   | `-J`   | Establece un nombre para el trabajo.    | `--job-name=miTrabajo`  |
| `--partition`  | `-p`   | Especifica la partición a la que se enviará el trabajo.   | `--partition=debug` |
| `--time`       | `-t`   | Establece un límite de tiempo para el trabajo. Formato: `días-horas:minutos:segundos`. | `--time=01:30:00` |
| `--output`     | `-o`   | Indica el nombre del archivo donde se guadará la salida `stdout` | `--output=salida-del-job.out` |
| `--error`      | `-e`   | Indica el nombre del archivo donde se guadará la salida `stderr` | `--error=errores-del-job.err` |

Si no se establecen valores para `--output` y/o `--error` se crearán archivos con el siguiente patrón `slurm-%j.out` donde `%j%` es el id del job.

## Opciones para la distribución de tareas


| **Opción Larga**    | **Opción Corta** | **Descripción**   | **Ejemplo** |
| :------------------ | :--------------: | :---------------- | :---------- |
| `--nodes`           | `-N`             | Número de nodos a asignar para el trabajo.  | `--nodes=2` |
| `--ntasks`          | `-n`             | Número de tareas a lanzar.                  | `--ntasks=4`|
| `--ntasks-per-node` |                  | Número de tareas por nodo.                  | `--ntasks-per-node=3`|


## Opciones para la solicitud de CPU

| **Opción**          |  **Descripción**   | **Ejemplo** |
| :------------------ | :----------------: | :---------------- |
| `--cpus-per-task`   |  Indica el número de cores por tarea  | `--cpus-per-task=3`|

## Opciones para la solicitud de memoria RAM

| **Opción**          |  **Descripción**   | **Ejemplo** |
| :------------------ | :----------------: | :---------------- |
| `--mem`             | Establece la memoria requerida por nodo.  | `--mem=4G`     |
| `--mem-per-cpu`     | Establece la mínima memoria requerida por cada núcleo CPU. | `--mem-per-cpu=200M` |
| `--mem-per-gpu`     | Establece la mínima memoria requerida por cada GPU reservado. | `--mem-per-gpu=2G` |

Para la memoria RAM la unidad por defecto son los MB y pueden usarse `[K|M|G|T]` como sufijos para expresar las unidades de memoría. Por ejemplo: 100K son 100 Kilobytes,y 10G son 10 gibabytes.

## Opciones para la solicitud de GPU

| **Opción**          |  **Descripción**   | **Ejemplo** |
| :------------------ | :----------------: | :---------------- |
| `--gres=shard:<numero>` | Establece la cantidad de GPU shards a usar. Permite el uso compartido de GPU (Recomendado). | `--gres=shard:1` |
| `--gres=gpu:<numero>`| Establece la cantidad de GPUs para uso exclusivo. | `--gres=gpu:1` |

En ambas opciones es posible adicionar el tipo de GPU que se desea reservar `--gres=<recurso>:<tipo>:<cantidad>`. Actualmente se dispone de los siguientes tipos de GPU: `tesla`, `a100` y `rtxa6000`. Usando cualquiera de estas opciones, los ejemplos anteriores podrían variar a `--gres=shard:a100:1` o `--gres=gpu:tesla:1`. 


## Opciones para el envío de mails

Es posible habilitar las notificaciones por correo electrónico cada vez que ocurra un determinado evento como el inicio de un job o su falla. Esta opción es bastante útil ya que permite conocer el estado del job sin la necesidad de estar revisando constantemente la cola de ejecución. Utilice la opción `ALL` para recibir notificaciones al iniciar y terminar un job. Opciones disponibles `ALL`, `BEGIN`, `END`, `FAIL`, `NONE`

| **Opción**          |  **Descripción**   | **Ejemplo** |
| :------------------ | :----------------: | :---------------- |
| `--mail-type` | Envía un mail cada vez que ocurra un determinado evento.  | `--mail-type=END,FAIL` |
| `--mail-user`| Establece el mail al cual se enviarán las notificaciones. | `--mail-user=<mi-coreo-electronico>` |