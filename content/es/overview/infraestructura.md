---
title: "Infraestructura"
date: 2022-03-18T10:40:40-05:00
weight: 20
---

{{< toc >}}

## Vista general

<p align="center">
  <img src="/overview/khipu_nodes.png">
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

## Nodo de administración

Nodo de acceso al cluster, usado principalmente para compilar y enviar trabajos.

<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>nLíder</td>
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
            <td>480 SSD, total 40 TB HDD </td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>

## Nodos de computación

Usado para procesamiento, gerenciado automáticamente por Slurm.

### Nodos CPU

<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>nCPU</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 6130 CPU @2.10 GHz 16 cores por socket, 32 por nodo.</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td>DRAM DDR4-1333 MHz, 128 GB por nodo </td>
        </tr>
        <tr>
            <td><b>Almacenamiento</b></td>
            <td>960 GB SSD</td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>

### Nodos GPU

<table>
    <thead>
        <tr>
            <th colspan="2">Especificaciones</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nombre</b></td>
            <td>nGPU</td>
        </tr>
        <tr>
            <td><b>Procesador</b></td>
            <td>Intel(R) Xeon(R) Gold 6230 CPU @2.10 GHz 20 cores por socket, 40 por nodo. </td>
        </tr>
        <tr>
            <td><b>Gráficos</b></td>
            <td>NVIDIA Tesla T4 16 GB GDDR6, PCIe 3.0 x16 1 GPU por nodo.</td>
        </tr>
        <tr>
            <td><b>Memoria</b></td>
            <td> DRAM DDR4-1333 MHz, 128 GB por nodo </td>
        </tr>
        <tr>
            <td><b>Almacenamiento</b></td>
            <td>480 GB SSD</td>
        </tr>
         <tr>
            <td><b>Red</b></td>
            <td>Infiniband FDR MT4119</td>
        </tr>
    </tbody>
</table>


</div>


## Software del sistema

- **Sistema Operativo:** CentOS Linux 7
- **Message Passing Library:** MPICH
- **Compiladores:** Intel, GCC
- **Job Scheduler:** SLURM 
- **Manejo de software:** Módulos de ambiente

## Aplicaciones de software

Revise [aquí](/guia_de_usuario/software/software_disponible ) la lista de softwares, herramientas y utilidades disponible en Khipu.