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
  - CUDA 11.0
  - tensorflow 2.4
  - pytorch 1.7
  - numpy, scipy, sklearn, numba
  - astropy
  - jupyter
- `/home/software/singularity/tf26.simg:latest`: defined by [tf26.singularity](specs/tf26.singularity)
  - CUDA 11.2
  - tensorflow 2.6
  - numpy, scipy, sklearn, numba
  - astropy
  - jupyter

# Building the image yourself
Anyone can build the image on any Linux system on which you have root access. This can be your personal laptop or desktop, or you can do on manivald:

```bash
singularity build --fakeroot base.simg specs/base.singularity
```

# Adding software to the image
Add the necessary libraries using a pull request by editing the [spec file](specs).
