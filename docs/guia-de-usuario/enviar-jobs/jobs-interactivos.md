---
title: "Jobs Intercativos"
---

[particiones]:/guia-de-usuario/enviar-jobs/particiones/

Para iniciar un job de manera interactiva deberá ejecutar el comando `srun` usando la siguiente sintaxis:

```bash
srun [recursos] --pty /bin/bash
```
Por defecto todos los jobs interactivos tienen una duración límite de 30 minutos. Usted puede modificar los recursos a reservarse y el tiempo límite de duración modificando `[recursos]` con los flags listados aquí. 

Por ejemplo:

- Si desea solicitar **4 cores** y **4GB RAM** en la [partición][particiones] `standard`.

```bash
srun -c 4 --mem=4GB -p standard --pty /bin/bash
```

- Si desea solicitar **1 shard GPU** en la [partición][particiones] `debug-gpu`.

```bash
srun --gres=shard:1 -p debug-gpu --pty /bin/bash
```

!!! warning Manténgase conectado mientras usa *jobs* interactivos
    Si usted pierde conexión, deja de tener actividad o sale de su sesión, perderá acceso a su job interactivo. Para mitigar la cancelación de su job por inactividad puede usar el comando `screen`. Sin embargo, úselo de manera razonable ya que usted estará manteniendo en reserva recursos que no está usando.