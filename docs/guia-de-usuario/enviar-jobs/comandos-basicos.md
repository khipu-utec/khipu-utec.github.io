---
title: "Comandos básicos de SLURM"
weight: 33
---
[particiones]:/guia-de-usuario/enviar-jobs/particiones/


En esta guía se van a desarrollar algunos comandos básicos de Slurm. Si es su primera vez usando Slurm, le será de mucha utilidad acompañar esta lectura realizando pruebas de los comandos mencionados en el cluster. Al inicio puede resultar un poco tedioso escribir nuestras cargas de trabajo en scripts, sin embargo poco a poco le resultará más sencillo y a la larga usted manejará una herramiento valiosa y ampliamente usada en el mundo del HPC.

Los principales comandos de Slurm se muestran en la siguiente tabla:

| Comando      | Descripción                          |
| :---------: | :---------------------------------- |
| `sbatch`       | :material-check:  Envía un batch script  |
| `srun`       | :material-check: Ejecuta un job paralelo (step)|
| `squeue`    | :material-check: Muestra información de la cola de trabajos |
| `scancel`    | :material-check: Envía un *signal* o cancela un job, array de jobs o job steps.   |
| `sinfo`    | :material-check: Muestra información de las [particiones][particiones]. |


## Creación de un batch script

Un batch script es un archivo que indica los recursos necesarios y los pasos a seguir para la ejecución de un determinado job. A continuación se muestra un ejemplo:

```bash linenums="1"
#!/bin/bash

#SBATCH --job-name=nombre_del_job
#SBATCH --partition=debug
#SBATCH --ntasks=1 --cpus-per-task=4
#SBATCH --mem=4G
#SBATCH --time=00:10:00
#SBATCH --output=nombre_archivo.out
#SBATCH --error=nombre_archivo.err
#SBATCH --mail-type=END,FAIL
#SBATCH --mail-user=user@example.com

## Especificar el directorio de trabajo
cd <mi-directorio-de-trabajo>

## Cargar los módulos necesarios
ml load <module>/<version>

## Ejecute su programa
srun <mi-programa>
```

En este ejemplo `#!/bin/bash` indica que debe ser interpretado como un script de bash. Las siguientes líneas que contienen `#SBATCH` son directivas que especifican determinadas opciones disponibles en Slurm usando la siguiente sintaxis:

```bash
#SBATCH <opción>=<valor>
```

Por ejemplo, la línea 3 `#SBATCH --job-name=nombre_del_job` indica cual es el nombre del job. 

La línea 4, especifica que el job se ejecutará en la [partición][particiones] `debug`. 

Las línea 5 y 6, especifican los recursos computacionales que serán usados para la ejecución del job. En este ejemplo, `--ntasks` indica que se ejecutará un proceso, `--cpus-per-task` que se usarán 4 núcleos CPU por cada proceso y `--mem` que se utilizará 4GB de RAM en total. 

La línea 7 indica que el límite máximo de duración del job será de 10 minutos. Si el job pasa de esa duración, será cancelado. 

Las líneas 8 y 9 indican donde se escribirán la salida estandar `--output` y la salida en caso de error `--error`. Si no se indican, tomaran un valor por defecto adicionado al identificador del job.

La línea 10 indica que se envíe un mail cuando el job llegue a los estados de `END` (culminación normal) o `FAIL` (culminación por error).

La línea 14 especifica el directorio de trabajo donde se encuentra el programa que será ejecutado. Si no lo especifica, usará el directorio desde donde esté enviando el job. Es altamente recomendado especificar su directorio de trabajo.

La línea 17 indica cuales [módulos][modulos] deberán ser cargados para la ejecución del job.

Finalmente, la línea 20 indica cual será el comando que será ejecutado. Es importante anteponer `srun` antes del comando que ejecutará. Por ejemplo: `srun python3 myapp.py` o `srun ./myapp`. 


## Envío de un batch job

Para enviar el job script que se creó en el paso anterior deberemos usar el comando `sbatch`, el cual posee la siguiente sintaxis:

```bash
 sbatch [opciones] nombre-de-mi-job.slurm [argumentos del job ...]
```

Donde las `[opciones]` tienen las mismas estructura que aquellas que escribimos en el batch script, pero sin la palabra `#SBATCH`. Aquellas opciones que especifiquemos al momento de ejecutar `sbatch` tiene precedencia por sobre aquellas que se encuentren dentro del script. Por ejemplo, si yo ejecuto `sbatch --partition=standard mi-scrip.slurm`, donde `mi-script.slurm` es el batch script anterior, el job se ejecutará en la partición `standard` en lugar de `debug` que fue especificada dentro del job. No siempre será necesario sobreescribir las opciones que se encuentran dentro del script. Para la mayoría de casos bastará con ejecutar:

```bash
sbatch nombre-de-mi-job.slurm
```

Si bien no hay una restricción en la extensión que debe de tener nuestro batch script, es una buena paractica usar la extension `*.slurm` para diferenciarlo de los bash script `*.sh` tradicionales. 

