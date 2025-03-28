# Reinicio de jobs con PyTorch

El cluster Khipu es una herramienta valiosa cuyos recursos son compartidos y limitados. Lamentablemente no se puede ofrecer recursos infinitos siempre, ya que limitaría su disponibilidad para todos. Ante esto el cluster mantiene una política de restricciones en el tiempo de ejecución basada en ciclos. 

Un ciclo de ejecución es la cantidad máxima de tiempo que un trabajo o job puede ser ejecutado de manera ininterrumpida en el cluster. Una vez su ciclo de ejecución llega al tiempo máximo, su trabajo será terminado por el gestor de colas. ¿Pero qué pasa cuando su trabajo requiere un tiempo mayor al que se disponible en un ciclo de ejecución? Pues, puede usar varios ciclos de ejecución para poder completarlo. De esta manera, si el cluster se encuentra libre, usted podrá obtener todos los ciclos que necesite inmediamente despues de su anterior. Y si el cluster tiene demanda, una vez acabado su anterior ciclo, su trabajo será encolado a la espera de que se liberen los recursos solicitados para poder ejecutar un nuevo ciclo de ejecución. Y así su trabajo podrá usar todos los ciclos que necesite para poder completarse.

Ahora que se ha explicado como funcionan los ciclos de ejecución en Khipu es importante mostrarlo con un ejemplo ¿no? En esta guía se va mostrar como podemos entrenar un modelo de PyTorch usando varios ciclos de ejecución en el cluster. Este ejemplo, aunque sencillo, puede ser luego adaptado y extendido para sus diferentes casos de uso.

## Prerequisitos

- Tener una cuenta de Khipu activa
- Cargar los módulos de `Python3`
    ```bash
    ml load python3
    # Crear un entorno virtual
    python3 -m venv venv
    source venv/bin/activate
    pip install torch
    ```

## Paso a paso

Para el siguiente tutorial vamos a usar el siguiendo modelo en PyTorch:

```py linenums="1"
import torch
from torch import nn # nn contains all of PyTorch's building blocks for neural networks

#########################
## Inicialización de cuda
#########################


print(f"PyTorch version: {torch.__version__}")
device = "cuda" if torch.cuda.is_available() else "cpu"
print(f"Usando como device: {device}")

# Función utilitaria para configurar los seed en GPU o CPU
def set_seed(seed):
    torch.manual_seed(seed)
    if device == "cuda": torch.cuda.manual_seed(seed)    

#########################
## Creación del dataset: creación de un conjunto de valores para la función f con adición de ruido
#########################


def f(x): return 7*x*x - x + 2
print("\nFunción original\t\t : f(x) = 7x^2 - x + 2")

set_seed(42)

X_all = torch.arange(-3.0, 3.0, 0.01, dtype=torch.float64,device="cpu").unsqueeze(dim=1)
y_all_clean = f(X_all)
y_all = y_all_clean + (torch.rand(size=X_all.shape, device="cpu")*4.0 - 2.0) 

# Envio de los datos al device
X_all = X_all.to(device)
y_all_clean = y_all_clean.to(device)
y_all = y_all.to(device)

#############################
## Separar datos en training, testing and validation data
############################

def sample_values_from_dataset(X, y, n_samples, random_seed=1969):
    set_seed(random_seed)
    # retrieve index to further use in the complete dataset
    random_indices = torch.randperm(n=X_all.shape[0], device="cpu")
    random_indices = random_indices.to(device)
    chosen_indices = random_indices[:n_samples]
    return X[chosen_indices,:], y[chosen_indices,:]

# Obtener una muestra aleatoria de 16 números
X, y = sample_values_from_dataset(X_all, y_all, 16)

X_train, y_train = X[0:8], y[0:8]
X_val, y_val = X[8:12], y[8:12]
X_test, y_test = X[12:16], y[12:16]

print(f"Forma del set de entrenamiento\t : X = {X_train.shape}, y = {y_train.shape}")
print(f"Forma del set de validación\t : X = {X_val.shape}, y = {y_val.shape}")
print(f"Forma del set de testeo\t\t : X = {X_test.shape}, y = {y_test.shape}")


#################################
## Creación del modelo
#################################

class PolynomialRegressionModel(nn.Module):
    def __init__(self, polynomial_degree):
        super().__init__()
        self.d = polynomial_degree
        
        self.coefficients = nn.ParameterList([nn.Parameter(torch.randn(1, dtype=torch.float64)) for i in range(self.d+1)])
    
    def forward(self, X):
        result = torch.zeros(size=(X.shape[0],1), device=X.device)
        for i in range(self.d+1):
            result = result + (torch.pow(X,i) * self.coefficients[i])
        return result

    def __str__(self):
        equation = f"f(x) = {self.coefficients[self.d].item():.4f} * x^{self.d}"
        for i in range(self.d-1, -1, -1):
            equation = equation + f" + {self.coefficients[i].item():.4f} * x^{i}"
        return equation

# Lets set the random number generator seed to ensure we always generate the same model whenever we re-execute this code block.
set_seed(1969)

my_model = PolynomialRegressionModel(polynomial_degree=2).to(device)

print("\n-- Mi modelo lineal --")
print("La función de mi modelo:", my_model)


##########################################
## Entrenamiento de mi modelo
#########################################

import os
import time

EPOCH_SAVE_PATH = "last_completed_epoch.txt"
MODEL_SAVE_PATH = "last_model_state.pth"

# función utilitaria para recuperar la última epoca ejecutada
def get_last_completed_epoch():
    if os.path.exists(EPOCH_SAVE_PATH):
        with open(EPOCH_SAVE_PATH, "r") as file:
            last_completed_epoch = int(file.read())
            file.close()
    else:
        last_completed_epoch = -1
    return last_completed_epoch


def train_model_v1(model, 
                   X_train, y_train, 
                   X_val, y_val,
                   learning_rate = 0.01,
                   number_of_epochs = 10,
                   verbosity_skip_level = 1):
    loss_fn = nn.L1Loss() 
    optimizer = torch.optim.SGD(params=model.parameters(), lr=learning_rate)

    curr_epoch = 0
    while curr_epoch < number_of_epochs:

        # Verifico si ejecuté previamente mi modelo
        last_epoch_completed = get_last_completed_epoch()
        if last_epoch_completed >= curr_epoch:
            print(f"Restaurando el último estado del modelo en la época: {last_epoch_completed}")
            # continuar con la siguiente epoca y restauro estado
            curr_epoch = last_epoch_completed + 1
            model = torch.load(f=MODEL_SAVE_PATH, weights_only=False)

        # Entrenamiento
        model.train()
        y_hat = model(X_train)
        loss = loss_fn(y_hat, y_train)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        # Validación
        model.eval()
        with torch.inference_mode():
            val_hat = model(X_val)
            val_loss = loss_fn(val_hat, y_val)
            if (verbosity_skip_level > 0) and (curr_epoch % verbosity_skip_level == 0):
                print(f"Epoca: {curr_epoch} | MAE Train Loss: {loss} | MAE Validation Loss: {val_loss} ")


        # Guardamos el estado del modelo
        torch.save(model, f=MODEL_SAVE_PATH) 
        with open(EPOCH_SAVE_PATH, "w") as file:
            file.write(str(curr_epoch))
            file.close()
        
        curr_epoch += 1
        time.sleep(0.01)

#####################################
## Ejecución
#####################################

print("\n** Entrenando mi modelo hasta 1000 epocas **")
train_model_v1(my_model, X_train, y_train, X_val, y_val,
               learning_rate = 0.01, number_of_epochs = 1000,
               verbosity_skip_level = 100)

print("\n La función de mi modelo final:", my_model)
# Eliminando archivo de epocas
if os.path.exists(EPOCH_SAVE_PATH):
  os.remove(EPOCH_SAVE_PATH)
```

