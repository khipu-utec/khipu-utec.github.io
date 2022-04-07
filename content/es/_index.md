---
title: Inicio
geekdocNav: true
geekdocAlign: center
geekdocAnchor: false
showLastmod: false
showTitle: false
---
# Bienvenido a la documentación de Khipu

![](logo-tiny.png)

Esta es la documentación oficial del cluster Khipu. En este sitio encontrarás toda la información sobre el cluster, las políticas y guías de uso. 

{{< button size="large" relref="overview/khipu/" >}}Iniciar{{< /button >}}

## Enlaces Rápidos

{{< columns >}}

### Primeros Pasos


Si no sabe por donde empezar con la documentación, este es un buen punto de [comienzo](/overview/primeros_pasos/). 

<--->

### Guía de Usuario

Si desea información detallada sobre el uso y manejo del cluster, puede consultar [aquí](/guia_de_usuario/). 

<--->

### Reportes

Información actualizada cada dos minutos sobre el estado actual del cluster [está aquí](/reportes/). 

{{< /columns >}}


{{< columns >}}

### Anuncios

La información actualizada más relevante correspondiente al funcionamiento del cluster [está aquí](/anuncios/). 

{{< /columns >}}

<!-- 
{{< columns >}}

### Zero initial configuration

Getting started in minutes. The theme is shipped with a default configuration and works out of the box.

<--->

### Handy shortcodes

We included some (hopefully) useful custom shortcodes so you don't have to and can focus on writing amazing docs.

<--->

### Dark mode

Powerful dark mode that detects your system preferences or can be controlled by a toggle switch.

{{< /columns >}}



{{< columns >}}

## Sobre Khipu

Khipu es un [cluster](https://en.wikipedia.org/wiki/Cluster) dedicado a la computación de alto desempeño, en inglés [High performance Computing (HPC)](https://en.wikipedia.org/wiki/Supercomputer). Está formado por una colección de servidores distribuídos, llamados nodos que se encuentran conectados a través de una interconexión de alta velocidad (Infiniband).

Khipu es parte del [Centro de Investigación en Computación Sostenible (COMPSUST)](https://compsust.utec.edu.pe/) de la [Universidad de Ingeniería y Tecnología (UTEC)](https://utec.edu.pe/).

> **Contacto:** khipu@utec.edu.pe. \
> Para información de acceso revisar [aquí](/politica/reglas-de-uso/). 


## Grupos de usuarios

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



<--->

## Infraestructura

Para mayor información sobre programas, ver [Software](/software/disponible/)


![](infraestructura.png)

<style>
.myTable {
    border-radius: 5px;
}
.myTable th {
    background-color:var(--header-background); 
    color: white; 
}

</style>

<div class="myTable">

| nLíder | 
| -- | 
| Nodo de acceso al cluster, usado principalmente para compilar y enviar trabajos. 
**Procesadores:** Intel(R) Xeon(R) Gold 6230 CPU @ 2.10 GHz 20 cores por socket, 40 por nodo. 
**Memoria:** DRAM DDR4-1333 MHz, 128 GB por nodo 
**Disco local:** 480 SSD, total 40 TB HDD 
**Network:** Infiniband FDR MT4119 | 


| nCPU (1, 2, 3, 4, 5) | 
| -- | 
| Usado para procesamiento, gerenciado automáticamente por Slurm.
**Procesadores:** Intel(R) Xeon(R) Gold 6130 CPU @2.10 GHz 16 cores por socket, 32 por nodo.
**Memoria:** DRAM DDR4-1333 MHz, 128 GB por nodo 
**Disco local:**  960 GB SSD
**Network:** Infiniband FDR MT4119 | 

| nGPU 1 | 
| -- | 
| Usado para procesamiento, gerenciado automáticamente por Slurm.
**Procesadores:** Intel(R) Xeon(R) Gold 6230 CPU @2.10 GHz 20 cores por socket, 40 por nodo. 
**Gráficos:** NVIDIA Tesla T4 16 GB GDDR6, PCIe 3.0 x16 1 GPU por nodo. 
**Memoria:** DRAM DDR4-1333 MHz, 128 GB por nodo 
**Disco local:**  480 GB SSD
**Network:** Infiniband FDR MT4119 | 

| nGPU 2 | 
| -- | 
| Usado para procesamiento, gerenciado automáticamente por Slurm.
**Procesadores:** AMD EPYC 7742 @2.24 GHz 64 cores por socket, 128 por nodo. 
**Gráficos:** NVIDIA Tesla T4 16 GB GDDR6, PCIe 3.0 x16 1 GPU por nodo. 
**Memoria:** DRAM DDR4-1333 MHz, 128 GB por nodo 
**Disco local:**  480 GB SSD
**Network:** Infiniband FDR MT4119 | 


</div> -->
