# Apptainer

Apptainer (anteriormente conocido como Singularity) es una plataforma de contenedores de código abierto, segura, portable y fácil de usar que permite a los usuarios crear y ejecutar contenedores que empaquetan software de forma portable y reproducible. Puedes construir un contenedor para Apptainer en tu laptop y luego ejecutarlo en otra PC, estación de trabajo, clúster HPC, servidor en la nube, etc. Apptainer permite a usuarios sin privilegios usar contenedores y prohíbe la escalada de privilegios dentro del contenedor; los usuarios son los mismos dentro y fuera del contenedor. Apptainer puede importar cualquier contenedor de registros OCI (Open Containers Initiative), lo que significa que puedes descargar, ejecutar y construir desde la mayoría de los contenedores en Docker Hub sin modificaciones. Más información sobre Apptainer se puede encontrar en su [sitio web oficial](https://apptainer.org/).

## Descripción general de la interfaz de Apptainer

Los comandos de Apptainer pueden ejecutarse de forma nativa en el nodo maestro o en cualquier nodo de cómputo, sin necesidad de cargar ningún módulo adicional.

El comando `help` ofrece una descripción general de las opciones y subcomandos de Apptainer:

```bash
$ apptainer help

Linux container platform optimized for High Performance Computing (HPC) and
Enterprise Performance Computing (EPC)

Usage:
  apptainer [global options...]

Description:
  Apptainer containers provide an application virtualization layer enabling
  mobility of compute via both application and environment portability. With
  Apptainer one is capable of building a root file system that runs on any
  other Linux system where Apptainer is installed.

Options:
      --build-config    use configuration needed for building containers
  -c, --config string   specify a configuration file (for root or
                        unprivileged installation only) (default
                        "/etc/apptainer/apptainer.conf")
  -d, --debug           print debugging information (highest verbosity)
  -h, --help            help for apptainer
      --nocolor         print without color output (default False)
  -q, --quiet           suppress normal output
  -s, --silent          only print errors
  -v, --verbose         print additional information
      --version         version for apptainer

Available Commands:
  build       Build an Apptainer image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  checkpoint  Manage container checkpoint state (experimental)
  completion  Generate the autocompletion script for the specified shell
  config      Manage various apptainer configuration (root user only)
  delete      Deletes requested image from the library
  exec        Run a command within a container
  help        Help about any command
  inspect     Show metadata for an image
  instance    Manage containers running as services
  key         Manage OpenPGP keys
  keyserver   Manage apptainer keyservers
  oci         Manage OCI containers
  overlay     Manage an EXT3 writable overlay image
  plugin      Manage Apptainer plugins
  pull        Pull an image from a URI
  push        Upload image to the provided URI
  registry    Manage authentication to OCI/Docker registries
  remote      Manage apptainer remote endpoints
  run         Run the user-defined default command within a container
  run-help    Show the user-defined help for an image
  search      Search a Container Library for images
  shell       Run a shell within a container
  sif         Manipulate Singularity Image Format (SIF) images
  sign        Add digital signature(s) to an image
  test        Run the user-defined tests within a container
  verify      Verify digital signature(s) within an image
  version     Show the version for Apptainer

Examples:
  $ apptainer help <command> [<subcommand>]
  $ apptainer help build
  $ apptainer help instance start

For additional help or support, please visit https://apptainer.org/help/

```

## Descarga de imágenes

- Las **imágenes de contenedor** son ejecutables que agrupan todos los componentes necesarios para una aplicación o entorno, como una plantilla para los contenedores.
- Los **contenedores** son la instancia en ejecución de las imágenes.

Los contenedores preconstruidos pueden obtenerse de diversas fuentes como:

- Apptainer/Singularity Hub
- [Docker Hub](https://hub.docker.com/)
- [NVIDIA NGC Catalog](https://catalog.ngc.nvidia.com/containers)
- Otros registros OCI

Puedes usar los comandos [pull](https://apptainer.org/docs/user/main/cli/apptainer_pull.html#apptainer-pull) y [build](https://apptainer.org/docs/user/main/cli/apptainer_build.html#apptainer-build) para descargar imágenes de un recurso externo como Docker:

```bash
(master)$ apptainer pull docker://alpine
(master)$ apptainer build <container-name>.sif docker://alpine
```

¡Recuerda! Los comandos `pull` y `build` deben ejecutarse en el nodo maestro. El nodo maestro es el único con acceso a internet.

## Interacción con contenedores existentes

### Run

Una vez descargada la imagen, estás listo para ejecutarla. Como ejemplo, descargaremos el contenedor ***lolcow*** y lo ejecutaremos.

```bash
# Container downloading
(master)$ apptainer pull docker://ghcr.io/apptainer/lolcow
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Copying blob 5ca731fc36c2 done   | 
Copying blob 16ec32c2132b done   | 
Copying config fd0daa4d89 done   | 
Writing manifest to image destination
2025/02/28 14:04:53  info unpack layer: sha256:16ec32c2132b43494832a05f2b02f7a822479f8250c173d0ab27b3de78b2f058
2025/02/28 14:04:54  info unpack layer: sha256:5ca731fc36c28789c5ddc3216563e8bfca2ab3ea10347e07554ebba1c953242e
INFO:    Creating SIF file...

# Container running in the system host
(master)$ apptainer run lolcow_latest.sif      
 ______________________________
< Fri Feb 28 14:05:17 -03 2025 >
 ------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
                

```

Como podemos ver, el contenedor fue descargado como un archivo de imagen de Apptainer `lolcow_latest.sif`. Este archivo fue ejecutado con el comando `apptainer run <container-image-name>.sif`. Sin embargo, `run` no es el único comando para ejecutar e interactuar con un contenedor; lo discutiremos en las siguientes líneas.

### Shell

Puedes crear un nuevo shell dentro de tu contenedor e interactuar con él como si fuera una máquina virtual.

```bash
(master)$ apptainer shell lolcow_latest.sif   
Apptainer>
```

El cambio en el prompt (de `(master)$` a `Apptainer>`) indica que ahora estás dentro del contenedor. Además, eres el mismo usuario que en el sistema anfitrión y tienes acceso al directorio home del usuario.

```bash
# I executing apptainer in the master node
Apptainer> hostname
khipu

# My user in the host system is aturing, in apptainer too
Apptainer> whoami
aturing

# I can access my home directory inside the container
Apptainer> pwd
/home/aturing
```

Nota: Puedes ejecutar `apptainer shell` en el nodo maestro, pero se recomienda hacerlo usando un trabajo interactivo de Slurm. Para ello, debes agregar el siguiente comando antes de tus comandos de Apptainer: `srun --pty --mem=2G -p debug <apptainer command>`.

A continuación se muestra el ejemplo anterior usando un trabajo interactivo de Slurm:

```bash
# I will execute an apptainer shell in a compute node
srun --pty --mem=2G -p debug apptainer shell lolcow_latest.sif 
Apptainer> hostname
n003
```

Recuerda que los trabajos en la partición `debug` tienen un límite de tiempo de 30 minutos. Para trabajos de larga duración, debes ejecutarlos usando un trabajo batch de Slurm. Explicaremos esto más adelante.

### Exec

Con el comando `exec` puedes ejecutar un comando personalizado dentro de un contenedor. Por ejemplo, para ejecutar el programa `cowsay` dentro del contenedor `lolcow_latest.sif`:

```bash
[rubaldo@khipu ~]$ apptainer exec lolcow_latest.sif cowsay Khipu
 _______
< Khipu >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

## Trabajo con archivos

Los archivos del sistema anfitrión son accesibles desde dentro del contenedor.

```bash
[rubaldo@khipu ~]$ echo "Hello from Khipu" > $HOME/testfile.txt
[rubaldo@khipu ~]$ apptainer exec lolcow_latest.sif cat $HOME/testfile.txt
Hello from Khipu
```

Por defecto, Apptainer monta `$HOME`, el directorio de trabajo actual y ubicaciones adicionales del sistema anfitrión dentro del contenedor.

Referencias:

- [https://apptainer.org/docs/user/main/quick_start.html](https://apptainer.org/docs/user/main/quick_start.html)

## Construcción de imágenes personalizadas

Apptainer permite a los usuarios construir contenedores a partir de un archivo de definición (como Docker con un Dockerfile). Dentro de este archivo puedes agregar variables de entorno o instalar dependencias de software para reproducir y compartir fácilmente tus contenedores.

Un archivo de definición tiene una cabecera y un cuerpo. La cabecera determina el contenedor base con el que comenzar, y el cuerpo se divide en secciones que realizan tareas como la instalación de software, la configuración del entorno y la copia de archivos al contenedor desde el sistema anfitrión. Más información sobre cómo crear el archivo de definición de Apptainer se puede encontrar [aquí](https://apptainer.org/docs/user/main/definition_files.html).

Por ejemplo, crearemos un archivo `lolcow.def` para definir un contenedor basado en **ubuntu** e instalar **cowsay** dentro de él.

```bash
BootStrap: docker
From: ubuntu:24.04

%post
   apt-get -y update
   apt-get -y install cowsay lolcat

%environment
   export LC_ALL=C
   export PATH=/usr/games:$PATH

%runscript
   date | cowsay | lolcat
   
%labels
   Author Khipu
   Exec: apptainer

```

En este ejemplo, la cabecera le indica a Apptainer que comience con la imagen base `ubuntu:24.04` de la biblioteca de contenedores de Docker. La sección `%post` se ejecuta en tiempo de construcción después de que la imagen base ha sido descargada. En este ejemplo, la usamos para actualizar la biblioteca de paquetes e instalar `cowsay` y `lolcat`. La sección `%environment` define variables de entorno para el contenedor. La sección `%runscript` se usa para definir las acciones del contenedor cuando es ejecutado (estos comandos no se ejecutan en tiempo de construcción). Finalmente, la sección `%labels` se usa para colocar información sobre el contenedor, como el autor, cómo usarlo, ejemplos, etc.

Para construir el contenedor a partir del archivo anterior, necesitamos ejecutar `apptainer build <container-name>.sif <container-definition-file>.def`:

```bash
[rubaldo@khipu apptainer]$ apptainer build lolcow.sif lolcow.def 
INFO:    Starting build...
...
INFO:    Adding labels
INFO:    Adding environment to container
INFO:    Adding runscript
INFO:    Creating SIF file...
INFO:    Build complete: lolcow.sif

```

Luego, para ejecutar el contenedor basta con ejecutar `apptainer run lolcow.sif` o `./lolcow.sif`.

## Soporte para GPU

Apptainer tiene soporte para contenedores que usan el framework de computación GPU CUDA de NVIDIA. Las siguientes líneas muestran cómo crear y ejecutar contenedores con GPU en Khipu.

### Descarga de imágenes

El primer paso, como se [mostró anteriormente](https://google.com), es descargar y construir la imagen del contenedor. Como ejemplo, descargaremos una imagen nvidia-cuda12.8.0.

```bash
[rubaldo@khipu]$ apptainer pull docker://nvidia/cuda:12.8.0-base-ubuntu20.04
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
...
INFO:    Creating SIF file...

# Or
[rubaldo@khipu]$ apptainer build cuda12.sif docker://nvidia/cuda:12.8.0-base-ubuntu20.04
```

Recuerda: el comando `apptainer pull <something>` solo funciona en el nodo maestro.

Los nodos GPU en Khipu son accesibles a través de Slurm. En las siguientes líneas mostraré cómo compilar y ejecutar tu código CUDA usando Apptainer y Slurm.

- Como ejemplo, usa el código [gpu-info.cu](http://gpu-info.cu) y compílalo en el nodo maestro:

```bash
# Load cuda module
[rubaldo@khipu]$ ml load cuda
[rubaldo@khipu]$ nvcc -o gpu_info gpu_info.cu
```

- Luego, para ejecutar el código:

```bash
[rubaldo@khipu]$ srun --mem=1G -p gpu apptainer exec --nv cuda_12.8.0-base-ubuntu20.04.sif ./gpu_info
CUDA Device(s) Found: 1
Device 0 Information:
Name: Tesla T4
Compute Capability: 7.5
Total Global Memory: 14915 MB
Shared Memory per Block: 48 KB
Registers per Block: 65536
Max Threads per Block: 1024
Max Block Dimensions: (1024, 1024, 64)
Max Grid Dimensions: (2147483647, 65535, 65535)
Clock Rate: 1590 MHz
Memory Clock Rate: 5001 MHz
Memory Bus Width: 256 bits
Total Constant Memory: 64 KB
Warp Size: 32
Multiprocessor Count: 40

```

Como puedes ver, `--nv` se pasa al comando `exec` para configurar el entorno y usar una GPU NVIDIA con las bibliotecas básicas de CUDA. Sin este flag, los dispositivos y bibliotecas CUDA no pueden ser utilizados. Este flag no es necesario en tiempo de construcción.

- Se recomienda ampliamente compilar el código CUDA en el nodo maestro, ya que las imágenes CUDA con el compilador de NVIDIA incluido (imágenes `devel`) son mucho más grandes que las imágenes `base`.

Como prueba de concepto, podemos compilar el mismo código usando Apptainer. Para ello necesitamos descargar una imagen CUDA de tipo `devel`, como **nvidia/cuda:12.5.1-devel-ubuntu20.04** (3.6 GB), que es más de 30 veces más grande que una imagen `base` (~95 MB).

```bash
[rubaldo@khipu]$ apptainer pull docker://nvidia/cuda:12.5.1-devel-ubuntu20.04
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
...
INFO:    Creating SIF file...
```

Para compilar el código, ejecuta:

```bash
[rubaldo@khipu]$ srun --mem=1G -p gpu apptainer exec --nv cuda_12.5.1-devel-ubuntu20.04.sif nvcc -o gpu_info gpu_info.cu
```

Y finalmente para ejecutar el código:

```bash
[rubaldo@khipu]$ srun --mem=1G -p gpu apptainer exec --nv cuda_12.5.1-devel-ubuntu20.04.sif ./gpu_info
CUDA Device(s) Found: 1
Device 0 Information:
Name: Tesla T4
Compute Capability: 7.5
Total Global Memory: 14915 MB
Shared Memory per Block: 48 KB
Registers per Block: 65536
Max Threads per Block: 1024
Max Block Dimensions: (1024, 1024, 64)
Max Grid Dimensions: (2147483647, 65535, 65535)
Clock Rate: 1590 MHz
Memory Clock Rate: 5001 MHz
Memory Bus Width: 256 bits
Total Constant Memory: 64 KB
Warp Size: 32
Multiprocessor Count: 40

```

Para practicar los comandos anteriores, puedes intentar ejecutarlos en todos nuestros nodos GPU agregando `--nodelist=<nombre del nodo gpu>` al comando `srun`. Para ver la lista de nodos GPU, ejecuta `sinfo`.

En todos los ejemplos anteriores se usó el comando `srun` para lanzar los trabajos de Slurm. Este comando es adecuado para trabajos de corta duración, pero si tu trabajo requiere mucho más tiempo, debes lanzarlo en modo batch con `sbatch`.

Más información sobre el soporte GPU de Apptainer se puede encontrar [aquí](https://apptainer.org/docs/user/latest/gpu.html).

## Modo interactivo

A veces quieres interactuar con tu contenedor como si estuvieras en un shell. Puedes hacerlo usando un trabajo interactivo de Slurm. Los trabajos interactivos deben ejecutarse en la partición `debug-gpu` y tienen un límite de tiempo de 30 minutos. Recuerda que los contenedores en Apptainer son inmutables después de la construcción.

```bash
[rubaldo@khipu]$ srun --pty -p debug-gpu apptainer run --nv cuda_12.8.0-base-ubuntu20.04.sif
Apptainer>

# To check the nvidia driver
Apptainer> nvidia-smi
Sun Mar  2 09:20:22 2025       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 560.35.03              Driver Version: 560.35.03      CUDA Version: 12.6     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  Tesla T4                       On  |   00000000:37:00.0 Off |                    0 |
| N/A   78C    P0             51W /   70W |   13831MiB /  15360MiB |     55%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
...

```

## Caché de Apptainer

Cuando generas una imagen SIF desde fuentes remotas, Apptainer almacena la imagen en caché. Por defecto, la carpeta de caché se crea en la variable de entorno `HOME` (`$HOME/.apptainer/cache`). El comando `apptainer cache` te permite listar y limpiar tu caché.

```bash
# List your cache
[rubaldo@khipu]$ apptainer cache list
There are 4 container file(s) using 7.31 GiB and 88 oci blob file(s) using 13.18 GiB of space
Total space used: 20.49 GiB

# The same command with details
apptainer cache list -v
NAME                     DATE CREATED           SIZE             TYPE
03bb9eb021f579ed871d5f   2025-03-02 07:52:09    4.41 MiB         blob
093f9cb4ed5900be5b069a   2025-02-08 14:25:21    0.18 KiB         blob
...

There are 4 container file(s) using 7.31 GiB and 88 oci blob file(s) using 13.18 GiB of space
Total space used: 20.49 GiB

# Clean you cache
apptainer cache clean

# Clean you cache files older than 15 days
apptainer cache clean --days 15

```

Intenta liberar tu caché de Apptainer regularmente.

## **Script de envío a SLURM**

Puedes lanzar tus trabajos de Apptainer en modo batch con un script de Slurm. Se recomienda ampliamente cuando tu trabajo tomará mucho tiempo de ejecución. Como ejemplo, el contenedor `lolcow_latest.sif` del primer ejemplo se ejecutará usando un archivo de envío `lolcow.slurm`.

```bash
#!/bin/bash

## Slurm Directives
#SBATCH --job-name=apptainer_lolcow
#SBATCH --output=apptainer_lolcow-%j.out
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=1G
#SBATCH -p debug

## Load modules, if needed

## Place to the directory where you container is, for example $HOME
cd $HOME

## Run the program using use 'srun'.
srun apptainer run lolcow_latest.sif 
```

En el script anterior, se solicitó **`1 CPU`** con **`1GB de RAM`** por CPU en la partición `debug` para la tarea. Este contenedor se ejecutará una sola vez (`--ntasks=1`); si deseas ejecutarlo más veces, intenta cambiar el valor de `--ntasks=`, pero recuerda que esto solo funciona cuando el comando se ejecuta con `srun`.

Para enviar este script, simplemente ejecuta:

```bash
[rubaldo@khipu]$ sbatch lolcow.slurm
# When the job finish, check you output with cat
[rubaldo@khipu]$ cat apptainer_lolcow-4377.out 
 _____________________________
< Mon Mar 2 11:36:49 -05 2025 >
 -----------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

Para trabajos con GPU NVIDIA, en este caso usando la imagen `cuda_12.8.0-base-ubuntu20.04.sif`, el script de Slurm `gpu_info.slurm` debe ser:

```bash
#!/bin/bash

## Slurm Directives
#SBATCH --job-name=apptainer_gpu_info
#SBATCH --output=apptainer_gpu_info-%j.out
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=1G
#SBATCH --gres=gpu:1
#SBATCH -p debug-gpu

## Load modules, if needed

## Place to the directory where you container is, for example $HOME
cd $HOME/apptainer

## Run the program using use 'srun'.
srun apptainer exec --nv cuda_12.8.0-base-ubuntu20.04.sif ./gpu_info
```

Luego, para enviar este script:

```bash
[rubaldo@khipu]$ sbatch gpu_info.slurm
# Wait until the job finish. Then you will have an output like this:
[rubaldo@khipu]$ cat apptainer_gpu_info-4576.out 
CUDA Device(s) Found: 1
Device 0 Information:
Name: Tesla T4
Compute Capability: 7.5
Total Global Memory: 14915 MB
Shared Memory per Block: 48 KB
Registers per Block: 65536
Max Threads per Block: 1024
Max Block Dimensions: (1024, 1024, 64)
Max Grid Dimensions: (2147483647, 65535, 65535)
Clock Rate: 1590 MHz
Memory Clock Rate: 5001 MHz
Memory Bus Width: 256 bits
Total Constant Memory: 64 KB
Warp Size: 32
Multiprocessor Count: 40

```

Si deseas recibir una notificación cuando tu trabajo falle o finalice, no olvides agregar las siguientes líneas a tu script de Slurm:

```bash
#SBATCH --mail-type=fail        # send email when job begins
#SBATCH --mail-type=end          # send email when job ends
#SBATCH --mail-user=<your email>
```

Referencias:

[https://hpc.nmsu.edu/discovery/software/apptainer/using-containers/](https://hpc.nmsu.edu/discovery/software/apptainer/using-containers/)
