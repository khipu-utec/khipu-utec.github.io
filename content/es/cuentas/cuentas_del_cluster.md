---
title: "Grupos de Cuentas"
date: 2022-03-18T10:46:56-05:00
weight: 11
---

Existen dos grupos de usuarios gerenciados automáticamente por [Slurm](): 1) investigación, y 2) educación.
Ambos grupos acceden al cluster por el nodo líder para enviar trabajos a la fila de ejecución.


### Grupo Educación 

Permite el uso del cluster para aula o laboratorio por los estudiantes a pedido de un instructor(a) registrado(a).
Este grupo es financiado por la universidad y sus trabajos serán procesados bajo las siguientes características:

{{< hint info>}}
{{< icon "gdoc_home" >}} Ocupar un nodo CPU y/o GPU. Equivalente a un total de 72 cores, 320 GB de memoria y 1.4 TB de almacenamiento.\
{{< icon "gdoc_timer" >}} Tiempo máximo de ejecución de trabajo: 2 horas.\
{{< icon "gdoc_code" >}} Número de procesos concurrentes: 2.\
{{< icon "gdoc_menu" >}} Cantidad de cores por proceso 8. 
{{< /hint >}}

### Grupo Investigación 

Este grupo está financiado por UTEC, proyectos de investigación (PI), Departamentos y Dirección de Escuela.
Los fondos centrales cubren costos de infraestructura, operación y soporte. Los PI y algunas unidades y departamentos financian la adquisición de nuevos nodos de procesamiento y almacenamiento.
Los usuarios de este grupo ejecutan trabajos en todos los nodos bajo las siguientes características: 

{{< hint info>}}
{{< icon "gdoc_home" >}} Ocupar cualquier nodo CPU y/o GPU. Equivalente a un total de 200 cores, 1.1 TB de memoria y 5.3 TB de almacenamiento.\
{{< icon "gdoc_timer" >}} Tiempo máximo de ejecución de trabajo NaN.\
{{< icon "gdoc_code" >}} Número de procesos concurrentes NaN.\
{{< icon "gdoc_menu" >}} Cantidad de cores por proceso NaN. 
{{< /hint >}}