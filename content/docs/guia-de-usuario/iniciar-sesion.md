---
title: "Iniciar Sesión"
weight: 1
---
{{< callout type="info" >}}
  Para los siguientes comandos necesita tener una cuenta **activa** en Khipu. 
{{< /callout >}}


El acceso a Khipu se realiza a traves del protocolo [SSH](https://es.wikipedia.org/wiki/Secure_Shell). La mayoría de sistemas operativos dispone de clientes SSH. En Windows, tenemos clientes como `cmd`, `powershell` o [Windows Terminal](https://apps.microsoft.com/detail/9n0dx20hk701?hl=en-US&gl=US). Mientras que en Linux/MacOS tenemos un terminal que viene por defecto.

## Inicio de sesión con contraseña

Para iniciar sesión en Khipu deberemos abrir un terminal y ejecutar el siguiente comando. No olvide reemplazar <username> por su nombre de usuario. 

```shell
ssh <username>@khipu.utec.edu.pe

# Ejemplo:
ssh alan.turing@khipu.utec.edu.pe

```
**Host keys**

Si es la primera vez que accede a Khipu le aparecerá un mensaje similar al siguiente:
```shell
The authenticity of host 'khipu.utec.edu.pe' can't be established.
ED25519 key fingerprint is SHA256:bqxgyGvWm+3WVS0BTDfnlfmsUMeeaUgMXzGBfhLArCE.
Are you sure you want to continue connecting (yes/no)?
```

Ese mensaje de advertencia es **normal** y ocurre cada vez que su cliente SSH accede a un ordenador por primera vez. Escriba `yes` y presionar *enter* para que se le solicite su contraseña de acceso.

```shell
alan.turing@khipu.utec.edu.pe's password:
```
Escriba su contraseña y presione nuevamente `enter`. Una vez hecho esto, usted habrá iniciado sesión en Khipu y será recibido con el siguiente mensaje:

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
 Good evening alan.turing!

```

Al iniciar sesión usted notará que su shell a cambiado a:

```shell
[alan.turing@khipu ~]$:
```
Lo cual le indica que se encuentra en el nodo de acceso a Khipu, mas no de cómputo. 

{{< callout type="error" >}}
**No realizar cómputos en el nodo de acceso.**
Utilice Slurm para solicitar y acceder a un nodo de cómputo.
{{< /callout >}}

Por el contrario, cuando utilice Slurm y acceda a un nodo de cómputo, su shell cambiará a:

```shell
[alan.turing@n001 ~]$:
```
Donde `n001` es el nodo de cómputo donde se encuentra.

Si desea cerrar su sesión y salir de Khipu, deberá ejecutar:

```shell
exit
```
## Inicio de sesión con SSH Keys

Es posible acceder a Khipu sin escribir su contraseña. Para ello es necesario generar una llave pública y copiarla a Khipu. Puede generar su llave pública con el siguiente comando.

```shell
ssh-keygen -t ed25519 
```

Su terminal responderá con:

```shell
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/username/.ssh/ed25519):
```

Presione *enter* para aceptar el valor por defecto. Su terminal responderá con:

```shell
Enter passphrase (empty for no passphrase):
```
Puede presionar *enter* si no desea añadir un *passphrase*. Sin embargo, es recomendado escribir uno. El valor del *passphrase* le ayudará a prevenir el acceso a su cuenta en caso su clave privada es robada. Mientras escribe su passphrase este no aparecerá en pantalla. Su terminal responderá con:

```shell
Enter same passphrase again:
```

Ingrese el *passphrase* nuevamente. Una vez hecho esto se generará el par de llaves dentro de un directorio `.ssh` en su directorio `/home`. Si usted olvida su *passphrase*, no podrá recuperarlo. En cambio, usted deberá generar y copiar al servidor un nuevo par de llaves SSH.

A continuación se muestra como copiar su clave pública a Khipu desde diferentes sistemas operativos:

{{< tabs items="Linux / MacOS, Windows" >}}

  {{< tab >}}
  ```shell
  ssh-copy-id -i ~/.ssh/ed25519.pub alan.turing@khipu.utec.edu.pe
  ```
  {{< /tab >}}
  {{< tab >}}
    ```shell
    type %HOMEPATH%\.ssh\ed25519.pub | ssh alan.turing@khipu.utec.edu.pe "mkdir .ssh; cat >> .ssh/authorized_keys"
    ```
  {{< /tab >}}

{{< /tabs >}}


## Troubleshooting

### Timeouts

Si usted obtiene un mensaje de error parecido al siguiente:

```shell
ssh: connect to host khipu.utec.edu.pe port 22: Operation timed out

```
Pruebe connectarse a una conección de internet más estable como una cableada o cambie de red WiFi. Si el error ocurre dentro de campus, por favor escriba a [khipu@utec.edu.pe](mailto:khipu@utec.edu.pe).

### Fallas de autenticación

{{< callout type="warning" >}}
Ingresar reiteradas veces una contraseña incorrecta bloqueará permanentemente su dirección IP.
{{< /callout >}}

Como medida para prevenir ataque de fuerza bruta, Khipu cuenta con un bloqueo automatizado de todas aquellas IPs que ingresan múltiples veces una contraseña incorrecta en un periodo de tiempo determinado. Las IPs bloqueadas no podran acceder a Khipu de manera permanente. Cuando esto ocurre se mostrará un mensaje de error parecido al siguiente:

```shell
ssh: connect to host khipu.utec.edu.pe port 22: Connection refused
```

Si usted por error escribe múltiples veces su contraseña y su IP es bloqueada por favor contacte a [khipu@utec.edu.pe](mailto:khipu@utec.edu.pe).