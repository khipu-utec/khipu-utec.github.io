---
title: "Carpeta `/local`"
weight: 31
---

Cada nodo de cómputo en Khipu cuenta con almacenamiento local (discos) que se encuentra configurado para montarse en la carpeta `/local`. Esta carpeta tiene los mismos permisos que una carpeta `/tmp`, y su función es la de un almacenamiento temporal para tareas que requieran escribir sobre archivos.

## Cómo utilizar `/local`
Los nodos de cómputo tienen acceso a la carpeta `/home` a través de NFS. Esto quiere decir que cualquier tarea en ejecución tendrá acceso a nuestra carpeta personal. Sin embargo, el acceso constante de una tarea a la carpeta `/home` no es deseable porque las lecturas y escrituras tienen un costo adicional de comunicación por red, que puede además saturar el ancho de banda.

Como alternativa, previo a la creación de una tarea un usuario puede crear una carpeta temporal en `/local`, donde su job realizará las lecturas y escrituras que necesita, sin necesidad de comunicarse por red para acceder a la carpeta `/home`. El siguiente script muestra el escenario descrito:

```bash
#!/bin/bash

#SBATCH --job-name=test
#SBATCH -p standard
#SBATCH --output=test_%j.out
#SBATCH --error=test_%j.err
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=32
#SBATCH --time=01:00:00

# Crear el directorio de trabajo en nodo de cómputo
mkdir -p /local/nombre.apellido/$SLURM_JOB_ID

# Copiar contenido necesario para la ejecución y moverse a la carpeta /local
cp -rp /home/mpintol/Documents/mt-test/* /local/nombre.apellido/$SLURM_JOB_ID
cd /local/nombre.apellido/$SLURM_JOB_ID

## Ejecutar el programa
srun ./test
```

Podemos observar que la carpeta `/local/nombre.apellido/$SLURM_JOB_ID` fue creada y contiene los archivos utilizados durante la ejecución de la tarea:

```bash
[nombre.apellido@n003 local]$ pwd
/local
[nombre.apelido@n003 local]$ ls -R nombre.apellido/
nombre.apellido/:
42641

nombre.apellido/42641:
test  test_42641.err  test_42641.out  test.sh
```

!!! warning "La carpeta raiz `/local` es compartida"
    Asegurarse de siempre crear una carpeta personal dentro de `/local` para manejar sus archivos. La carpeta `/local` es de por sí compartida y si se crean archivos directamente en esta ubicación, cualquier usuario podría modificarlos o eliminarlos