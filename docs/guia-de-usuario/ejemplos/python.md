---
title: "Python"
weight: 37
---

Ejecución del programa en Python, `prueba_python.py`:

```python
from math import factorial as f

print("Hola mundo")
N = 100
print("%d! = %d" %(N, f(N)))
```

1. Crear el archivo ej5.sh:

```bash
#!/bin/bash
#SBATCH -J ej5 # nombre del job
#SBATCH -p debug # nombre de la particion 
#SBATCH -c 1  # numero de cpu cores a usar

module load python3/3.10.2 # carga el modulo de python version 3.10.2
python3 prueba_python.py # siendo prueba_python.py el nombre del programa python
module unload python3/3.10.2 
```

2. Enviar a ejecutar el job:

```shell
sbatch ej5.sh
```