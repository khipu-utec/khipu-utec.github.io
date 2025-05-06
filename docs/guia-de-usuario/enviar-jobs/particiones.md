---
title: "Particiones"
weight: 31
---

Las particiones son grupos de nodos que tienen características similares y que comparten un mismo conjunto de restricciones.

Las particiones permiten organizar los recursos disponibles y gestionar de manera eficiente cómo se asignan las tareas o trabajos. Khipu posee seis diferentes particiones en Slurm.

| Nombre de partición | Descripción|
| :------------------:  | ---------- |
| debug | partición para probar ejecuciones pequeñas en CPU con propósitos de debugging |
| debug-gpu | partición para probar ejecuciones pequeñas en GPU con propósitos de debugging  |
| standard | partición de uso general en CPU  |
| big-mem | partición de uso general en CPU con gran cantidad de memoria |
| gpu | partición de uso general en GPU  |
| data-science | partición de Ciencia de Datos  |


El número de *jobs* y la cantidad de recursos que pueden ser reservados dependerá de la partición seleccionada y su tipo de usuario. Para listar las particiones que se encuentran disponibles para su usuario, ejecute el comando:

```shell
sinfo -O "partition"     
```

## Detalles de las particiones

A continuación se listan las particiones existentes y el detalle de cada una de ellas.

| Partición | Nodos | Total de Cores | Total de Memoria RAM (GB) | Total shards de GPU | Máx duración del Job (min) |
| :--------:  | :--------:  |:--------------: | :-------------------------: | :-------------------: | :---: |
| debug | n003 | 32 | 160 | - | 30 |
| debug-gpu | g001 | 32 | 160 | 32 | 30 |
| standard | n00[3-6] | 144 | 1385 | - | depende del tipo de usuario |
| big-mem | g002, ds001, ag001 | 224 | 3095 | - | depende del tipo de usuario |
| gpu | g00[1-2], ag001 | 208 | 2224 | 208 | depende del tipo de usuario |
| data-science | ds001 | 96 | 1031 | 96 | depende del tipo de usuario |


### debug

Tiempos de espera más cortos y tiempo de ejecución pequeño. Utilice esta partición para probar la ejecución de su job antes de enviarlo a una partición de más recursos.

### debug-gpu

Tiempos de espera más cortos y tiempo de ejecución pequeño. Utilice esta partición para probar la ejecución de su job en GPU antes de enviarlo a una partición de más recursos.

### standard

Partición de uso general para tareas que requieren cores de CPU. Utilice esta partición para ejecutar tareas intensas en uso de CPU. Los tiempos de espera en cola dependerán de la cantidad de usuarios que se encuentren usando la partición. Procure dimensionar correctamente su trabajo para disminuir su tiempo de espera en la cola. Use la partición `debug` para dicho propósito.

### big-mem

Partición de uso general para tareas que requieren cores de CPU y gran cantidad de memoria RAM. Utilice esta partición para ejecutar tareas intensas en memoria. Los tiempos de espera en cola dependerán de la cantidad de usuarios que se encuentren usando la partición. Procure dimensionar correctamente su trabajo para disminuir su tiempo de espera en la cola. Use la partición `debug` para dicho propósito.

### gpu

Partición de uso general para tareas que requieren cores de GPU. Es una de las particiones de mayor demanda en Khipu, es por ello que no es posible reservar GPUs de manera exclusiva. El uso de la GPU es compartido a traves de [sharding](). Los tiempos de espera en cola dependerán de la cantidad de usuarios que se encuentren usando la partición. Procure dimensionar correctamente su trabajo para disminuir su tiempo de espera en la cola. Use la partición `debug-gpu` para dicho propósito.

### data-science

Partición de uso general, pero con acceso priorizado para los miembros de la facultad de Ciencia de Datos. Dentro de esta partición se encuentra el nodo `ds001`, el único nodo de cómputo con acceso a internet. Cuenta con una tarjeta GPU compartida a través de [sharding](). 