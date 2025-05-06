[slurm]: https://slurm.schedmd.com/documentation.html
[politica-de-uso]:/info/politicas-de-uso/
[particiones]:/guia-de-usuario/enviar-jobs/particiones/
[comandos-basicos]:/guia-de-usuario/enviar-jobs/comandos-basicos/
[jobs-interactivos]:/guia-de-usuario/enviar-jobs/jobs-interactivos/

# Sobre el envío de Jobs

!!! warning "¡El nodo de acceso no es para realizar cómputos!"
      Use [Slurm][slurm] para enviar sus cargas de trabajo a los diferentes nodos de cómputo. 


Khipu es un recurso compartido por múltiples usuarios al mismo tiempo. Para asignar y gestionar los recursos de manera justa hacemos uso del gestor de trabajos [Slurm][slurm]. A través de [Slurm][slurm] los usuarios puede enviar, cancelar y revisar sus cargas de trabajo o *jobs* a los diferentes nodos de cómputo disponibles.

Los *jobs* pueden ser ejecutados de dos maneras distintas:

1. [**Modo *batch*:**][comandos-basicos] permite enviar un script que contiene todo lo necesario para la ejecución de su *job*. El cual se ejecutará de manera ininterrupida por el perido de tiempo asignado en el script y permitido a su tipo de cuenta. De esta manera usted manda a ejecutar su carga de trabajo y no tiene la necesitar de mantenerse conectado a Khipu para que continue su ejecución. La salida de su *job* será escrita de manera continua en un archivo, el cual usted puede consultar multiples veces para ver el avance de su trabajo hasta su finalización. 

2. [**Modo interactivo:**][jobs-interactivos] permite a los usuarios interactuar con su *job* en ejecución de manera directa a través de la línea de comandos. Este modo es similar a reservar recursos en una nodo, conectarte a él y luego de manera manual (interactiva) ejecutar cada unos de los comandos que requiera para completar su trabajo. Sin embargo, si su conexión con el nodo es interrupida o deja de tener actividad, su *job* será terminado. Este tipo de *jobs* es ideal para cargas de trabajo pequeñas, preparar o realizar pequeñas pruebas de la ejecución de jobs más largos o realizar labores de debugging. 

Los *jobs* que envíe a través de [Slurm][slurm] serán puestos en ejecución dependiendo de su tipo de cuenta y la cantidad de recursos solicitados (núcleos CPU, memoría RAM o  GPU). Generalmente, los *jobs* con pedidos de recursos más modestos y acotados suelen esperar menos tiempo en la fila de [Slurm][slurm]. Si desconoce la cantidad de recursos que su *job* demandará puede usar las [particiones][particiones] `debug` o `debug-gpu` para realizar pruebas antes de enviar sus jobs a otras [particiones][particiones] más demandadas. 


## Uso del nodo de acceso


El nodo de acceso es compartido por todos los usuarios y, como su nombre lo indica, debe ser usado para el acceso al cluster y la ejecución de tareas administrativas como:

- Compilación de código. 
- Descarga de archivos. El nodo de acceso es el único nodo con salida a internet.
- Creación de ambientes de trabajo (`virtualenv`) e instalación de paquetes de Python
- Envío y manejo de jobs 
- Transferencia de archivos 
- Pequeñas tareas de pre y post-procesamiento que no demanden de uso alto de recursos en CPU y RAM.

Todas las tareas que no se ajusten a las indicaciones dadas sobre el uso del nodo de acceso serán {--canceladas--} sin previo aviso y el usuario responsable será **sancionado** de acuerdo a nuestra [política de uso][politica-de-uso]. 

