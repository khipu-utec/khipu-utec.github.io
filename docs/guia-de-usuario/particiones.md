---
title: "Particiones"
weight: 33
---

Las particiones pueden ser consideradas como colas de trabajos, cada una de ellas con sus propios límites en cuanto al tamaño y la duración del job. Khipu posee cinco diferentes particiones en slurm.

| Nombre de partición | Descripción|
| :------------------:  | ---------- |
| debug | partición para probar ejecuciones pequeñas en CPU con propósitos de debugging |
| debug-gpu | partición para probar ejecuciones pequeñas en GPU con propósitos de debugging  |
| standard | partición de uso general en CPU  |
| big-mem | partición de uso general en CPU con gran cantidad de memoria |
| gpu | partición de uso general en GPU  |



 El número y el tamaño de los jobs permitidos depende de acuerdo a la partición seleccionada y su tipo de usuario. 


Para listar las particiones que se encuentran disponibles para sua usuario, ejecute el comando:

```shell
sinfo -O "partition"     
```

## Detalles de las particiones

| Partición | Nodos | Total de Cores | Total de Memoria RAM (GB) | Total shards de GPU | Máx duración del Job (min) |
| :--------:  | :--------:  |:--------------: | :-------------------------: | :-------------------: | :---: |
| debug | n005 | 16 | 32 | - | 30 |
| debug-gpu | g001 | 12 | 60 | 12 | 30 |
| standard | n00[3-5] | 80 | 352 | - | depende del tipo de usuario |
| big-mem | g00[2-3], ag001 | 54 | 2470 | - | depende del tipo de usuario |
| gpu | g00[1-3], ag001 | 190 | 630 | 190 | depende del tipo de usuario |



### debug

Tiempos de espera más cortos y tiempo de ejecución pequeño. Utilice esta partición para probar la ejecución de su job antes de enviarlo a una partición de más recursos.

### debug-gpu

Tiempos de espera más cortos y tiempo de ejecución pequeño. Utilice esta partición para probar la ejecución de su job en GPU antes de enviarlo a una partición de más recursos.

### standard

Partición de uso general para tareas que requieren cores de CPU. Utilice esta partición para ejecutar tareas intensas en uso de CPU. Los tiempos de espera en cola dependerán de la cantidad de usuarios que se encuentren usando la partición. Procure dimensionar correctamente su trabajo para disminuir su tiempo de espera en la cola. Use la partición `debug` para dicho propósito.

### big-mem

Partición de uso general para tareas que requieren cores de CPU y gran cantidad de memoria RAM. Utilice esta partición para ejecutar tareas intensas en memoria. Los tiempos de espera en cola dependerán de la cantidad de usuarios que se encuentren usando la partición. Procure dimensionar correctamente su trabajo para disminuir su tiempo de espera en la cola. Use la partición `debug` para dicho propósito.

### gpu

Partición de uso general para tareas que requieren cores de GPU. Es una de las particiones de mayor demanda en Khipu, es por ese motivo que no es posible reservar GPUs de manera exclusiva. El uso de la GPU es compartido a traves de [sharding](). Los tiempos de espera en cola dependerán de la cantidad de usuarios que se encuentren usando la partición. Procure dimensionar correctamente su trabajo para disminuir su tiempo de espera en la cola. Use la partición `debug-gpu` para dicho propósito.