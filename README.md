# Custom libraries on the KBFI cluster

We use a software layer [singularity](https://docs.sylabs.io/guides/latest/user-guide/) that allows every user to create an isolated environment or "singularity image" for themselves in which they can install anything they wish. It's widely used by the LHC grid and by multiple HPC sites.
This allows to install any system libraries or binary packages, including complicated stuff like CUDA, for multiple mutually incompatible setups at the same time (in different images).

Basically, this means that instead of executing directly

``` bash
[manivald]$ python3 my_program.py
```

which will fail due to missing libraries, you run

```bash
[manivald]$ /home/software/bin/run-env.sh /home/software/singularity/base.simg python3 my_program.py
```

such that your program runs within the environment defined in `/home/software/singularity/base.simg`.

# Prebuilt images

A number of images are maintained here:
```
/home/software/singularity/base.simg: modern gcc, cmake, OpenCL, CUDA 11.7
/home/software/singularity/pytorch.simg:2024-05-21: pytorch 2.3, CUDA 12.1
/home/software/singularity/tf-2.14.0.simg:2024-01-16: tensorflow 2.14, CUDA 11.8
```

You can also find images on dockerhub, in /cvmfs, or from the internet (do not run untrusted images).

# Advanced

## Building the image yourself

For local usage (manivald, gpu0), do this **once** on manivald:
```bash
singularity build --fakeroot base.simg specs/base.singularity
```

## Adding software to the image
Add the necessary libraries using a pull request by editing the [spec file](specs).


# Misc

## Building mlpf-torchsparse

The [torchsparse](https://github.com/mit-han-lab/torchsparse) build should be done on a GPU node for it to have GPU support.
