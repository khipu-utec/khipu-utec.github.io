---
level: restricted
---
# Jupyter Lab / Notebook [conda]

To run Jupyter Lab/Notebook in Khipu you have the following options:

- Using an interactive session
- Using a batch job

In both cases you will use your browser to interact with your jupyter session.

## Requirements

You can use the already created miniconda environment or create your own using pip or miniconda.  If you want to create your own, run:

```bash
# Load conda module
(master)$ ml load miniconda

# Create and activate the conda environment
(master)$ conda create -n <your-env-name>
(master)$ conda activate <your-env-name>

# Jupyter installation
(master)$ conda install jupyterlab # or
(master)$ conda install jupyter
```

## Interactive session

1. **Start an interactive session in Slurm.** You must select the partition depending on the resources you will use. Its recommended to start with `debug` or `debug-gpu` partitions.

```bash
# Create an interactive session from the master node
(master)$ srun --pty --mem=4G -p debug bash

# Change to the directory you want to use , for example my-project-dir
(computer)$ cd my-project-dir

# Execute jupyter in your conda environment in the compute node
(computer)$ ml load miniconda
(computer)$ conda activate <your-env-name>

(computer)$ ijup_port=$(shuf -i8000-9999 -n1)
(computer)$ ijup_ip=$(hostname -i)

(computer)$ echo "I will run jupyter on $ijup_ip:$ijup_port"
(computer)$ jupyter notebook --no-browser --port=$ijup_port --ip=$ijup_ip

# If you are using jupyter lab
(computer)$ jupyter lab --no-browser --port=$ijup_port --ip=$ijup_ip
```

---

- Remember: job on the `debug` and `debug-gpu`partitions have a timelimit of 30 minutes. If you need more time, you can use the other available partitions.
    
    You will have an ouput similar to:
    

```bash
[W 2025-03-07 11:49:16.903 ServerApp] A `_jupyter_server_extension_points` function was not found in jupyter_lsp. Instead, a `_jupyter_server_extension_paths` function was found and will be used for now. This function name will be deprecated in future releases of Jupyter Server.                                   
[W 2025-03-07 11:49:17.096 ServerApp] A `_jupyter_server_extension_points` function was not found in notebook_shim. Instead, a `_jupyter_server_extension_paths` function was found and will be used for now. This function name will be deprecated in future releases of Jupyter Server.                                 
[I 2025-03-07 11:49:17.097 ServerApp] jupyter_lsp | extension was successfully linked.                  
[I 2025-03-07 11:49:17.100 ServerApp] jupyter_server_terminals | extension was successfully linked.     
[I 2025-03-07 11:49:17.104 ServerApp] jupyterlab | extension was successfully linked.                   
[I 2025-03-07 11:49:17.108 ServerApp] notebook | extension was successfully linked.                     
[I 2025-03-07 11:49:17.110 ServerApp] Writing Jupyter server cookie secret to /home/abcd/.local/share/jupyter/runtime/jupyter_cookie_secret
[I 2025-03-07 11:49:17.641 ServerApp] notebook_shim | extension was successfully linked.                
[I 2025-03-07 11:49:17.677 ServerApp] notebook_shim | extension was successfully loaded.                
[I 2025-03-07 11:49:17.679 ServerApp] jupyter_lsp | extension was successfully loaded.                  
[I 2025-03-07 11:49:17.680 ServerApp] jupyter_server_terminals | extension was successfully loaded.     
[I 2025-03-07 11:49:17.691 LabApp] JupyterLab extension loaded from /home/abcd/.conda/envs/jupyter/lib/python3.12/site-packages/jupyterlab
[I 2025-03-07 11:49:17.691 LabApp] JupyterLab application directory is /home/abcd/.conda/envs/jupyter/share/jupyter/lab
[I 2025-03-07 11:49:17.692 LabApp] Extension Manager is 'pypi'.                                         
[I 2025-03-07 11:49:17.722 ServerApp] jupyterlab | extension was successfully loaded.                   
[I 2025-03-07 11:49:17.727 ServerApp] notebook | extension was successfully loaded.                     
[I 2025-03-07 11:49:17.727 ServerApp] Serving notebooks from local directory: /home/abcd             
[I 2025-03-07 11:49:17.727 ServerApp] Jupyter Server 2.15.0 is running at:                              
[I 2025-03-07 11:49:17.727 ServerApp] http://192.168.152.3:8226/tree?token=abcdabcdabcdabcdabcd
```

1. **Setup an ssh tunnel to Khipu.** As you might already notice, if you copy an paste on your browser `$ijup_ip:$ijup_port` it won’t work because you can not access this direction on your local network. However, there is way to redirect the service running on this remote ip:port to your local machine in a local ip:port. This is possible with the use of SSH tunnels. To do so, you need to run the following command in your local machine:

