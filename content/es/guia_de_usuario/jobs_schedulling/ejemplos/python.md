---
title: "Python"
date: 2022-05-25T19:46:06-05:00
weight: 43
---

Ejecución del programa en Python, prueba_python.py:

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
#SBATCH -p investigacion # nombre de la particion 
#SBATCH -c 1  # numero de cpu cores a usar

module load python/2.7.17 # carga el modulo de python version 2.7.17
python2.7 prueba_python.py # siendo prueba_python.py el nombre del programa python
module unload python/2.7.17 
```

2. Enviar a ejecutar el job:

```shell
sbatch ej5.sh
```