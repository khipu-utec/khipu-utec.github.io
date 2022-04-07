---
title: "Software Disponible"
date: 2022-03-18T10:48:49-05:00
weight: 31
---

{{< toc >}}

Para poder satisfacer las diferentes necesidades de nuestros usuarios, hacemos uso de [sotfware modules](https://modules.readthedocs.io/en/latest/index.html) a fin de poder hacer disponibles multiples versiones de software. Los módulos ayudan a cambiar entre diferentes aplicaciones y sus versiones con relativa facilidad, especialmente en ambientes compartidos. 
## Módulos disponibles

Usted puede listar los módulos que se encuentran disponibles con el siguiente comando.

```shell
module avail
```

A continuación se comparte una muestra de la salida del comando anterior.

```shell
--------------------------------------- /opt/apps/modulefiles ----------------------------------------
armadillo/10.8.2               go/1.17.6                      mpich/4.0
brams/5.3                      gromacs/2020.1                 namd/2019.07.11-multicore
cmake/3.16.5                   gromacs/2020.3                 namd/2022.04.05-multicore
cmake/3.22.2                   gromacs/gpu-2020.1             namd/2022.04.05-multicore-CUDA
cuda/10.2                      gromacs/gpu-2020.3             openfoam/v2112
cuda/11.4                      intel-oneapi/2022.1.2          pmix/2.1
gaussian/1.1.0                 julia/1.5.3                    python/2.7.17
gcc/10.1.0                     meep/1.17.1                    python/3.7.7
gcc/5.5.0                      metis/5.1.0                    python/3.9.2
gcc/6.5.0                      mpich/1.5                      speedtest/latest
gcc/7.5.0                      mpich/3.1.4                    telemac/8.3.0
gcc/8.4.0                      mpich/3.2.1                    valgrind/3.15.0
gcc/9.2.0                      mpich/3.3.2                    valgrind/3.18.1
go/1.14.6                      mpich/3.4.0                    wrf/4.2
```

A continuación se muestra la lista de software disponible por categorías. 

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
<table>
    <thead>
        <tr>
            <th>Categoría</th>
            <th>Componente</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Sistema Operativo</b></td>
            <td>CentOS Linux 7.6 x86_64</td>
        </tr>
        <tr>
            <td><b>Gestor de trabajos</b></td>
            <td>Slurm</td>
        </tr>
        <tr>
            <td style="text-align: center;" colspan="2">Herramientas de desarrollo</td>
        </tr>
        <tr>
            <td><b>Editores de texto</b></td>
            <td>Vim, Nano</td>
        </tr>
        <tr>
            <td><b>Sistema de Control de Versiones<b/></td>
            <td>Git</td>
        </tr>
        <tr>
            <td><b>Compiladores</b></td>
            <td>GNU (gcc, g++, gfortran), CMake, Intel </td>
        </tr>
        <tr>
            <td><b>Lenguajes</b></td>
            <td>C, C++, Python, R, Java, Go, Julia</td>
        </tr>
        <tr>
            <td><b>Debugging, profiling, tracing</b></td>
            <td>GDB, CGDB, Valgrind</td>
        </tr>
        <tr>
            <td><b>Distribución MPI</b></td>
            <td>MPICH</td>
        </tr>
        <tr>
            <td><b>GPU</b></td>
            <td>CUDA</td>
        </tr>
        <tr>
            <td style="text-align: center;" colspan="2">Otros</td>
        </tr>
        <tr>
            <td><b>Ingeniería</b></td>
            <td>OpenFOAM</td>
        </tr>
        <tr>
            <td><b>Geo-sotfware</b></td>
            <td>WRF, BRAMS, Telemac</td>
        </tr>
        <tr>
            <td><b>Bioinformática</b></td>
            <td>NAMD</td>
        </tr>
    </tbody>
</table>
</div>