El siguiente job script incluye el parámetro `--signal` el cual se usa para enviar una señal 30 segundos antes de quel job se termine por falta de tiempo. Cuando esto ocurre se captura la señal y se añade un handler propio para ella. En este handler se guarda el output actual en otro archivo y se envía una solicitud de **requeue**. Un job requeue permite volver a enviar a ejecución el job actual. De esta manera, se solicita un nuevo ciclo de ejecución antes de que termine el actual. Para evitar que las solicitudes de requeue no tengan fin, se establece un parámetro que establece la cantidad máxima de reinicios. 

```sh linenums="1"
#!/bin/bash

## Slurm Directives
#SBATCH --job-name sample-pytorch
#SBATCH --output sample-pytorch-%J.out
#SBATCH --error sample-pytorch-%J.err
#SBATCH -t 00:01:00
#SBATCH -p debug-gpu
#SBATCH --signal=B:SIGTERM@30

export PYTHONUNBUFFERED=TRUE
##############################################################
##  Gather some information from the job and setting limits ##

max_restarts=4      # tweak this number to fit your needs
scontext=$(scontrol show job ${SLURM_JOB_ID})
restarts=$(echo ${scontext} | grep -o 'Restarts=[0-9]*****' | cut -d= -f2)
outfile=sample-pytorch-${SLURM_JOB_ID}.out

##                                                          ##
##############################################################
##  Build a term-handler function to be executed            ##
##      when the job gets the SIGTERM                       ##

term_handler()
{
    echo "Executing term handler at $(date)"
    if [[ $restarts -lt $max_restarts ]];then
        # Copy the log file because it will be overwriten
        cp -v "${outfile}" "${outfile}.${restarts}"
        scontrol requeue ${SLURM_JOB_ID}
        exit 0
    else
        echo "Your job is over the Maximun restarts limit"
        exit 1
    fi
}

## Call the function when the jobs recieves the SIGTERM     ##
trap 'term_handler' SIGTERM

# print some job-information
cat <<EOF
SLURM_JOB_ID:         $SLURM_JOB_ID
SLURM_JOB_NAME:       $SLURM_JOB_NAME
SLURM_JOB_PARTITION:  $SLURM_JOB_PARTITION
SLURM_SUBMIT_HOST:    $SLURM_SUBMIT_HOST
Restarts:             $restarts
EOF


##                                                          ##
##############################################################
##          Here begins your actual program                 ##

##  

## Place to your working directory, for example $HOME
cd ~/my-model-dir

## Load modules
ml load python3
source venv/bin/activate

srun python3 my_model.py
```

!!! note
    Si por la naturaleza de su trabajo no puede adaptarlo para su ejecución en ciclos, es posible aumentarle su tiempo límite de ejecución. Sin embargo, estos casos deberían ser la excepción y no la regla.