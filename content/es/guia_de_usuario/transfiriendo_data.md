---
title: "Transfiriendo Data"
date: 2022-03-18T10:58:17-05:00
weight: 50
---

{{< toc >}}


Se recomienda realizar regularmente copias de sus archivos en Khipu a sus ordenadores locales. Es importante recordar que **Khipu no puede ser usado como almacenamiento personal en la nube**. Por otro lado, se recomienda comprimir previamente los archivos que se van a transferir a fin de agilizar el proceso.
## Transferir archivos usando scp

{{< expand "¿Qué es 'scp'?" >}}
## Secure Copy Protocol (scp)
`scp` es un comando que permite copiar de manera secura archivos y directorios desde dos ubicaciones remotas. Con este comando podremos copiar archivos o directorios desde nuestro ordenador local a uno remoto y viceversa. Cuando se transfiere datos mediante `scp` los archivos y contraseñas son encriptados para evitar que alguien tenga acceso no autorizado mientras se realiza el proceso de copia. 

A continuación se muestran diversos ejemplos de como usar el comando `scp`.
{{< /expand >}}



- Copiar desde mi ordenador local a Khipu
    
    ```bash
    # Copiar my-archivo.txt a mi directorio /home en Khipu
    scp my-archivo.txt my-usuario@khipu.utec.edu.pe:~

    # Copiar my-folder/ a mi directorio /home en Khipu.
    # El flag -r indica que se esta ejecutando el comando de manera recursiva.
    scp -r my-folder my-usuario@khipu.utec.edu.pe:~
    ```

- Copiar de Khipu a mi ordenador local

    ```bash
    # Copiar my-archivo.txt a mi directorio actual en mi ordenador
    scp my-usuario@khipu.utec.edu.pe:~/path/to/my-archivo.txt .

    # Copiar my-folder/ a mi directorio actual en mi ordenador
    scp -r my-usuario@khipu.utec.edu.pe:~/path/to/my-folder .
    ```
> Mayor información: [https://linux.die.net/man/1/scp](https://linux.die.net/man/1/scp)

## Transferir archivos usando rsync

{{< expand "¿Qué es 'rsync'?" >}}
## Remote Synchronization (rsync)
`rsync` (Remote Synchronization) es una herramienta de sincronización entre archivos remotos y locales. Este comando minimiza la cantidad de datos copiados, ya que solo copia aquellas partes que cambiaron entre ambos directorios. 

A continuación se muestran algunos ejemplos del uso de `rsync`.{{< /expand >}}

```shell
# Sincronizar un directorio local a uno remoto. 
# El flag -a indica que se trata de archivos
rsync -a ~/path/to/my-folder my-usuario@khipu.utec.edu.pe:~/path/in/khipu

# Sincronizar un directorio local a uno remoto
rsync -a  my-usuario@khipu.utec.edu.pe:~/path/to/my-folder ~/path/in/local

# Sincronizar un directorio local a uno remoto comprimiendo previamente
# El flag -az indica que se trata de archivos que se van a comprimir
rsync -az ~/path/to/my-folder my-usuario@khipu.utec.edu.pe:~/path/in/khipu

# Sincronizar un directorio remoto a uno local comprimiendo previamente y mostrando el progreso
# El flag -azP indica que se trata de archivos que se van a comprimir y se muestra el progreso
rsync -azP my-usuario@khipu.utec.edu.pe:~/path/to/my-folder ~/path/in/local
```

> Mayor información: [https://linux.die.net/man/1/rsync](https://linux.die.net/man/1/rsyncl)
## Revisar la cantidad de almacenamiento usado

El comando `du` (disk usage) es empleado para conocer el espacio que ocupa un archivo o directorio. A continuación se muestran algunos ejemplos útiles:

```bash
# Revisar cuanto almacenamiento voy usando en mi /home
cd ~; du -hs

# Revisar cuanto almacenamiento voy usando por cada carpeta en mi /home 
cd ~; du -d 1 -h

# Revisar cuanto almacenamiento voy usando por cada carpeta en mi /home y ordenar por tamaño 
cd ~; du -d 1 -h | sort -hr
```

> Mayor información: [https://man7.org/linux/man-pages/man1/du.1.html](https://man7.org/linux/man-pages/man1/du.1.html)