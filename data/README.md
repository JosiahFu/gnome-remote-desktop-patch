# How to produce the PTX instructions for CUDA kernels

For the generation of the PTX instructions, the CUDA toolkit needs to be
installed (See for this below in "Retrieving the CUDA toolkit").

## Generation

When the CUDA toolkit is installed, and the current directory is `src`,
generate the PTX instructions via:
`/opt/cuda/bin/./nvcc -arch=compute_30 -ptx grd-cuda-avc-utils.cu -o ../data/grd-cuda-avc-utils_30.ptx`

The nvcc path differ from OS to OS. In the case above, Archlinux is used, which
uses the path `/opt/cuda/bin/nvcc` for nvcc.

`-arch=compute_30` tells nvcc to generate instructions for GPUs with compute
capability 3.0.
CUDA GPUs with higher compute capability can also run CUDA kernels with lower
compute capability.
The CUDA kernel for gnome-remote-desktop uses compute capability 3.0, as
compute capability 3.0 is the one of Kepler GPUs.
Kepler GPUs are the first generation GPUs, that support NVENC. To remain
compatible with these GPUs, generate the instructions for compute capability
3.0.

Also append a suffix, like in the example above, for the compute capability of
the generated PTX instructions.
If a kernel might be more efficient with newer CUDA features, generate a kernel
for the higher necessary compute capability and another one for older GPUs as
fallback.
Use then the CUDA functions to check the compute capability of the selected GPU
at runtime to determine, which PTX instructions should be loaded.

## Retrieving the CUDA toolkit:

Retrieving the CUDA toolkit depends on the distribution. It should be noted,
that the generation of PTX instructions for compute capability 3.0 was removed
from the CUDA toolkit version 11.
So, an older version of the CUDA toolkit (version 10) is needed to generate PTX
instructions for Kepler GPUs.

## Instructions to retrieve the CUDA toolkit version 10 on Archlinux:

While the current version of the CUDA toolkit can be found in the `community`
repository (included by default) under the name `cuda`, the older version 10 is
available via the Arch Linux Archive:

For this, the easiest way here is to use the `downgrade` tool (AUR):

Run `downgrade gcc8 gcc8-libs cuda` (might require root privileges) and choose
the latest gcc8 and gcc8-libs version for gcc8 (dependency of CUDA 10) and for
CUDA 10 choose the latest CUDA 10 release.
The downgrade utility will then download and install these packages.

After this, you can use nvcc to generate PTX instructions for Kepler GPUs as
well.
