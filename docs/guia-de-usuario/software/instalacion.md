[lista-de-software]: /guia-de-usuario/software/lista

# Instalación de Software

El siguiente documento describe como instalar software en Khipu para un proyecto o curso. Antes de realizar una instalación, por favor revise si el software [ya se encuentra disponible][lista-de-software] como módulo con el comando `module avail` o `ml av`.


## Requisitos para instalar software

- En caso de software licenciado, el usuario debe proveer la licencia y/o instalador del software al administrador para ser instalado. 
- El usuario puede instalar el software y libraries necesarios en su `/home/<username>/`, en caso de no requerir licencia o ser de código abierto. 
- El usuario puede compilar en el nodo líder antes de enviar un job a ejecutar. No enviar jobs de compilación a la fila del cluster. 
- Siendo Khipu un cluster enfocado al procesamiento y HPC; no se instalarán bases de datos, containers o máquinas virtuales. 
- Si el software o library requiere permisos especiales para instalación, solamente el profesor encargado del curso o un investigador de proyecto pueden solicitar la instalación bajo coordinación con el administrador. 
<!-- - En caso de necesitar libraries adicionales de Python, no crear ambientes virtuales para instalar. Solamente el profesor encargado del curso o un investigador de proyecto pueden solicitar la instalación bajo coordinación con el administrador.  -->
- Si el software que planea instalar posee dependencias como librerías, interfaces o módulos; revise primero si ya se encuentran disponibles en el cluster antes de instalarlas por su cuenta. 
  
Para entrar en contacto con el administrador del cluster, por favor enviar un mensaje a [khipu@utec.edu.pe](mailto:khipu@utec.edu.pe).

<!-- > Por favor entrar en contacto en caso se requiera un software o library adicional que va a ser usado por múltiples investigadores o participantes de un curso que no ha sido especificado en el Formulario de Solicitud de Acceso.  -->

## Proceso de instalación

Tal como se mencionó anteriormente, usted puede instalar software libre o que no requiere licencia en su espacio personal de trabajo. Sin embargo, antes de proceder con la instalación asegúrese que dicho software no requiere permisos **root** para ser instalado. Una vez echo esto, usted deberá moverse al directorio en el cual planea realizar la instalación y seguir las instrucciones del proveedor del software que planea instalar. 

Por lo general, los pasos son similares, es por ello que a continuación se muestra un ejemplo realizando la instalación de la librería [Zlib](https://www.zlib.net/), un software usado para comprimir y descomprimir data.

- Creamos un directorio en el cual almacenaremos los códigos fuente y binarios de las apps que vamos a instalar. 

```shell
mkdir myApps
cd myApps
APPS_DIR=$(pwd)
mkdir sources      # source files
mkdir apps         # binary files
```


- Entraremos al directorio `sources/` y descargaremos el código fuente del paquete desde su [sitio web](https://www.zlib.net/).

```shell
cd $APPS_DIR/sources
wget  https://www.zlib.net/zlib-1.2.12.tar.xz
```

- Descomprimimos el archivo descargado.

```shell
tar -xvf zlib-1.2.12.tar.xz
```

- Entramos al nuevo directorio con los contenidos del paquete que acabamos de descomprimir.

```shell
cd zlib-1.2.12/
```

- Cargamos el módulo del compilador para poder crear los binarios a partir del código fuente. En este caso cargaremos GCC 7.5.0, sin embargo es recomendable seguir los requisitos de cada software. 

```shell
module load gcc/7.5.0
```

- Por lo general, muchos código fuentes se compilan usando simplemente `make` y `make install`. Sin embargo, nosotros debemos configurar previamente el lugar donde se guardaran los binarios, ya que si no hacemos esto, se guardaran en los directorios `/usr/lib` a los cuales no tenemos acceso. Por lo general, bastará con configurar el argumento `--prefix`.

```shell
./configure --prefix=$APPS_DIR/apps/zlib
```

Para mayor información de como configurar el path de instalación, es recomendable seguir la documentación del software a instalar.

- Compilamos el código fuente.
  
```shell
make 
```

- Instalamos el software.

```shell
make install    # copia los binarios a --prefix
```

- Si nos movemos al directorio donde se encuentra la instalación, encontraremos por lo general una lista de directorios como la siguiente.
  
```shell
cd $APPS_DIR/apps/zlib
ls
```

```
# salida del comando anterior
include  lib  share
```

- En esos directorios se encuentran los binarios con los cuales podremos hacer uso del software.
