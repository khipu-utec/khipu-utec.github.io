---
title: "Primeros Pasos"
date: 2022-03-18T10:44:02-05:00
weight: 10
---

{{< toc >}}
## Clusters HPC Khipu

Visto de manera sencilla, Khipu es una colección de computadoras que que se encuentran conectadas entre sí y que trabajan como si fueran una sola. A cada computador conectado se le conoce como **nodo**. El nodo de acceso, o también llamado **nodo líder**, es aquel al cual nos conectamos de manera remota para acceder al cluster. Los nodos que se encargan de la ejecución de nuestros **jobs** o trabajos, son llamados **nodos de cómputo**. Para enviar un job hacemos uso de un **scheduler**, el cual se encargado de manejar los diferentes jobs que se envían y garantizar equidad al momento que pasan a ejecutarse. Todos los nodos de computo, poseen un [filesystem](https://en.wikipedia.org/wiki/File_system) compartido lo cual permite a los jobs acceder y modificar la data de cualquier nodo sin la necesidad de copiarlo al otro.
## Solicitar una cuenta

Para poder acceder al cluster Khipu es necesario solicitar una cuenta. Las cuentas se dividen en dos grupos: [educación e investigación](/cuentas/cuentas_del_cluster/). Las solicitudes de cuentas no implican costo alguno para los miembros de UTEC y pueden ser solicitadas por el docente del curso o el encargado del proyecto de investigación. Mayores detalles sobre las cuentas en Khipu lo encuentra [aquí](/cuentas/).

<div style="text-align: center;">
{{< button href="https://web.khipu.utec.edu.pe/register/" size="large">}}Abrir solicitud de acceso{{< /button >}}
</div>

Una vez completado el formulario y aprobada la solicitud de acceso, las credenciales serán enviadas a los correos registrados. A partir del momento en que se reciben las credenciales, el usuario se compromete a cumplir las [políticas de uso](). 

## Iniciar Sesión

Una vez adquirida una cuenta, puede acceder al cluster por terminal via SSH. Diríjase al apartado de [inicio de sesión](/guia_de_usuario/iniciar_sesion/) para conocer cómo realizar este proceso. 

## Enviar un Job

Dentro del cluster, usted puede enviar a ejecución sus trabajos o jobs a través de un [scheduler](https://es.wikipedia.org/wiki/Planificador). Una vez adquirida una cuenta, puede acceder al cluster por terminal via SSH. Diríjase al apartado de [enviar jobs](/guia_de_usuario/ejecutando_trabajos/enviar_jobs/) para conocer cómo realizar este proceso. 

## Linux



## Enviar Archivos

## Uso de Software

## Reglas de uso

## Obtener ayuda