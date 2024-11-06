---
title: "Infraestructura"
weight: 3
---

## Vista general

<p align="center">
  <img src="/khipu_nodes.png">
</p>

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

## Nodo de acceso

Es el nodo mediante el cual se accede al cluster. Es usado principalmente para compilar, enviar y monitorear los jobs.

<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>khipu</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 6230 CPU @ 2.10 GHz 20 cores por socket, 40 por nodo. </td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td>DRAM DDR4-1333 MHz, 128 GB por nodo </td>
        </tr>
        <tr>
            <td><b>Almacenamiento</b></td>
            <td>1TB SSD, 40 TB HDD </td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>

## Nodos CPU

Usados para procesamiento de jobs que requieren de CPU y RAM. Es gerenciado automáticamente por Slurm.

### n003

<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>n003</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 6130 CPU @2.10 GHz 16 cores por socket, 32 por nodo.</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td>157 GB DRAM DDR4-1333 MHz </td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>

### n004

<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>n004</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 6130 CPU @2.10 GHz 16 cores por socket, 32 por nodo.</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td>126 GB DRAM DDR4-1333 MHz </td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>

### n005

<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>n003</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 6130 CPU @2.10 GHz 16 cores por socket, 32 por nodo.</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td>94 GB DRAM DDR4-1333 MHz </td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>

## Nodos GPU

Usados para procesamiento de jobs que requieren de CPU, RAM y/o GPU. Es gerenciado automáticamente por Slurm.

### g001
<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>g001</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 6230 CPU @2.10 GHz 16 cores por socket, 32 por nodo. </td>
        </tr>
        <tr>
            <td><b>Gráficos</b></td>
            <td>NVIDIA Tesla T4 16 GB GDDR6</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td> 157 GB DRAM DDR4-1333 MHz</td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>

### g002
<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>g002</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 5418Y 2.0 GHz 24 cores por socket, 48 por nodo. </td>
        </tr>
        <tr>
            <td><b>Gráficos</b></td>
            <td>NVIDIA RTX A6000 48 GB GDDR6</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td> 1TB DRAM DDR5 5600MHz</td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband Mellanox MT28908</td>
        </tr>
    </tbody>
</table>


### g003
<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>g003</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 5418Y 2.0 GHz 24 cores por socket, 48 por nodo. </td>
        </tr>
        <tr>
            <td><b>Gráficos</b></td>
            <td>NVIDIA RTX A6000 48 GB GDDR6</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td> 1TB DRAM DDR5 5600MHz</td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband Mellanox MT28908</td>
        </tr>
    </tbody>
</table>

### ag001
<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>ag001</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>AMD EPYC 7742 64-Core Processor 64 cores por socket, 128 por nodo. </td>
        </tr>
        <tr>
            <td><b>Gráficos</b></td>
            <td>x2 NVIDIA A100 40 GB GDDR6</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td> 1TB DRAM DDR4</td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband Mellanox MT28908</td>
        </tr>
    </tbody>
</table>

</div>


## Software del sistema

- **Sistema Operativo:** Rocky Linux 8.10 (Green Obsidian)
- **Message Passing Library:** MPICH
- **Compiladores:** Intel, GCC
- **Job Scheduler:** SLURM 
- **Manejo de software:** Módulos de ambiente

## Aplicaciones de software

Revise [aquí](/guia_de_usuario/software/software_disponible ) la lista de softwares, herramientas y utilidades disponible en Khipu.