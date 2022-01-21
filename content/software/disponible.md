---
title: "Software Disponible"
date: 2022-01-19T13:24:03-05:00

---

{{< hint info >}}
**Última actualización:** 23 Agosto 2020.
{{< /hint >}}

El cluster Khipu usa [CentOS 7 (Linux/GNU)](https://www.centos.org/) como sistema operativo principal y [Slurm](https://slurm.schedmd.com/overview.html) como manager de filas.
Para listar desde su terminal el software disponible para el usuario.

```shell
module avail
```

La siguiente tabla muestra los comandos para poder cargar y usar el software en su entorno. 

| Nombre | Versión | Comando de uso | Categoría |
| :---:  | :---:   | :---:          | :---:     |
| cmake |3.16.5 | module load cmake/3.16.5 | compiler |
| gcc   | 5.5.0 | module load gcc/5.5.0 | compiler |
| gcc | 6.5.0 | module load gcc/6.5.0 | compiler |
| gcc | 7.5.0 | module load gcc/7.5.0 | compiler |
| gcc | 8.4.0 | module load gcc/8.4.0 | compiler |
| gcc | 9.2.0 | module load gcc/9.2.0 | compiler |
| hdf5| 1.10.5 | module load hdf5/1.10.5 | lib |
| mpich | 3.1.4 | module load mpich/3.1.4 | compiler |
| mpich | 3.3.2 | module load mpich/3.3.2 | compiler |
| openmpi | 2.1.6| module load openmpi/2.1.6 | compiler|
| openmpi | 3.1.5| module load openmpi/3.1.5 | compiler|
| openmpi | 4.0.3 | module load openmpi/4.0.3 | compiler |
| python | 2.7.17 | module load python/2.7.17 | language |
| python | 3.7.7 | module load python/3.7.7 | language | 
| mpich | 1.5 | module load mpich/1.5 | compiler |
| mpich | 3.2.1 | module load mpich/3.2.1 | compiler |
| pmix | 2.1 | module load pmix/2.1 | lib |

**Ejemplo**

Para cargar gcc versión 7.5.0, usar el comando de uso sugerido:

```shell
module load gcc/7.5.0
```

Una vez terminado de el usar el software es recomendable hacer unload. Esto también se usa en scripts de jobs (ver ejemplos en la Guía de Uso). 


```shell
module unload gcc/7.5.0
```