# Uso de software

## Módulos

Usted puede listar los módulos que se encuentran disponibles con el siguiente comando.

```shell
ml av 
# O también
module avail
```

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

Si usted no encuentra un paquete y considera que su instalación como modulo podría ser beneficiosa para otros usuarios más, puede escribirnos a [khipu@utec.edu.pe](mailto:khipu@utec.edu.pe) para solicitar su instalación.
