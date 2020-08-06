# Custom libraries on the KBFI cluster

We use a software layer [singularity](https://sylabs.io/singularity/) that allows every user to create an isolated environment or "singularity image" for themselves in which they can install anything they wish. It's widely used by the LHC grid and by multiple HPC sites.

Basically, this means that instead of executing directly

``` bash
[manivald]$ python3 my_program.py
```

which will fail due to missing libraries, you run

```bash
[manivald]$ singularity shell /home/software/singularity/base.simg:latest
Singularity> python3 my_program.py

or

[manivald]$ singularity exec /home/software/singularity/base.simg:latest python3 my_program.py
```

such that your program runs within the environment defined in `/home/software/singularity/base.simg:latest`.

This extra step is necessary to allow multiple users to work on the same system, even if they require mutually incompatible libraries. It also means that the user can directly install any software they wish, rather than waiting for the admin to do it.

We have the following software packages available:
- `/home/software/singularity/base.simg:latest`: defined by [base.singularity](specs/base.singularity)
  - CUDA 10.2
  - tensorflow 2.3, pytorch 1.5
  - numpy, scipy, sklearn, numba
  - astropy
  - jupyter

# Building the image yourself
Anyone can build the image on any Linux system on which you have root access. This can be your personal laptop or desktop, or you can use the [Syslabs Remote Builder](https://cloud.sylabs.io/builder) on manivald after creating a Syslabs account.

```bash
#as root on your personal linux machine, copy the image to manivald later
sudo singularity build base.simg specs/base.singularity

#using the remote builder on manivald
#one-time only: create a syslabs account and authentication token, then run 
singularity remote login

#now you can build the image
singularity build -r base.simg specs/base.singularity
```

# Adding software to the image
Add the necessary libraries using a pull request by editing the [spec file](specs).

# Using the GPUs on the cluster
We have 10x GPUs available using the batch system:

```bash
#run a single command, requesting one GPU
srun --gpus=1 -p gpu nvidia-smi -L

#run an interactive shell
srun --gpus=1 -p gpu --pty /bin/bash

#run a batch script
sbatch --gpus=1 -p gpu script.sh
```

Here is an example batch script where we do a tensorflow training using the singularity image:
```bash
#!/bin/bash

#the following environment variable contains the allocated GPUs
echo $CUDA_VISIBLE_DEVICES

singularity exec -B /scratch -B /home --nv \
  /home/software/singularity/base.simg:latest \
  python3 my_tensorflow_training.py
```
