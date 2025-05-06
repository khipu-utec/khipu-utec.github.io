!!! info "Recomendación General"
    Asegúrese de reservar la cantidad de RAM y CPUs necesarios para la ejecución de su **job**. No reserve recursos que no necesita, ya que de hacerlo, perjudicará la ejecución de los demás usuarios del cluster. 

A continuación se muestran algunos ejemplos de como medir el uso de CPU y RAM de su job a fin de que pueda refinar la reserva de recursos.  

### Jobs en ejecución

Si su **job** se encuentra en ejecución, usted puede revisar su uso actual de recursos. Sin embargo, deberá esperar hasta su finalización para ver el uso máximo de recursos durante toda su ejecución. 

La manera más sencilla de revisar el uso instantáneo de recursos es hacer crear un *job* interactivo en el nodo de computación donde su *job* se encuentra ejecutándose. Para saber en que nodo debe crear el *job* interactivo, ejecute:

```shell
squeue --me
```
El cual nos da como salida: 
```text
JOBID PARTITION     NAME     USER  ST       TIME  NODES NODELIST(REASON)
21615 standard    bert-sar juan   PD       0:00      1 n003
```

En ella podemos notar que su job **bert-sar** se encuentra ejecutandose en el nodo `n003` de la parición `standard`. Con esa información crearemos el job interactivo.

```shell
srun --pty -t 02:00 --mem=1G -p standard --nodelist=n003 bash
```

Una vez dentro del nodo de cómputo, ejecutaremos `ps` o `htop`.

- `ps` le brindará la información instantánea del uso de recursos cada vez que ejecute el comando. 

    ```
    [alan.turing@n004 ~]$ ps -u$USER -o %cpu,rss,args
    %CPU   RSS COMMAND
    0.0  2376 python triangle.py
    0.0  2380 python triangle.py
    0.0  2380 python triangle.py
    0.0  2380 python triangle.py
    0.0  2380 python triangle.py
    0.0  2380 python triangle.py
    0.0  2380 python triangle.py
    0.0  2380 python triangle.py
    0.0  2380 python triangle.py
    ```

    El reporte de memoria de `ps` se muestra en KB, podemos notar que los procesos listados consumen alrededor de 2000 KB de RAM y que el uso de los CPUs es casi nulo.

- `htop` se ejecuta de manera interactiva y muestra las estadísticas de uso en vivo. Puede presionar la tecla `u`, ingresar su nombre de usuario y luego `enter` para filtrar solo sus procesos. La información del uso de memoria, se encuentra en la columna RES. Para solicitar ayuda puede presionar `?` y si desea salir `q` .

![Monitorear con Htop](/assets/images/monitorear-htop.webp){ loading=lazy }


### Jobs finalizados

Slurm guarda las estadísticas de cada job, incluído cuanta memoria y CPU fue utilizada.


#### sacct

También se puede usar `sacct` para obtener la información del job. Lamentablemente, el output por defecto de `sacct` no es del todo entendible, por ello se recomienda procesar la salida de la siguiente manera. 

```
[alan.turing@khipu ~]$ export SACCT_FORMAT="JobID%20,JobName,User,Partition,NodeList,Elapsed,State,ExitCode,MaxRSS,AllocTRES%32"
[alan.turing@khipu ~]$ sacct -j 21886
     JobID    JobName      User  Partition        NodeList    Elapsed      State ExitCode     MaxRSS                        AllocTRES 
---------- ---------- --------- ---------- --------------- ---------- ---------- -------- ---------- -------------------------------- 
      1063 simple_ex+   alan.tur+      debug            n005   00:00:11  COMPLETED      0:0             billing=2,cpu=2,mem=200M,node=1 
1063.batch      batch                                 n005   00:00:11  COMPLETED      0:0                       cpu=2,mem=200M,node=1 
```