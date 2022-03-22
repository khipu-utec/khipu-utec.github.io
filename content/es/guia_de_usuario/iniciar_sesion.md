---
title: "Iniciar Sesión"
date: 2022-03-18T10:48:11-05:00
weight: 21
---

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

5. **Opcional:**  para accesar sin escribir la contraseña deben copiar la llave pública de su computador hacia el cluster.
La llave pública debe haber sido creada previamente con el siguiente comando (recomendamos usar los valores por defecto presionando ENTER).

```shell
ssh-keygen
```

Linux o MacOS:

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub juana.perez@khipu.utec.edu.pe
```

Windows:

```powershell
type %HOMEPATH%\.ssh\id_rsa.pub | ssh juana.perez@khipu.utec.edu.pe "mkdir .ssh; cat >> .ssh/authorized_keys"
```
