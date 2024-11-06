---
title: "Bienvenido a Khipu!"
---

Khipu es un cluster de Computación de Alto Desempeño ([HPC](https://es.wikipedia.org/wiki/Computaci%C3%B3n_de_alto_rendimiento) en inglés) que forma parte del [Centro de Investigación para la Computación Sostenible (COMPSUST)](https://compsust.utec.edu.pe) de la [Universidad de Ingeniería y Tecnología (UTEC)](https://utec.edu.pe). En él es posible realizar ejecuciones que requieran gran poder de cómputo ya sea en CPU o GPU. 


## ¿Cuanto cuesta acceder a Khipu?

Khipu es **gratuito** para todos los miembros de la comunidad de UTEC y sus proyectos asociados que requieran de recursos computacionales para sus labores de investigación. Cualquier miembro de UTEC puede solicitar su acceso a Khipu a travez del formulario de [registro](https://web.khipu.utec.edu.pe/register/) que se encuentra en el portal web. Una vez su solicitud haya sido revisada, le serán enviadas sus credenciales de acceso al correo registrado en el formulario. 

{{< callout type="info" >}}
  Por el momento no se dispone de acceso para personas sin afiliación a UTEC.
{{< /callout >}}

## Agradecimiento / citación

Alentamos a todos a los usuarios cuyo proyecto culmine en una publicación a añadir al cluster Khipu dentro de los agradecimientos. Puede añadir un mensaje de agradecimiento como el siguiente:

> “El trabajo computacional del presente proyecto fue apoyado por los recursos del cluster HPC Khipu (https://web.khipu.utec.edu.pe/) de la Universidad de Ingeniería y Tecnología (UTEC)"

## Soporte

Las preguntas, pedidos y solicitudes de asistencia serán atendidas a través del correo [khipu@utec.edu.pe](mailto:khipu@utec.edu.pe). Si su consulta es sobre la ejecución de su trabajos por favor incluya los jobs ids,  comandos utilizados y los mensajes de error recibidos. Esto nos ayudará a hacer más efectiva su atención.

## Inicio Rápido

Si ya recibió sus credenciales de acceso le presentamos tres tareas básicas que le serán de utilidad al momento de usar Khipu. 

{{% steps %}}

### Acceder a Khipu

```bash
ssh <mi-usuario>@khipu.utec.edu.pe
```

### Revisar software disponible

```bash
ml av # or
module avail
```

### Revisar cola de Slurm

```bash
squeue # or
squeue -u <mi-usuario>
```

{{% /steps %}}
