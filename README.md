# Custom libraries on the KBFI cluster

We use a software layer [singularity](https://sylabs.io/singularity/) that allows every user to create an isolated environment or "singularity image" for themselves in which they can install anything they wish. It's widely used by the LHC grid and by multiple HPC sites.

Basically, this means that instead of executing directly

``` bash
[manivald]$ python3 my_program.py
```

which will fail due to missing libraries, you run

```bash
[manivald]$ singularity shell /scratch/0/singularity/images/base.simg:latest
Singularity> python3 my_program.py

or

[manivald]$ singularity exec /scratch/0/singularity/images/base.simg:latest python3 my_program.py
```

such that your program runs within the environment defined in `/scratch/0/singularity/images/base.simg`.

This extra step is necessary to allow multiple users to work on the same system, even if they require mutually incompatible libraries. It also means that the user can directly install any software they wish, rather than waiting for the admin to do it.

We have the following software packages available:
- `/scratch/0/singularity/images/base.simg:latest`: defined by [base.singularity](specs/base.singularity)
  - CUDA 10.2
  - tensorflow 2.3, pytorch 1.5
  - numpy, scipy, sklearn, numba
  - astropy
  - jupyter

# Building the image yourself

Anyone can build the image on any Linux system on which you have root access. This can be your personal laptop or desktop, or you can use the [Syslabs Remote Builder](https://cloud.sylabs.io/builder).

```bash
sudo singularity build images/base.simg specs/base.singularity
```

Currently, you cannot build the image on `manivald` due to permission concerns.

# Adding libraries

Add the necessary libraries using a pull request. Currently, the image is built manually, but we are investigating ways to automate it.