## Examinar la cola de ejecución

Cuando usted envía su job, no necesariamente se ejecutará de manera inmediata. Este puede estar en espera por un tiempo hasta que se liberen los recursos necesarios para que pueda entrar a ejecución. Para ver los jobs que se encuentran en espera y en ejecución usaremos el comando `squeue`.

```bash
squeue
```

El cual nos mostrará un resultado parecido al siguiente:

```txt
   JOBID PARTITION    NAME     USER   ST   TIME  NODES NODELIST(REASON)
   5757       gpu pretrain some-user  PD   0:00      5 (PartitionNodeLimit)
   5758  standard     bash some-user  PD   0:00      1 (QOSMaxWallDurationPerJobLimit)
   5741       gpu fn_somet some-user  R    1:17:21   1 g001
```

Aquí podemos notar que hay tres jobs, de los cuales uno se encuentra en ejecución (por eso tienen la `R` de *running*) y otro dos se encuentra pendientes `PD`. Es importante visualizar que los jobs en pendiente tienen en el apartado de **NODELIST(REASON)**, una explicación rápida de por qué se encuentra en ese estado. En este ejemplo, `(PartitionNodeLimit)` me indica que estoy tratando de reservar más nodos de los permitidos en esa partición y `(QOSMaxWallDurationPerJobLimit)` que estoy tratando de reservar más tiempo del permitido por cada job.


En algunos casos desearemos solamente visualizar el estado de nuestros jobs. Para ello ejecutaremos:

```bash
squeue --me
```

## Cancelar trabajos

En algunos casos necesitaremos cancelar nuestro job porque tal vez faltó añadir algo, o no está ejecutando como esperamos. En sos casos bastará con ejecutar el comando `scancel` acompañado del id del job:

```bash
scancel <job-id>
```
El identificador del job puede obtenerse mirando la cola de ejecución con `squeue`.

##  Ver información sobre los nodos y particiones

Para obtener información sobre las particiones, su estado y nodos disponibles ejecutaremos  el comando `sinfo`. El cual nos mostrará en patalla una salida similar a la siguiente:

```txt
PARTITION    AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug*          up   infinite      1   idle n003
debug-gpu       up   infinite      1    mix g001
standard        up   infinite      4   idle n[003-006]
big-mem         up   infinite      3   idle ag001,g002,n006
gpu             up   infinite      1    mix g001
gpu             up   infinite      2   idle ag001,g002
data-science    up   infinite      1   down ds001
```

Ahí podemos ver que algunos nodos/particiones se encuentran sin uso `ìdle`, mientras que otras poseen algunos nodos en uso y otros en desuso `mix`, o se encuentran caídos `down`. Los nodos pueden encontrarse caídos por errores en su funcionamiento, fallas en la conectividad o por la realización de trabajos de mantenimiento. Si desea obtener mayores detalles del porqué del estado de un nodo caído, puede ejecutar el comando `sinfo -R`. El cual nos mostrará en patalla una salida similar a la siguiente:


```bash
REASON               USER      TIMESTAMP           NODELIST
Mantenimiento progra root      2025-05-05T17:56:50 ds001

```

En este caso podemos observar que el motivo de la caída del nodo es debido a unos trabajos de manteniento.


## Bonus: Obtener detalles de un job

En algunas ocasiones, olvidamos detalles del job que estamos ejecutando o que se encuentra por ejecutar. En esos casos podemos usar el comando `scontrol show job <job-id>` para obtener mayor información sobre ese job. 


Por ejemplo, si quiero ver detalles del job `1077` ejecutaré:

```bash
scontrol show job 1077
```

Y obtendré una respuesta en pantalla parecida a la siguiente:

```text
JobId=1077 JobName=CudaJob
   UserId=alan.turing(1000) GroupId=alan.turing(1000) MCS_label=N/A
   Priority=1683 Nice=0 Account=pregrado QOS=a-pregrado
   JobState=COMPLETED Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:00:02 TimeLimit=00:10:00 TimeMin=N/A
   SubmitTime=2024-11-06T02:41:48 EligibleTime=2024-11-06T02:41:48
   AccrueTime=2024-11-06T02:41:48
   StartTime=2024-11-06T02:41:48 EndTime=2024-11-06T02:41:50 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2024-11-06T02:41:48 Scheduler=Main
   Partition=debug AllocNode:Sid=khipu:2943597
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=n005
   BatchHost=n005
   NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   ReqTRES=cpu=1,mem=100M,node=1,billing=1
   AllocTRES=cpu=1,mem=100M,node=1,billing=1
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryCPU=100M MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=/home/alan.turing/gpu_job_shard.sh
   WorkDir=/home/alan.turing
   StdErr=/home/alan.turing/slurm-1077.out
   StdIn=/dev/null
   StdOut=/home/alan.turing/slurm-1077.out
   Power=

```
