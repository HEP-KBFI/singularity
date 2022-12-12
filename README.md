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

# Prebuilt images

I maintain a number of images for libraries like TF, pytorch here: 
```
ls /home/software/singularity/
```

You can also find images on dockerhub, in /cvmfs, or from the internet (do not run untrusted images).

# Building the image yourself

For local usage (manivald, gpu0), do this **once** on manivald:
```bash
singularity build --fakeroot base.simg specs/base.singularity
```

# Adding software to the image
Add the necessary libraries using a pull request by editing the [spec file](specs).
