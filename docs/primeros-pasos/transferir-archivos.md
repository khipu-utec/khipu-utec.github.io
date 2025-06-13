# Transferir archivos hacia/desde Khipu

!!! info

    Se recomienda realizar regularmente copias de sus archivos en Khipu a sus ordenadores locales. Es importante recordar que **Khipu no es un  almacenamiento personal en la nube** y que la información almacenada no posee respaldo. 

La transferencia de archivos hacia/desde Khipu puede ser realizada usando `scp` o `rsync`.

## Copiando archivos usando `scp`

???+ abstract "Secure Copy Protocol `scp`"
    `scp` es un comando que permite copiar de manera secura archivos y directorios desde dos ubicaciones remotas. Con este comando podremos copiar archivos o directorios desde nuestro ordenador local a uno remoto y viceversa. Cuando se transfiere datos mediante `scp` los archivos y contraseñas son encriptados para evitar que alguien tenga acceso no autorizado mientras se realiza el proceso de copia. 

La sintaxis básica del comando `scp` es la siguiente:

```shell
scp <path-origen> <path-destino>
```
A continuación se muestran algunos ejemplos:

- Copiar desde mi ordenador local a Khipu
    
    ```bash
    # Copiar `mi-archivo.txt` al directorio `/home/<usuario-en-khipu>` (`~`) en Khipu
    scp mi-archivo.txt <usuario-en-khipu>@khipu.utec.edu.pe:~

    # Copiar `mi-folder/` al directorio `/home/<usuario-en-khipu>` (`~`) en Khipu
    # El flag -r indica que se esta ejecutando el comando de manera recursiva.
    scp -r mi-folder/ <usuario-en-khipu>@khipu.utec.edu.pe:~
    ```

- Copiar de Khipu a mi ordenador local

    ```bash
    # Copiar `mi-archivo-en-khipu.txt` al directorio actual de mi ordenador
    scp mi-usuario@khipu.utec.edu.pe:~/path/a/mi-archivo-en-khipu.txt .

    # Copiar `mi-folder-en-khipu/` al directorio actual de mi ordenador
    scp -r mi-usuario@khipu.utec.edu.pe:~/path/a/mi-folder-en-khipu/ .
    ```

## Copiando archivos usando `rsync`

???+ abstract "Remote Synchronization `rsync`"
    `rsync` es una herramienta de sincronización entre archivos remotos y locales. Este comando minimiza la cantidad de datos copiados, ya que solo copia aquellas partes que cambiaron entre ambos directorios. Este comando es recomendado para la transferencia de archivos de gran tamaño ya que mantiene un registro del progreso. De esta manera, si la transmisión es interrumpida, `rsync` continuará desde donde se quedó, sin necesidad de iniciar desde cero.


La sintaxis básica para el uso de `rsync` es la siguiente:

```shell
rsync <opciones> <path-origen> <path-destino>
rsync <opciones> <path-origen-local> <usuario-en-khipu>@khipu.utec.edu.pe:<path-destino-en-Khipu>
rsync <opciones> <usuario-en-khipu>@khipu.utec.edu.pe:<path-origen-en-Khipu> <path-destino-local>
```

A continuación se muestran algunos ejemplos del uso de `rsync`.

- Sincronizar desde mi ordenador local a Khipu
    
    ```bash
    # El flag -a indica que se trata de archivos
    rsync -a ~/path/en/mi-folder <usuario-en-khipu>@khipu.utec.edu.pe:~/path/en/khipu

    # El flag -az indica que se trata de archivos que se van a comprimir
    rsync -az ~/path/en/mi-folder <usuario-en-khipu>@khipu.utec.edu.pe:~/path/en/khipu

    ```

- Sincronizar un directorio remoto a uno local

    ```bash
    # El flag -a indica que se trata de archivos
    rsync -a <usuario-en-khipu>@khipu.utec.edu.pe:~/path/en/khipu ~/path/en/mi-folder-local

    # El flag -azP indica que se trata de archivos que se van a comprimir y se muestra el progreso
    rsync -azP <usuario-en-khipu>@khipu.utec.edu.pe:~/path/en/khipu ~/path/en/mi-folder-local
    ```
