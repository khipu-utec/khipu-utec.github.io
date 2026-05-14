---
title: "Nodos"
weight: 31
---

Los nodos están configurados en SLURM de modo que todos los recursos (memoria, CPUs, GPUs) se encuentren disponibles para reservar al enviar tareas.

Las configuraciones de cada nodo se encuentran listadas a continuación:

| Nodo | Configuración |
| :------------------:  | ---------- |
| n003 | **CPUs:** 64<br>**RealMemory:** 225316MiB |
| n004 | **CPUs:** 64<br>**RealMemory:** 225316MiB |
| n005 | **CPUs:** 64<br>**RealMemory:** 193059MiB |
| n006 | **CPUs:** 96<br>**RealMemory:** 1031783MiB |
| g001 | **Gres:**<br>- **gpu:** tesla:1,shard:16<br>**CPUs:** 64<br>**RealMemory:** 192052MiB  |
| g002 | **Gres:**<br>- **gpu:** rtxa6000:1,shard:48<br>**CPUs:** 96<br>**RealMemory:** 1031783MiB  |
| ag001 | **Gres:**<br>- **gpu:** a100:1<br>- **gpu:** a100_3g.20gb:1<br>- **gpu:** a100_2g.10gb:1<br>- **gpu:** a100_1g.5gb:2<br>- shard:80<br>**CPUs:** 128<br>**RealMemory:** 1031900MiB  |
| ds001 | **Gres:**<br>- **gpu:** rtxa6000:1,shard:48<br>**CPUs:** 96<br>**RealMemory:** 1031783MiB  |

Donde Gres representa recursos genéricos (**G**eneric **RES**ources).

## Particiones del Nodo `ag001`
El nodo `ag001` cuenta con dos GPUs NVIDIA A100 PCIe-40GB. Estos GPUs son los únicos en Khipu que soportan la funcionalidad *Multi Instance GPU* (MIG), que permite dividir un GPU en sub-instancias aisladas, con múltiples configuraciones posibles.

La distribución de GPUs es la siguiente:

- Una de las GPUs se mantiene sin modificar
- Una de las GPUs se utiliza para proporcionar 4 instancias MIG:
	- 1 instancia de tipo `3g.20gb`
	- 1 instancia de tipo `2g.10gb`
	- 2 instancias de tipo `1g.5gb`

Los nombres de cada tipo de instancia indican sus capacidades. Por ejemplo, las instancias de nombre `3g.20gb` cuentan con 3/7 de la capacidad de procesamiento del GPU, y con 20GiB de VRAM. Estas instancias son detectadas por SLURM como GPUs independientes, y se pueden solicitar con normalidad. La configuración se encuentra ilustrada en la siguiente imagen:

![Perfil de Configuración de NVIDIA MIG en GPU A100](/assets/images/mig-profile.png){ loading=lazy }


!!! note
	Dado que los GPUs del nodo `ag001` fueron redistribuidos para contar con más instancias de distintas capacidades, se sugiere no utilizar *shards* cuando trabajemos con este nodo, dado que de momento estas no se encuentran distribuidas equitativamente. En lugar de eso, podemos reservar instancias más pequeñas según los recursos que necesitamos.

## Envío de tareas con GPUs específicos

Si, por ejemplo, necesitamos utilizar un GPU específico, podemos especificarlo al enviar la tarea con `sbatch`. El siguiente ejemplo reserva dos instancias de `1g.5gb` del nodo `ag001`:

```bash
#!/bin/bash
#SBATCH --job-name=mi_trabajo    # Nombre del trabajo
#SBATCH --partition=gpu          # Partición a usar
#SBATCH --nodelist=ag001         # Nodos a utilizar
#SBATCH --gres=gpu:a100_1g.5gb:2 # GRES a utilizar
#SBATCH ...

# Continuar con el envío de la tarea
module load ...
```

Cabe resaltar que no podemos reservar un GPU que no está disponible en un nodo, por lo que es mejor especificar el nodo que deseamos reservar en caso querramos utilizar un GPU en específico.