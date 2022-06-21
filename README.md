# Custom libraries on the KBFI cluster

We use a software layer [singularity](https://sylabs.io/singularity/) that allows every user to create an isolated environment or "singularity image" for themselves in which they can install anything they wish. It's widely used by the LHC grid and by multiple HPC sites.
This allows to install any system libraries or binary packages, including complicated stuff like CUDA, for multiple mutually incompatible setups at the same time (in different images).

Basically, this means that instead of executing directly

``` bash
[manivald]$ python3 my_program.py
```

which will fail due to missing libraries, you run

```bash
[manivald]$ singularity shell /home/software/singularity/base.simg
Singularity> python3 my_program.py

or

[manivald]$ singularity exec /home/software/singularity/base.simg python3 my_program.py
```

such that your program runs within the environment defined in `/home/software/singularity/base.simg`.

# Building the image yourself

For local usage (manivald, gpu0):
```bash
singularity build --fakeroot base.simg specs/base.singularity
```

For batch usage where the jobs run in a container, the sandbox mode must be used
```bash
singularity build --fakeroot --sandbox /home/software/singularity/base-2022-XX-XX specs/base.singularity
```

# Adding software to the image
Add the necessary libraries using a pull request by editing the [spec file](specs).