```bash
 (local)$ ssh -N -L $ijup_port:$ijup_ip:$ijup_port <your-username>@khipu.utec.edu.pe
```

It will ask for your password (if you are not using ssh-keys). After you enter it, the tunnel has been connected.

If you are using windows, you need to use Putty to create the SSH tunnel.  

1. **Open your browser to access your jupyter session.** Now, you just need to open the following address on your browser `localhost:$ijup_port`. The notebook will ask you for a token which can be found on the ouput of Step 1.

![image.png](Jupyter%20Lab%20Notebook%20%5Bconda%5D%201af30cfc329480e8be7ff1b555064629/image.png)

**Remember**: the installation of packages or the download of files must be done on the master node. Compute nodes don’t have access to internet. In order to do so, you can have an additional SSH session on the master node to do those things that requires internet access.

1. **Kill your Jupyter server.** Once you have finish, you can kill your sessión by pressing `cntrl+c` twice. 

## Batch session

1. **Submit an Slurm batch script.** The same steps aforementioned can be summarized to the following script `jupyter.slurm`.

```bash
#!/bin/bash

## Slurm Directives
#SBATCH --job-name jupyter-server
#SBATCH --output jupyter-server-%J.out
#SBATCH --mem=4G
#SBATCH -p debug

## Load modules
ml load miniconda
conda activate <your-environment>

## Place to your working directory, for example $HOME
cd $HOME

## Execute your jupyter server

ijup_port=$(shuf -i8000-9999 -n1)
ijup_ip=$(hostname -i)

echo -e "
    Copy/Paste this in your local terminal to ssh tunnel with remote
    -----------------------------------------------------------------
    ssh -N -L $ijup_port:$ijup_ip:$ijup_port $USER@khipu.utec.edu.pe
    -----------------------------------------------------------------
    Then open a browser on your local machine to the following address
    ------------------------------------------------------------------
    localhost:$ijup_port 
    ------------------------------------------------------------------
    "
# If you are using jupyter notebook
jupyter notebook --no-browser --port=$ijup_port --ip=$ijup_ip 

# If you are using jupyter lab
jupyter lab --no-browser --port=$ijup_port --ip=$ijup_ip
```

- Remember to edit the above script with your own information before submit it.

Then, to submit this script just run the classical `sbatch jupyter.slurm`. Once your batch job is running (you can check its status with `squeue`), you will have a file named `jupyter-server-<job-id>.txt` containing information you will need to create the SSH tunnel. Here is sample output:

```bash
(master)$ cat jupyter-server-4694.txt

Copy/Paste this in your local terminal to ssh tunnel with remote
-----------------------------------------------------------------
ssh -N -L 8312:192.168.152.3:8312 <your-username>@khipu.utec.edu.pe
-----------------------------------------------------------------
Then open a browser on your local machine to the following address
------------------------------------------------------------------
localhost:8312
------------------------------------------------------------------
[W 2025-03-07 12:38:25.145 ServerApp] A `_jupyter_server_extension_points` function was not found in jupy
ter_lsp. Instead, a `_jupyter_server_extension_paths` function was found and will be used for now. This f
unction name will be deprecated in future releases of Jupyter Server.
[W 2025-03-07 12:38:25.287 ServerApp] A `_jupyter_server_extension_points` function was not found in note
book_shim. Instead, a `_jupyter_server_extension_paths` function was found and will be used for now. This
 function name will be deprecated in future releases of Jupyter Server.
....
```

1. **Setup an ssh tunnel to Khipu.** Similar to the interactive session tutorial, copy and paste the line containing the SSH tunnel command (`ssh -N -L ..`) in the `jupyter-server-<job-id>.txt` file. Using the sample output, you will run:

```bash
ssh -N -L 8312:192.168.152.3:8312 <your-username>@khipu.utec.edu.pe
```

Enter your password (if it is asked) and that all, your SSH tunnel is ready.

1. **Open your browser to access your jupyter session.** In a similar way, use the `jupyter-server-<job-id>.txt` file and find your jupyter server url.  For example [`localhost:8312`](http://localhost:8312) in the sample output. The jupyter token is also in this file, i will look like `http://127.0.0.1:$ijup_port/tree?token=TOKEN`. Open this address on your browser, enter your jupyter token and start working with your jupyter server.
2. Stop the jupyter server. Once your are finish working, do not forget to stop your jupyter server with`scancel <job-id>`.

References:

- https://docs.ccv.brown.edu/oscar/jupyter-notebooks/jupyter-notebooks-on-oscar-1