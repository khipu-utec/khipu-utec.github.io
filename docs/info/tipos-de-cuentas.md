[slurm]: https://slurm.schedmd.com/documentation.html

Existen diferentes grupos de usuarios que son gerenciados de manera automática por [Slurm][slurm]. De manera general podemos agruparlos en: educación e investigación.

## Grupo Educación

Permite el uso del cluster por los estudiantes de pregrado o posgrado, a pedido de un instructor(a) registrado(a), en el marco del desarrollo de un curso. De acuerdo al tipo de estudiantes (pregrado o posgrado) se disponen los siguiente límites:

### Pregrado

- **Particiones disponibles**: debug, debug-gpu, standard, gpu
- **Cantidad máxima de recursos por usuario**: 12 cores, 36GB RAM y 4 shards GPU
- **Cantidad máxima de recursos por job**: 1 shard GPU
- **Cantidad máxima de jobs enviados:** 10
- **Tiempo máximo de ejecución por job (Walltime):** 30 minutos

### Posgrado

- **Particiones disponibles**: debug, debug-gpu, standard, gpu
- **Cantidad máxima de recursos por usuario**: 24 cores, 78GB RAM y 8 shards GPU
- **Cantidad máxima de recursos por job**: 2 shards GPU
- **Cantidad máxima de jobs enviados:** 10
- **Tiempo máximo de ejecución por job (Walltime):** 2 horas

### Docencia

- **Particiones disponibles**: debug, debug-gpu, standard, gpu
- **Cantidad máxima de recursos por usuario**: 24 cores, 78GB RAM y 8 shards GPU
- **Cantidad máxima de recursos por job**: 2 shards GPU
- **Cantidad máxima de jobs enviados:** 10
- **Tiempo máximo de ejecución por job (Walltime):** 2 horas

## Grupo Investigación

Permite el uso del cluster para todos aquellos miembros de UTEC y asociados que requieran de recursos computacionales para el desarrollo de un proyecto de investigación. La solicitud de acceso es registrada por el investigador principal (PI). De acuerdo al proyecto se disponen los siguiente niveles y límites por nivel:

#### Tesis

- **Particiones disponibles**: debug, debug-gpu, standard, gpu, big-mem
- **Cantidad máxima de recursos por usuario**: 48 cores, 96GB RAM y 8 shards GPU
- **Cantidad máxima de recursos por job**: 4 shards GPU
- **Cantidad máxima de jobs en ejecución:** 2
- **Tiempo máximo de ejecución por job (Walltime):** 1 día


#### Investigación Nivel I

- **Particiones disponibles**: debug, debug-gpu, standard, gpu, big-mem
- **Cantidad máxima de recursos por usuario**: 64 cores, 128GB RAM y 8 shards GPU
- **Cantidad máxima de recursos por job**: 8 shards GPU
- **Cantidad máxima de jobs en ejecución:** 3
- **Tiempo máximo de ejecución por job (Walltime):** 3 días

#### Investigación Nivel II

- **Particiones disponibles**: debug, debug-gpu, standard, gpu, big-mem
- **Cantidad máxima de recursos por usuario**: 64 cores, 256GB RAM y 16 shards GPU
- **Cantidad máxima de recursos por job**: 16 shards GPU
- **Cantidad máxima de jobs en ejecución:** 4
- **Tiempo máximo de ejecución por job (Walltime):** 5 días
