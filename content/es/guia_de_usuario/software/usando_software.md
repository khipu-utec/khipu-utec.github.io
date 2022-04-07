---
title: "Usando Software"
date: 2022-03-18T10:49:20-05:00
weight: 32
---

{{< toc >}}

## Módulos

### Buscar módulos

Usted también puede buscar módulos mediante el comando `avail`. Por ejemplo, para listar todas las versiones de MPICH 3, ejecutaremos:

```shell
module avail mpich/3
```

Y obtendremos una salida similar a:

```shell

--------------------------------------- /opt/apps/modulefiles ----------------------------------------
mpich/1.5   mpich/3.1.4 mpich/3.2.1 mpich/3.3.2 mpich/3.4.0 mpich/4.0
```

### Cargar módulos

Usted puede cargar un modulo a su ambiente de trabajo a traves del comando `module load`. Al cargar un modulo, usted carga todas las variables de ambiente necesarias para poder utilizar dicho paquete de software. 

Es importante tomar en cuenta que:

- El comando es case-sensitive a los nombres de los módulos
- Cuando usted usa `module load` se cargan también las dependencias del modulo. No es necesario cargarlas de manera separada.
- Cuando usted enviá sus jobs que requieren de un módulo, no olvide añadirlo con `module load` a su bash script.

Por ejemplo, si desea cargar gcc versión 7.5.0, deberá ejecutar:


```shell
module load gcc/7.5.0
```

También puede cargar varios módulos al mismo tiempo:

```shell
module load gcc/7.5.0 mpich/3.3.2
```

### Descargar módulos

Una vez terminado de usar el software, es recomendable hacer `module unload <modulename>`. Este proceso también se debe realizar e los scripts de los jobs.

```shell
module unload gcc/7.5.0
```

También puede descargar todos los módulos a la vez con:

```shell
module purge
```

### Colección de módulos

En casos cuando se trabaja con gran cantidad de módulos, puede ser molestoso estar cargando cada uno de ellos. Ante esto existe la opción de añadir dicho módulos a una colección y de este modo solo cargar la colección en lugar que todos los módulos de manera individual. 

**Guardar colecciones**

Para ello, usted deberá cargar previamente los módulos que desea añadir a la colección y despues ejecutar. 

```shell
module save <collection-name>
```
**Cargar colecciones**

Usted podrá cargar la colección creada con:

```shell
module restore <collection-name>
```
**Listar colecciones**

Para listar las colecciones creadas:

```shell
module savelist
```
### Obtener información y ayuda de los módulos

Si desea mostrar la información de configuración de un modulo en especifico:

```shell
module show <modulename> #or
module display <modulename>
```

Usted puede obtener una breve descripción sobre un módulo ejecutando:

```txt
module help <modulename>/<version>
```

Si usted no encuentra un paquete y considera que su instalación como modulo podría ser beneficiosa para otros usuarios más, puede escribirnos a [khipu@utec.edu.pe](khipu@utec.edu.pe) para solicitar su instalación.


<!-- 
---

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




Una vez terminado de el usar el software es recomendable hacer unload. Esto también se usa en scripts de jobs (ver ejemplos en la Guía de Uso). 


```shell
module unload gcc/7.5.0
https://docs.ycrc.yale.edu/clusters-at-yale/applications/modules/
``` -->