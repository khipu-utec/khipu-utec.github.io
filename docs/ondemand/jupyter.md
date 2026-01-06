# Jupyter Lab

Khipu cuenta con instalaciones de [Jupyter](https://jupyter.org/) en todos sus nodos. Esto permite crear sesiones de [Jupyter Lab](https://jupyterlab.readthedocs.io/en/latest/) interactivas en el navegador.

![Sesión de Jupyter en la web](/assets/images/ood-jupyter.png)

## Creación de Sesiones

Para crear una sesión de Jupyter, el usuario debe ingresar a Open Ondemand con sus credenciales. Una vez en el panel principal, seleccionar la opción **Interactive Apps > Jupyter Notebook**.

![Opción de Jupyter en el Menú](/assets/images/ood-jupyter-menu.png)

La creación de sesiones de Jupyter funciona de manera similar a la creación de cualquier tarea en SLURM. Al crear una sesión de Jupyter, el usuario envía una tarea a la cola de ejecución de SLURM. Al igual que con otras tareas, debemos especificar parámetros para su ejecución. Los parámetros necesarios para crear una sesión de Jupyter son:

- Partición
- QOS (tipo de cuenta a utilizar)
- Número de Horas
- Número de CPUs (cores)
- Cantidad de Memoria (GB)
- Cantidad de Shards de GPU (sólo para particiones `debug-gpu`, `gpu`, `all`)

## Ambientes Virtuales

Jupyter permite ejecutar *notebooks* con diferentes *kernels*. Una práctica común es utilizar ambientes virtuales (`venv`) de Python para separar los *kernels* y las dependencias de software que tienen instaladas. En nuestra instalación de Jupyter, los usuarios tienen acceso a ambientes virtuales globales (compartidos) y personales (privados).

### Ambientes virtuales globales

Con el fin de proveer paquetes de software comúnmente utilizados para distintos tipos de aplicaciones (HPC, Data Science, IA, etc), Khipu cuenta con ambientes virtuales globales disponibles para todos los usuarios. En la siguiente tabla se presentan los ambientes globales disponibles con los paquetes de software que se encuentran instalados en cada uno:

| **Ambiente Virtual** | **Descripción**                                                                        | **Paquetes de Software**                                                                                                                                                                                                                                   |
|----------------------|----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| General HPC          | Herramientas básicas para uso general en HPC, Ciencia de datos, GPU e Inteligencia Artificial | IPykernel v7.1.0, Numpy v2.3.5, Pandas v2.3.3, MatplotLib v3.10.8, Seaborn v0.13.2, Awkward v2.8.11, Dask v2025.12.0, CuPy (CUDA 12x) v13.6.0, Scipy v1.16.3, Scikit-Learn v1.8.0, Keras v3.12.0, PyTorch v2.9.1, TorchVision v0.24.1, TensorFlow v2.20.0  |

### Ambientes virtuales personales

Los usuarios de Khipu son libres de instalar ambientes virtuales personales en sus entornos. De esta manera pueden instalar los paquetes de software que necesiten y utilizarlos en Jupyter. Los comandos para crear un ambiente virtual e instalar su *kernel* de Jupyter en su entorno son los siguientes:

```shell
ml python3/3.13.2
python3 -m venv <my-env>
```

!!! warning "Importante"
	Antes de crear el ambiente,** es necesario cargar el módulo `python3/3.13.2` y es importante utilizar esta versión para crearlo, ya que este módulo carga la versión de Python específica que utiliza nuestra instalación de Jupyter.** Si un usuario crea un ambiente virtual en base a otra versión de Python, es probable que el *kernel* aparezca como disponible en Jupyter Lab, pero que no pueda inicializarse correctamente. 

Para instalar el *kernel* de un ambiente en el entorno del usuario y habilitar su uso en sesiones de Jupyter, es necesario ejecutar el siguiente comando:

```shell
source <path-to-my-env>/bin/activate
python -m ipykernel install --user --name <my-env> --display-name "Test Env"
```

Una vez instalado el *kernel*, este nos aparecerá como opción al seleccionar un *kernel* en las sesiones de Jupyter.

![Ambientes virtuales en sesión](/assets/images/ood-venv-session.png)

!!! note "Instalación de paquetes de software"
	Una vez creado su ambiente, el usuario puede instalar paquetes de software utilizando `pip`. Una vez instalado el *kernel* en su entorno, este se encontrará disponible en Jupyter. El usuario puede instalar paquetes de software en su ambiente virtual antes o después de que instaló su *kernel*.

Para más información sobre el manejo de ambientes virtuales, ver: [https://docs.python.org/3/library/venv.html](https://docs.python.org/3/library/venv.html)