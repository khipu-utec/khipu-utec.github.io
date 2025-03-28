# Apptainer

Apptainer (formerly Singularity)  is an open-source, secure, portable and easy-to-use container platform that allows users to create and run containers that package up pieces of software in a way that is portable and reproducible. You can build a container for Apptainer on your laptop, and then run it on a different PC, workstation, HPC cluster, cloud server, etc. Apptainer allows unprivileged users to use containers and prohibits privilege escalation within the container; users are the same inside and outside the container. Apptainer can import any container from OCI (Open Containers Initiative) registries. It means you can pull, run, and build from most containers on Docker Hub without changes. More information about Apptainer can be found on their [official website](https://apptainer.org/).

## Overview of the Apptainer Interface

Apptainer commands can be executed natively on the master or any compute node, without the need to load any additional module

The `help` command gives an overview of Apptainer options and subcommands as follows:

```bash
$ apptainer help

Linux container platform optimized for High Performance Computing (HPC) and
Enterprise Performance Computing (EPC)

Usage:
  apptainer [global options...]

Description:
  Apptainer containers provide an application virtualization layer enabling
  mobility of compute via both application and environment portability. With
  Apptainer one is capable of building a root file system that runs on any
  other Linux system where Apptainer is installed.

Options:
      --build-config    use configuration needed for building containers
  -c, --config string   specify a configuration file (for root or
                        unprivileged installation only) (default
                        "/etc/apptainer/apptainer.conf")
  -d, --debug           print debugging information (highest verbosity)
  -h, --help            help for apptainer
      --nocolor         print without color output (default False)
  -q, --quiet           suppress normal output
  -s, --silent          only print errors
  -v, --verbose         print additional information
      --version         version for apptainer

Available Commands:
  build       Build an Apptainer image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  checkpoint  Manage container checkpoint state (experimental)
  completion  Generate the autocompletion script for the specified shell
  config      Manage various apptainer configuration (root user only)
  delete      Deletes requested image from the library
  exec        Run a command within a container
  help        Help about any command
  inspect     Show metadata for an image
  instance    Manage containers running as services
  key         Manage OpenPGP keys
  keyserver   Manage apptainer keyservers
  oci         Manage OCI containers
  overlay     Manage an EXT3 writable overlay image
  plugin      Manage Apptainer plugins
  pull        Pull an image from a URI
  push        Upload image to the provided URI
  registry    Manage authentication to OCI/Docker registries
  remote      Manage apptainer remote endpoints
  run         Run the user-defined default command within a container
  run-help    Show the user-defined help for an image
  search      Search a Container Library for images
  shell       Run a shell within a container
  sif         Manipulate Singularity Image Format (SIF) images
  sign        Add digital signature(s) to an image
  test        Run the user-defined tests within a container
  verify      Verify digital signature(s) within an image
  version     Show the version for Apptainer

Examples:
  $ apptainer help <command> [<subcommand>]
  $ apptainer help build
  $ apptainer help instance start

For additional help or support, please visit https://apptainer.org/help/

```

## Downloading Images

- **Container Images** are executables that bundle together all necessary components for an application or an environment, like a template for containers.
- **Containers** are the runtime instance of images.

Pre-built containers can be obtained from a variety of sources like:

- Apptainer/Singularity Hub
- [Docker Hub](https://hub.docker.com/)
- [NVIDIA NGC Catalog](https://catalog.ngc.nvidia.com/containers)
- Other OCI registries

 You can use the [pull](https://apptainer.org/docs/user/main/cli/apptainer_pull.html#apptainer-pull) and [build](https://apptainer.org/docs/user/main/cli/apptainer_build.html#apptainer-build) commands to download images from an external resource like docker:

```bash
(master)$ apptainer pull docker://alpine
(master)$ apptainer build <container-name>.sif docker://alpine
```

Remember!! <pull> and <build> commands should be executed on the master node. The master node is the only one with internet access.

## Interacting with existing containers

### Run

Once the image is downloaded, you are ready to run it.  As an example, we will download the ***lolcow*** container and run it.

```bash
# Container downloading
(master)$ apptainer pull docker://ghcr.io/apptainer/lolcow
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
Copying blob 5ca731fc36c2 done   | 
Copying blob 16ec32c2132b done   | 
Copying config fd0daa4d89 done   | 
Writing manifest to image destination
2025/02/28 14:04:53  info unpack layer: sha256:16ec32c2132b43494832a05f2b02f7a822479f8250c173d0ab27b3de78b2f058
2025/02/28 14:04:54  info unpack layer: sha256:5ca731fc36c28789c5ddc3216563e8bfca2ab3ea10347e07554ebba1c953242e
INFO:    Creating SIF file...

# Container running in the system host
(master)$ apptainer run lolcow_latest.sif      
 ______________________________
< Fri Feb 28 14:05:17 -03 2025 >
 ------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
                

```

As we can see the container was download as an Apptainer Image File `lolcow_latest.sif` . This file was executed with the command `apptainer run <container-image-name>.sif`  . However, `run` is not the only command to run and interact with container, we will discuss this in the next lines. 

### Shell

You can create a new shell within your container and interact with it as though it were a virtual machine.

```bash
(master)$ apptainer shell lolcow_latest.sif   
Apptainer>
```

The change in prompt ( from `(master)$` to `Apptainer>`) indicates you are now inside the container. Additionally, your are the same user as you are in the host system and you can have access to the user home directory.

```bash
# I executing apptainer in the master node
Apptainer> hostname
khipu

# My user in the host system is aturing, in apptainer too
Apptainer> whoami
aturing

# I can access my home directory inside the container
Apptainer> pwd
/home/aturing
```

Note: You can execute apptainer shell in the master node, but is highly recommended to do so using an interactive slurm job.  To do so, you must add the following command before your apptainer commands `srun --pty --mem=2G -p debug <apptainer command>`. 

Here is the above example using and interactive slurm job.

```bash
# I will execute an apptainer shell in a compute node
srun --pty --mem=2G -p debug apptainer shell lolcow_latest.sif 
Apptainer> hostname
n003
```

Remember that jobs in the `debug` partition has a time limit of 30 minutes. For long time running jobs, you must execute it using an slurm batch job. We will explain this later. 

### Exec

With `exec`command you can execute a custom command within a container. For example, to execute the `cowsay`program within the `lolcow_latest.sif` container:

```bash
[rubaldo@khipu ~]$ apptainer exec lolcow_latest.sif cowsay Khipu
 _______
< Khipu >
 -------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

## Working with files

Files on the system host are accesible from within the container.  

```bash
[rubaldo@khipu ~]$ echo "Hello from Khipu" > $HOME/testfile.txt
[rubaldo@khipu ~]$ apptainer exec lolcow_latest.sif cat $HOME/testfile.txt
Hello from Khipu
```

By default, Apptainer bind mounts `$HOME`, the current working directory, and additional system locations from the host into the container.

References:

- [https://apptainer.org/docs/user/main/quick_start.html](https://apptainer.org/docs/user/main/quick_start.html)

# Building custom images

Apptainer allows users to build containers from a definition file (like Docker does with a dockerfile in ). Within this file you can add environment variables or install software dependencies to easily reproduce and share your containers.

A definition file has a header and a body. The header determines the base container to begin with, and the body is further divided into sections that perform tasks such as software installation, environment setup, and copying files into the container from host system.  Further information of how to create the apptainer definition file can be found [here](https://apptainer.org/docs/user/main/definition_files.html).

For example, we will create a `lolcow.def` file to define a container based on **ubuntu** and then install **cowsay** within. 

```bash
BootStrap: docker
From: ubuntu:24.04

%post
   apt-get -y update
   apt-get -y install cowsay lolcat

%environment
   export LC_ALL=C
   export PATH=/usr/games:$PATH

%runscript
   date | cowsay | lolcat
   
%labels
   Author Khipu
   Exec: apptainer

```

In this example the header tells Apptainer to start with a `ubuntu:24.04` base image from Docker Container Library. The `%post` section is executed in build time after the base image has been downloaded. In this example, we are using it to update the package library and install `cowsay` and `lolcat`.  The `%environment` section defines environment variables  for the container. The `%runscript` section is used to place actions for the container when it is executed (This commands will not be executed at build time). Finally, the `%label` section is used to place information about the container like the author, how to used it, examples, etc. 

To build the container from the above file we need to execute `apptainer build <container-name>.sif <container-definition-file>.def`

```bash
[rubaldo@khipu apptainer]$ apptainer build lolcow.sif lolcow.def 
INFO:    Starting build...
...
INFO:    Adding labels
INFO:    Adding environment to container
INFO:    Adding runscript
INFO:    Creating SIF file...
INFO:    Build complete: lolcow.sif

```

Then, to run the container is enough to execute `apptainer run lolcow.sif` or `./lolcow.sif`.

# GPU support

Apptainer has support for containers that use NVIDIA’s CUDA GPU computing framework. The following lines show how to create and run GPU containers in Khipu.

## Download images

The first step as [shown before](https://google.com) is to pull and build the container image. As an example we will `pull` a nvida-cuda12.8.0 image.

```bash
[rubaldo@khipu]$ apptainer pull docker://nvidia/cuda:12.8.0-base-ubuntu20.04
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
...
INFO:    Creating SIF file...

# Or
[rubaldo@khipu]$ apptainer build cuda12.sif docker://nvidia/cuda:12.8.0-base-ubuntu20.04
```

Remember: the `apptainer pull <something>` command only works on the master node. 

GPU nodes in Khipu are accesible through Slurm. In the following lines I will show how to compile and run your cuda code using Apptainer and Slurm.

- As a sample, use the [gpu-info.cu](http://gpu-info.cu) code and compile it in the master node:

```bash
# Load cuda module
[rubaldo@khipu]$ ml load cuda
[rubaldo@khipu]$ nvcc -o gpu_info gpu_info.cu
```

- Then, to execute the code just run:

```bash
[rubaldo@khipu]$ srun --mem=1G -p gpu apptainer exec --nv cuda_12.8.0-base-ubuntu20.04.sif ./gpu_info
CUDA Device(s) Found: 1
Device 0 Information:
Name: Tesla T4
Compute Capability: 7.5
Total Global Memory: 14915 MB
Shared Memory per Block: 48 KB
Registers per Block: 65536
Max Threads per Block: 1024
Max Block Dimensions: (1024, 1024, 64)
Max Grid Dimensions: (2147483647, 65535, 65535)
Clock Rate: 1590 MHz
Memory Clock Rate: 5001 MHz
Memory Bus Width: 256 bits
Total Constant Memory: 64 KB
Warp Size: 32
Multiprocessor Count: 40

```

As you can see the `--nv` is passed to the `exec` command to setup the environment to use an NVIDIA GPU and basic CUDA libraries. Without this flag the cuda devices  and libraries can not be used. This flag is not required at build time. 

- Is highly recommended to compile your cuda code in the master node, because cuda images with nvidia compiler within (`devel` images) are much bigger than `base` container images.

As a prove of concept, we can compile the same code using Apptainer. In order to do so we need to pull a `devel` cuda image like **nvidia/cuda:12.5.1-devel-ubuntu20.04** (3.6 GB) which is more than 30 times bigger than a `base` image (~95 MB).

```bash
[rubaldo@khipu]$ apptainer pull docker://nvidia/cuda:12.5.1-devel-ubuntu20.04
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
...
INFO:    Creating SIF file...
```

Then to compile the code, run:

```bash
[rubaldo@khipu]$ srun --mem=1G -p gpu apptainer exec --nv cuda_12.5.1-devel-ubuntu20.04.sif nvcc -o gpu_info gpu_info.cu
```

And finally to execute the code:

```bash
[rubaldo@khipu]$ srun --mem=1G -p gpu apptainer exec --nv cuda_12.5.1-devel-ubuntu20.04.sif ./gpu_info
CUDA Device(s) Found: 1
Device 0 Information:
Name: Tesla T4
Compute Capability: 7.5
Total Global Memory: 14915 MB
Shared Memory per Block: 48 KB
Registers per Block: 65536
Max Threads per Block: 1024
Max Block Dimensions: (1024, 1024, 64)
Max Grid Dimensions: (2147483647, 65535, 65535)
Clock Rate: 1590 MHz
Memory Clock Rate: 5001 MHz
Memory Bus Width: 256 bits
Total Constant Memory: 64 KB
Warp Size: 32
Multiprocessor Count: 40

```

To practice the above commands you can try to execute it all our gpu nodes adding the `--nodelist=<name of a gpu node>` to the srun command. To check the list of gpu nodes execute `sinfo`.

In all the previuos examples the `srun` commands was used to launch the slurm jobs. This command is suitable for short time jobs, but if your job requires much more time you should launch your jobs in batch mode with `sbatch`.

More information about Apptainer GPU support can be found [here](https://apptainer.org/docs/user/latest/gpu.html).

## Interactive mode

Sometimes you want to interact with your container like if you were on a shell. You can do this using an interactive slurm job. Interactive jobs should be executed on the `debug-gpu` partition and has a time limit of 30 minutes. Remember that container in apptainer are inmutable after build.

```bash
[rubaldo@khipu]$ srun --pty -p debug-gpu apptainer run --nv cuda_12.8.0-base-ubuntu20.04.sif
Apptainer>

# To check the nvidia driver
Apptainer> nvidia-smi
Sun Mar  2 09:20:22 2025       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 560.35.03              Driver Version: 560.35.03      CUDA Version: 12.6     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  Tesla T4                       On  |   00000000:37:00.0 Off |                    0 |
| N/A   78C    P0             51W /   70W |   13831MiB /  15360MiB |     55%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
...

```

## Apptainer Cache

When you generate a SIF image from remote sources, Apptainer will cache the image. By default, the cache folder will be created in `HOME` environmental variable (`$HOME/.apptainer/cache`). The `apptainer cache` command allows you to list and clean up your cache.

```bash
# List your cache
[rubaldo@khipu]$ apptainer cache list
There are 4 container file(s) using 7.31 GiB and 88 oci blob file(s) using 13.18 GiB of space
Total space used: 20.49 GiB

# The same command with details
apptainer cache list -v
NAME                     DATE CREATED           SIZE             TYPE
03bb9eb021f579ed871d5f   2025-03-02 07:52:09    4.41 MiB         blob
093f9cb4ed5900be5b069a   2025-02-08 14:25:21    0.18 KiB         blob
...

There are 4 container file(s) using 7.31 GiB and 88 oci blob file(s) using 13.18 GiB of space
Total space used: 20.49 GiB

# Clean you cache
apptainer cache clean

# Clean you cache files older than 15 days
apptainer cache clean --days 15

```

Try to regularly free up your apptainer cache.

## **SLURM Submission Script**

You can launch your Apptainer jobs in batch mode with an slurm script. It is highly recommended when your job will take a lot of time executing. As an example, the `lolcow_latest.sif`, container of the first example, will be run using a `lolcow.slurm` submission file.

```bash
#!/bin/bash

## Slurm Directives
#SBATCH --job-name=apptainer_lolcow
#SBATCH --output=apptainer_lolcow-%j.out
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=1G
#SBATCH -p debug

## Load modules, if needed

## Place to the directory where you container is, for example $HOME
cd $HOME

## Run the program using use 'srun'.
srun apptainer run lolcow_latest.sif 
```

In the above script, **`1 CPU`** with **`1GB RAM`** per CPU was requested in the `debug` partition for the task. This container will run only one (`--ntasks=1`), if you want to execute it more times try changing the value of `--ntasks=`, but remember that this only works when your commands is executed with the `srun` command.

To submit this script, just run:

```bash
[rubaldo@khipu]$ sbatch lolcow.slurm
# When the job finish, check you output with cat
[rubaldo@khipu]$ cat apptainer_lolcow-4377.out 
 _____________________________
< Mon Mar 2 11:36:49 -05 2025 >
 -----------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

For NVIDIA GPU jobs, in this case using the `cuda_12.8.0-base-ubuntu20.04.sif` image, the Slurm script `gpu_info.slurm` should be:

```bash
#!/bin/bash

## Slurm Directives
#SBATCH --job-name=apptainer_gpu_info
#SBATCH --output=apptainer_gpu_info-%j.out
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=1G
#SBATCH --gres=gpu:1
#SBATCH -p debug-gpu

## Load modules, if needed

## Place to the directory where you container is, for example $HOME
cd $HOME/apptainer

## Run the program using use 'srun'.
srun apptainer exec --nv cuda_12.8.0-base-ubuntu20.04.sif ./gpu_info
```

Then, to submit this script, just run:

```bash
[rubaldo@khipu]$ sbatch gpu_info.slurm
# Wait until the job finish. Then you will have an output like this:
[rubaldo@khipu]$ cat apptainer_gpu_info-4576.out 
CUDA Device(s) Found: 1
Device 0 Information:
Name: Tesla T4
Compute Capability: 7.5
Total Global Memory: 14915 MB
Shared Memory per Block: 48 KB
Registers per Block: 65536
Max Threads per Block: 1024
Max Block Dimensions: (1024, 1024, 64)
Max Grid Dimensions: (2147483647, 65535, 65535)
Clock Rate: 1590 MHz
Memory Clock Rate: 5001 MHz
Memory Bus Width: 256 bits
Total Constant Memory: 64 KB
Warp Size: 32
Multiprocessor Count: 40

```

If you want to get notified when your job fails or finish, don’t forget to add the following lines to you slurm script.

```bash
#SBATCH --mail-type=fail        # send email when job begins
#SBATCH --mail-type=end          # send email when job ends
#SBATCH --mail-user=<your email>
```

References:

[https://hpc.nmsu.edu/discovery/software/apptainer/using-containers/](https://hpc.nmsu.edu/discovery/software/apptainer/using-containers/)