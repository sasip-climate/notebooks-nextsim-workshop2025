## NeXtSIM-DG on the GPU
Robert Jendersie (Otto von Guericke University, Magdeburg)

### About
In this part of the workshop we try out the GPU version of neXtSIM-DG. Our implementation of neXtSIM-DG's dynamical core with the heterogenous compute framework [Kokkos](https://github.com/kokkos/kokkos) (for details see [Jendersie et al. (2025)](https://doi.org/10.5194/gmd-18-3017-2025)) delivers a significant speedup over the OpenMP based parallel CPU code and runs on diverse hardware. Through the effective utilization of modern GPUs, higher resolution sea-ice simulations become feasible.

In the session we will set up neXtSIM-DG to use a GPU and run the benchmark from [Mehlmann et al. (2021)](https://doi.org/10.5194/gmd-18-3017-2025) at high resolution. This will be done on a dedicated machine but a low-resolution example of the benchmark is provided here as a Jupyther notebook. Once successfully built, the GPU version can also be used to run the other cases faster.

### Perquisites
To run the GPU version of neXtSIM-DG, a computer with a compatible GPU is required. In principle, the device just needs to be supported by one of the available [Kokkos backends](https://kokkos.org/kokkos-core-wiki/get-started/configuration-guide.html), however only the CUDA and HIP backends have been tested. To participate we therefore recommend one of the following options:
* Use your own hardware if you have access to a machine with a CUDA or ROCm supported GPU. This includes all NVIDIA GPUs, from data center GPUs (e.g. A100, H100), to desktop GPUs (e.g. RTX3090) down to mobile chips (e.g. GTX 1650). On the AMD side, [ROCm support](https://rocm.docs.amd.com/en/latest/compatibility/compatibility-matrix.html) is limited to data center GPUs (e.g. MI250X) and some deskop GPUs.
* Work on our server which is equipped with two H100 GPUs. Accounts will be provided in the session.
* (Use a cloud service. It is technically possible to run neXtSIM-DG in a [google collab](https://colab.research.google.com/drive/1Lqd5_UR1iBk-nCCWEOhgYVzp7gDKObEv?usp=sharing). However, at least on the free tier this is impractical, with a full build requiring around 30 min.)

### Setup
The GPU version of neXtSIM-DG is not available in the docker image since it has to be build from source for a specific target architecture. We assume that the GPU drivers and CUDA or ROCm are already installed.
Further standard dependencies include NetCDF, Boost and Eigen, which can be installed via a package manager or module system.
```bash
# ubuntu
sudo apt install netcdf-bin libnetcdf-c++4-dev libboost-all-dev cmake libeigen3-dev
# fedora
sudo dnf install netcdf-cxx4-devel boost-devel eigen3-devel cmake gcc14-c++
```
In addition, the GPU version requires Kokkos. While a system-wide installation of Kokkos is possible, it is easier to use a local version.
After checking out the `kokkos` branch of neXtSIM-DG, a release of the Kokkos library can simply be placed in `lib/kokkos`.
```bash
# clone needed branch
git clone --single-branch --branch kokkos https://github.com/nextsimhub/nextsimdg
# get kokkos
wget https://github.com/kokkos/kokkos/releases/download/4.6.01/kokkos-4.6.01.zip 
unzip kokkos-4.6.01.zip
mv kokkos-4.6.01 lib/kokkos
rm kokkos-4.6.01.zip
```

With all dependencies in place, neXtSIM-DG can be configured and build.
```bash
mkdir build && cd build
cmake .. -D BUILD_TESTS=OFF -D CMAKE_BUILD_TYPE=RELEASE -D WITH_THREADS=ON -D WITH_KOKKOS=ON -D Kokkos_ENABLE_OPENMP=ON -D Kokkos_ENABLE_CUDA=ON
make -j8
```
The specific `Kokkos_` arguments needed vary based on the machine. The appropriate [device backend](https://kokkos.org/kokkos-core-wiki/get-started/configuration-guide.html) has to be enabled, e.g. `Kokkos_ENABLE_CUDA`  for CUDA. Furthermore, if the build and execution environments are different, it is necessary to specify the GPU architecture manually, e.g. `Kokkos_ARCH_AMPERE80=ON` for the A100.

During configuration, CMake calls a python script which generates the initial condition files for the benchmark. For this, an active python environment with NetCDF is expected, but the script can also be run manually later on.
```bash
cd run
python make_init_benchmark.py 
```
### References
Mehlmann, C., Danilov, S., Losch, M., Lemieux, J. F., Hutter, N., Richter, T., et al. (2021). Simulating linear kinematic features in viscous-plastic sea ice models on quadrilateral and triangular grids with different variable staggering. Journal of Advances in Modeling Earth Systems, 13(11). [2021ms002523](https://doi.org/10.1029/2021ms002523).

Richter, T., Dansereau, V., Lessig, C., & Minakowski, P. (2023). A dynamical core based on a discontinuous Galerkin method for higher-order finite-element sea ice modeling. Geoscientific Model Development, 16(13), 3907–3926. [gmd-16-3907-2023](https://doi.org/10.5194/gmd-16-3907-2023).

Jendersie, R., Lessig, C., & Richter, T. (2025). A GPU parallelization of the neXtSIM-DG dynamical core (v0.3.1). Geoscientific Model Development, 18(10), 3017–3040. [gmd-18-3017-2025](https://doi.org/10.5194/gmd-18-3017-2025).