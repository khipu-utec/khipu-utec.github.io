# Miniconda

En Khipu, se puede emplear el gestor de paquetes `conda` a través del módulo `Miniconda`. Para ello será necesario necesario ejecutar los siguiente pasos:

## Creación de ambiente de trabajo

- Desde el nodo de acceso, cargaremos el modulo de Miniconda.

    ```shell
    module load miniconda/3.0
    ```
- Crearemos el ambiente de conda sobre el cual trabajaremos.
  
    ```shell
    conda create --name my-env
    ```

- Si deseamaos crear un ambiente y a la vez instalar paquetes.

    ```
    conda create --name my-env pytorch torchvision
    ```

## Creación de batch script

En el script que creemos, deberemos especificar la cantidad de recursos que necesitamos. Luego deberemos cargar el módulo de **Miniconda** y activar nuestro ambiente de trabajo. Finalmente, escribiremos el comando necesario para ejecutar nuestro programa.

```bash
#!/bin/bash
#SBATCH --job-name=app-test      # nombre del job
#SBATCH --nodes=1                # cantidad de nodos
#SBATCH --ntasks=1               # cantidad de tareas
#SBATCH --cpus-per-task=1        # cpu-cores por task 
#SBATCH --mem=4G                 # memoria total por nodo
#SBATCH --gres=gpu:1             # numero de gpus por nodo
#SBATCH --time=00:05:00          # limite total de ejecucion

module purge
module load miniconda/3.0
conda activate my-env

python app-test.py

conda deactivate
```

Si al momento de enviar su job obtienen el siguiente mensaje. 

```text
CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run
    $ conda init <SHELL_NAME>
Currently supported shells are:
  - bash
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell
See 'conda init --help' for more information and options.
IMPORTANT: You may need to close and restart your shell after running 'conda init'.
```

Deberan adicionar  `eval "$(conda shell.bash hook)"` a su batch script luego de haber cargado el modulo de miniconda. Por lo tanto, el batch script resultante será el siguiente. 

```bash
#!/bin/bash
#SBATCH --job-name=app-test      # nombre del job
#SBATCH --nodes=1                # cantidad de nodos
#SBATCH --ntasks=1               # cantidad de tareas
#SBATCH --cpus-per-task=1        # cpu-cores por task 
#SBATCH --mem=4G                 # memoria total por nodo
#SBATCH --gres=gpu:1             # numero de gpus por nodo
#SBATCH --time=00:05:00          # limite total de ejecucion

module purge
module load miniconda/3.0
eval "$(conda shell.bash hook)"
conda activate my-env

python app-test.py

conda deactivate
```