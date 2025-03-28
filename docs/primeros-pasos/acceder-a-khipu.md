[putty]: https://www.putty.org/
[mesa-de-ayuda]: servicedesk.utec.edu.pe
[configurar-ssh-keys]: configurar-llaves-ssh.md

# Acceder a Khipu

!!! note "Nota"

    Para los siguientes comandos necesita tener una cuenta **activa** en Khipu. 

El acceso a Khipu se realiza a través de la :octicons-command-palette-16: interfaz de comandos o **terminal** que se encuentra disponible en la mayoría de sistemas operativos. Para ello se hace uso de un cliente SSH. En :material-microsoft-windows: Windows, tenemos clientes como `cmd`, `powershell` o [Putty][putty]. Mientras que en :fontawesome-brands-linux: Linux y :fontawesome-brands-apple: MacOS ya incluyen una terminal que viene por defecto.

## Inicio de sesión

- Abra una terminal y ejecute el siguiente comando. No olvide reemplazar <username> por su nombre de usuario. 

```bash
ssh <username>@khipu.utec.edu.pe
```

???+ note "Host keys"

    Si es la primera vez que accede a Khipu le aparecerá un mensaje similar al siguiente:
    
    ```shell
    The authenticity of host 'khipu.utec.edu.pe' can't be established.
    ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
    Are you sure you want to continue connecting (yes/no)?
    ```

    Ese mensaje de advertencia es **normal** y aparece la primera vez que su cliente SSH se conecta a un ordenador nuevo.  Escriba `yes` y presione ++enter++ para que se le solicite su contraseña de acceso.


- Escriba su contraseña y presione ++enter++ . Una vez hecho esto, usted habrá iniciado sesión en el cluster y tendrá el siguiente mensaje:

```shell
Last login: Mon Nov  4 17:30:01 2024 from *.*.*.*

         --*-*- Sustainable Computing Research Center -*-*--

                 _  ___     _             
                | |/ / |   (_)            
                | ' /| |__  _ _ __  _   _ 
                |  < | '_ \| | '_ \| | | |
                | . \| | | | | |_) | |_| |
                |_|\_\_| |_|_| .__/ \__,_| v3.0
                             | |          
                             |_|          

===========================================================================
  This system is for authorized users only and users must comply with all
  Khipu computing, network and research policies. All activity may be
  recorded for security and monitoring purposes. 
===========================================================================
 
            Docs          https://docs.khipu.utec.edu.pe
            Support       khipu@utec.edu.pe
 
 ===================[ Maintenance Information ]============================

 No maintenance information.

===========================================================================
 Good evening <username>!

```
!!! warning

    Ingresar su contraseña de manera incorrecta en un periodo corto de tiempo provocará que el acceso a Khipu desde su IP pública sea bloqueado.


- ¡Felicidades :material-emoticon-happy-outline:, ya se encuentra dentro de Khipu! 


- ¡Opcional! Revise la siguiente [guía][configurar-ssh-keys] si desea acceder a Khipu usando llaves SSH.


## Problemas frequentes


### 1. Timeouts

Si usted obtiene un mensaje de error parecido al siguiente:

```shell
ssh: connect to host khipu.utec.edu.pe port 22: Operation timed out

```
Pruebe connectarse a una conección de internet más estable como una red cableada o cambie de red WiFi. Si el error ocurre dentro del campus o se encuentra fuera de Perú, por favor genere un ticket en :material-headset: [mesa de ayuda][mesa-de-ayuda].

### 2. Fallas de autenticación

```shell
ssh: connect to host khipu.utec.edu.pe port 22: Connection refused
```

Como medida para prevenir ataque de fuerza bruta, Khipu cuenta con un bloqueo automatizado de las IPs desde las cuales se realizan inicios fallidos de sesión en  un periodo corto de tiempo. :warning: Si usted ingresa su contraseña de manera incorrecta más de tres veces seguidas, provocará que el acceso a Khipu desde su IP pública sea bloqueado y le aparecerá un mensaje como el de arriba. Para restablecer su contraseña o desbloquear el acceso desde su IP pública, debe generar un ticket en :material-headset: [mesa de ayuda][mesa-de-ayuda]. 
