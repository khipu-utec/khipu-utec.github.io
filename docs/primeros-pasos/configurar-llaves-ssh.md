# Configurar llaves SSH

Es posible acceder a Khipu usando un par de llaves SSH. La llave pública deberá copiarla a Khipu, mientras que la privada debe permanecer en su ordenador. 

### Generación de llaves SSH

=== "Desde una terminal (:fontawesome-brands-linux: Linux, :fontawesome-brands-apple:  MacOS o :material-microsoft-windows: Windows)"

    El par de llaves SSH puede ser generado usando :fontawesome-brands-linux: Linux, :fontawesome-brands-apple:  MacOS o :material-microsoft-windows: Windows. Para ello abra una terminal en su computadora local y ejecute el siguiente comando:

    ```shell
    ssh-keygen -t ed25519
    ```

    Su terminal responderá preguntando por un nombre y lugar donde guardar el par de llaves. Si desea optar por la opción por defecto, presione ++enter++. Si desea un nombre y lugar distino escríbalo en pantalla. Tambien puede especificar el lugar y nombre de manera directa añadiendo el flag `-f <filename>` al comando  anterior. 
    
    A continuación se muestra un ejemplo del comando completo, no olvide reemplazar `<username>` por su nombre de usuario:

    ```shell
    ssh-keygen -t ed25519 -f /home/<username>/.ssh/id_ed25519
    ```

    Luego se le pedirá que ingrese un *passphrase* (1). Este debe tener una longitud de por lo menos 8 caracteres (preferiblemente 12). El *passphrase* debe ser preferiblemente una combinación de números, letras y caracteres especiales para tener mayor seguridad. Una vez escrito, presione ++enter++ para continuar.
    { .annotate }

    1.  :information_source: Un *passphrase* es una secuencia de palabras o una frase completa que se utiliza como contraseña.


    !!! info "Olvido de *passphrase*"
        :octicons-alert-24: Si usted olvida su **passphrase**, no podrá recuperarlo. En cambio, usted deberá eliminar el par de llaves anterior y generar una nuevas.

    Finalmente se generarán un par de llaves SSH en la locación que especificó (`/home/<username>/.ssh/` en el ejemplo anterior). El par de llaves incluye una llave privada `id_ed25519` y una pública `id_ed25519.pub`. 

    !!! warning "Advertencia"
        La llave privada no debe ser compartida con **nadie**, incluyendo Khipu. Esta llave debe permanecer almacenada en el ordenador donde fue generada. La llave pública es la única que será compartida y almacenada en el cluster. Utilice un *passphare* robusto para proteger sus llaves en caso de robo y prevenir un uso no autorizado de las mismas.
        


### Copiar su llave pública a Khipu


A continuación se muestra como copiar su clave pública a Khipu desde diferentes sistemas operativos:

=== "Desde una terminal (:fontawesome-brands-linux: Linux y  :fontawesome-brands-apple:  MacOS)"
    
    Ejecute desde su terminal `ssh-copy-id -i <ubicación-de-la-llave-pública> <usuario>@khipu.utec.edu.pe`. Por ejemplo, para las llaves generadas en los pasos anteriores el comando a ejecutar será:

    ```shell
    ssh-copy-id -i /home/<username>/.ssh/id_ed25519.pub <username>@khipu.utec.edu.pe
    ```
=== ":material-microsoft-windows: Windows"

    Si el par de llaves del ejemplo anterior hubieran sido generadas en :material-microsoft-windows: Windows,  el comando a usar será: 
    ```shell
    type %USERPROFILE%\.ssh/id_ed25519.pub | ssh <username>@khipu.utec.edu.pe "mkdir -p .ssh && cat >> .ssh/authorized_keys"
    ```

    También, puede optar por copiar el contenido de la llave pública `id_ed25519.pub` y escribirlo en Khipu en `.ssh/authorized_keys` usando un editor de texto como `vim` o `nano`.

!!! info
    Una vez que su llave pública fue copiada en Khipu correctamente, usted podrá acceder a Khipu usando sus llaves SSH. A diferencia del inicio de sesión con contraseña, el *passphrase* le será solicitado una sola vez por inicio de sesión de su ordenador local. 