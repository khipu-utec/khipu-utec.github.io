---
title: "Iniciar Sesión"
date: 2022-03-18T10:48:11-05:00
weight: 21
---
{{< toc >}}
## Inicio de sesión regular

Para poder iniciar sesión en Khipu deberemos seguir los siguientes pasos. 

1. Abrir un terminal y escribir el siguiente comando reemplazando su nombre de usuario en el campo <username>. 

```shell
ssh <username>@khipu.utec.edu.pe
```

**Ejemplo:**

```shell
ssh juana.perez@khipu.utec.edu.pe
```

2. Aparecerá el siguiente diálogo. Aceptar escribiendo yes y presionar enter. Luego colocar el password (este no aparecerá en pantalla al escribir) y presionar enter.

```shell
Are you sure you want to continue connecting (yes/no)?
juana.perez@khipu.utec.edu.pe's password:
```

3. Al lograr accesar al cluster aparecerá el prompt del terminal remoto. 

```shell
Last login: Fri Aug 21 11:28:13 2020 from 190.236.197.233
[juanaperez@khipu ~]$
```

4. Para cerrar o salir del cluster:

```shell
[juana.perez@khipu ~]$ exit
```
## Inicio de sesión con SSH Keys

{{< expand "Sobre las SSH Keys" >}}
## ¿Qué son las SSH Keys?
SSH o *Secure Shell Keys* son un conjunto de información que se emplea para identificar  y encriptar la comunicación entre su máquina local y un servidor. Se compone de dos archivos: una clave pública (`id_rsa.pub`) y otra privada (`id_rsa` o `id_rsa.ppk`). Se manera simple la clave pública es un "candado" y la clave privada es la llave para abrir dicho "candado". El "candado" (llave pública) puede ser compartida sin mayores problemas, en cambio si comparte su llave privada, alguien podría suplantar su identidad.

Cuando se conecta a un servidor, este presentará su llave pública. Usted provará su identidad mediante el uso de su llave privada. La comunicación con el servidor remoto y toda la información enviado durante este proceso será encriptada mediante su llave pública, de tal manera que solo usted pueda desencriptarla usando su llave privada. 
{{< /expand >}}

Si desea acceder a Khipu sin escribir su contraseña deberá copiar la llave pública de su computador al cluster. La llave pública debe haber sido creada previamente con el siguiente comando.

```shell
ssh-keygen
```

Su terminal responderá con:

```shell
Generating public/private rsa key pair.
Enter file in which to save the key (/home/username/.ssh/id_rsa):
```

Presione Enter para aceptar el valor por defecto. Su terminal responderá con:

```shell
Enter passphrase (empty for no passphrase):
```

Usted deberá eligir un *passphrase* seguro. Este valor le ayudará a prevenir el acceso a su cuenta en caso su clave privada es robada. Mientras escribe su passphrase este no aparecerá en pantalla. Su terminal responderá con:

```shell
Enter same passphrase again:
```

Ingrese el *passphrase* nuevamente. Una vez hecho esto se generará el par de llaves dentro de un directorio `.ssh` en su directorio `/home`. Si usted olvida su *passphrase*, no podrá recuperarlo. En cambio, usted deberá generar y copiar al servidor un nuevo par de SSH Keys.

A continuación se muestra como copiar su clave pública a Khipu desde diferentes sistemas operativos:
### Linux o MacOS

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub juana.perez@khipu.utec.edu.pe
```

### Windows

```powershell
type %HOMEPATH%\.ssh\id_rsa.pub | ssh juana.perez@khipu.utec.edu.pe "mkdir .ssh; cat >> .ssh/authorized_keys"
```
