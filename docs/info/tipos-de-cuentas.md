[slurm]: https://slurm.schedmd.com/documentation.html

Existen diferentes grupos de usuarios que son gerenciados de manera automática por [Slurm][slurm]. De manera general podemos agruparlos en: educación e investigación.

## Grupo Educación

Permite el uso del cluster por los estudiantes de pregrado o posgrado, a pedido de un instructor(a) registrado(a), en el marco del desarrollo de un curso. De acuerdo al tipo de estudiantes (pregrado o posgrado) se disponen los siguiente límites:

### Pregrado

- **Particiones disponibles**: debug, debug-gpu, standard, gpu
- **Cantidad máxima de recursos por usuario**: 32 cores, 98GB RAM y 8 shards GPU
- **Tiempo máximo de ejecución por job (Walltime):** 8 horas

### Posgrado

- **Particiones disponibles**: debug, debug-gpu, standard, gpu
- **Cantidad máxima de recursos por usuario**: 32 cores, 98GB RAM y 8 shards GPU
- **Tiempo máximo de ejecución por job (Walltime):** 8 horas

### Docencia

- **Particiones disponibles**: debug, debug-gpu, standard, gpu
- **Cantidad máxima de recursos por usuario**: 32 cores, 98GB RAM y 32 shards GPU
- **Tiempo máximo de ejecución por job (Walltime):** 8 horas

## Grupo Investigación

Permite el uso del cluster para todos aquellos miembros de UTEC y asociados que requieran de recursos computacionales para el desarrollo de un proyecto de investigación. La solicitud de acceso es registrada por el investigador principal (PI). De acuerdo al proyecto se disponen los siguiente niveles y límites por nivel:

#### Tesis

- **Particiones disponibles**: debug, debug-gpu, standard, gpu, big-mem
- **Cantidad máxima de recursos por usuario**: 32 cores, 96GB RAM y 40 shards GPU
- **Tiempo máximo de ejecución por job (Walltime):** 24 horas


#### Investigación Nivel I

- **Particiones disponibles**: debug, debug-gpu, standard, gpu, big-mem
- **Cantidad máxima de recursos por usuario**: 48 cores, 120GB RAM y 40 shards GPU
- **Tiempo máximo de ejecución por job (Walltime):** 48 horas


#### Investigación Nivel II

- **Particiones disponibles**: debug, debug-gpu, standard, gpu, big-mem
- **Cantidad máxima de recursos por usuario**: 96 cores, 162GB RAM y 80 shards GPU
- **Tiempo máximo de ejecución por job (Walltime):** 96 horas


## ¿Cómo puedo saber mi tipo de cuenta?

Una vez inicies sesión en Khipu, puedes ejecutar el commando:

```
myaccount
```
Obtendrás el nombre de tu cuenta y sus límites. La salida en pantalla será similar a:

```
$ myaccount
---------------------------------------------------------
Khipu account                                            
---------------------------------------------------------
User                 Account            Default          
-------------------- ------------------ -----------------
aturin               docencia           docencia           

--------------------------------------------------------------
Account Limits                                                
--------------------------------------------------------------
Account           Resources                       TimeLimit   
----------------- ------------------------------- ------------
a-docencia        cpu=32,gres/shard=32,mem=98G      08:00:00 
```