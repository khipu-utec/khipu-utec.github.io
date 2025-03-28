---
title: "Comandos básicos de SLURM"
weight: 31
---


## `srun` - Ejecutar trabajos

El comando `srun` es utilizado para ejecutar jobs de manera directa o interactiva. Se puede especificar cuántos nodos, CPUs, memoria, etc., se requiere utilizar.

**Sintaxis básica:**

```bash
srun [opciones] [comando]
```
Ejemplo: Ejecutar un script en un solo nodo con una CPU:

```bash
srun -n 1 --ntasks=1 --cpus-per-task=1 bash mi_script.sh
```

Donde `mi_script.sh` contiene lo siguiente:

```bash
#!/bin/bash

echo "Iniciando en $(date)"
echo "El job fue enviado a la partición ${SLURM_JOB_PARTITION}"
echo "Nombre del job: ${SLURM_JOB_NAME}, Job ID: ${SLURM_JOB_ID}"
echo "Tengo ${SLURM_CPUS_ON_NODE} CPUs en el nodo $(hostname)"
echo "Voy a dormir 10s para que me veas en la fila"
sleep 10 
```
Al pedir un job interactivo, se reservaran los recursos y se iniciará sesión en un shell de alguno de los nodos de cómputo. 

Ejemplo: Reservar un nodo de manera interactiva:

```bash
srun --pty -t 2:00 --mem=2G -p debug bash
```
El comando anterior asignará un CPU y 2GiB RAM por un periodo de 2 minutos. Durante ese tiempo podremos ejecutar comandos dentro de la shell de manera interactiva. Para salir de la sesión, deberemos ejecutar `exit` o presionar `Ctrl`+`d`.


## `sbatch` - Enviar trabajos a la cola

El comando sbatch se utiliza para enviar un trabajo a la cola de SLURM. Este comando es ideal para trabajos que se ejecutan en segundo plano, como tareas largas o de alto rendimiento.

**Sintaxis básica:**

```bash
sbatch [opciones] [archivo_de_script]
```

Ejemplo: Enviar un trabajo de script:

```
sbatch mi_script.sb
```


## `squeue` - Ver el estado de los trabajos

El comando squeue muestra los trabajos en ejecución y en espera en la cola de SLURM. Con ese comando nuede ver el estado de tus trabajos, qué partitición y nodos están utilizando, etc.

**Sintaxis básica:**

```bash
squeue [opciones]
```

Ejemplo: Ver los trabajos que he enviado:

```bash
squeue --me
```
Con ello obtendré una salida parecida a:

```text
[alan.turing@khipu ~]$ squeue
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
1077     debug  CudaJob  alan.turing  R       0:02      1 n005
```

## `scancel` - Cancelar trabajos

Este comando te permite cancelar trabajos en ejecución o en espera en la cola.

**Sintaxis básica:**

```bash
scancel [opciones] [job_id]
```

Ejemplo: Cancelar un trabajo con un ID específico:

```bash
scancel 1077
```

## `scontrol` - Controlar y gestionar trabajos

El comando scontrol le permite obtener información detallada sobre trabajos o recursos, y realizar operaciones de control (pausar, reanudar, etc.).

**Sintaxis básica:**

```bash
scontrol [opciones] [comando]
```

Ejemplo: Ver el estado de un trabajo:

```bash
scontrol show job 1077
```

Con ello obtendré una salida parecida a:

```text
[alan.turing@khipu ~]$ scontrol show job 1077
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
##  `sinfo` - Ver información sobre los nodos y particiones

Con sinfo puede obtener información sobre las particiones, su estado y nodos disponibles en el sistema.

**Sintaxis básica:**

```bash
sinfo [opciones]
```

Ejemplo: Ver el estado de las particiones:

```bash
sinfo
```
Ejemplo: Ver el estado de las particiones y la razón de dicho estado:

```bash
sinfo -R
```

