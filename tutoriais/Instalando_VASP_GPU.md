Para seguir esse tutorial você deve ter baixado os arquivos:
- `vasp-6.5.1.tar.gz` -> Instalador do vasp
- `potpaw_LDA.64.tgz` -> Arquivos de Pseudo-potenciais LDA
- `potpaw_PBE.64.tgz` -> Arquivos de Pseudo-potenciais PBE

Em seguida, abra um *terminal do Ubuntu* e siga os comandos a seguir. 

--- 
## 1. Instalar oneAPI base e HPC Toolkit da Intel 

Os comandos a seguir foram obtidos do website oficial: https://www.intel.com/content/www/us/en/developer/tools/oneapi/toolkits.html#base-kit

- Vamos instalar o keyring do repositório
```bash
wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB \
| gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null
```

```bash
echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | sudo tee /etc/apt/sources.list.d/oneAPI.list
```

- Agora instale os pacotes a partir do repositório oficial
```bash
sudo apt install intel-oneapi-base-toolkit intel-oneapi-hpc-toolkit
```

- Como padrão, os pacotes devem ter sido instalados em `/opt/intel`. Teste a instalação usando o comando
```bash
ls /opt/intel
```

- Abra o arquivo `~/.bashrc` usando o comando 
```bash
nano ~/.bashrc
```

- Escreva os novos PATHs usando as seguintes linhas no arquivo `~/.bashrc`
```bash
# oneAPI MKL
export PATH=/opt/intel/oneapi/mkl/2025.2:$PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2025.2/lib/intel64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/compiler/2025.2/lib:$LD_LIBRARY_PATH
```
**OBS:** Para salvar o arquivo no `nano` use `Ctrl+S` e para fechar o arquivo use `Ctrl+X`

- Ative a nova configuração do arquivo `~/.bashrc`
```bash
source ~/.bashrc
```

- E ative as configurações do `oneapi` com o comando
```bash
source /opt/intel/oneapi/setvars.sh
```

## 2. Instalar NVHPC da Nvidia

Aqui estaremos seguindo os comandos apresentados no site oficial: https://developer.nvidia.com/hpc-sdk-downloads

- Instale o nvhpc usando os comandos abaixo
```bash
curl https://developer.download.nvidia.com/hpc-sdk/ubuntu/DEB-GPG-KEY-NVIDIA-HPC-SDK | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-hpcsdk-archive-keyring.gpg
```
Ele pedirá uma senha de `sudo` 

```bash
echo 'deb [signed-by=/usr/share/keyrings/nvidia-hpcsdk-archive-keyring.gpg] https://developer.download.nvidia.com/hpc-sdk/ubuntu/amd64 /' | sudo tee /etc/apt/sources.list.d/nvhpc.list
```

```bash
sudo apt-get update -y
```

```bash
sudo apt-get install -y nvhpc-25-7-cuda-multi
```
que deve ter sido instalado em `/opt/nvidia/`

- Adiciona as seguintes linhas ao arquivo `~/.bashrc`
```bash
# NVIDIA-HPC SDK
NVCOMPILERS=/opt/nvidia/hpc_sdk; export NVCOMPILERS
export NVHPCSDKPATH=$NVCOMPILERS/Linux_x86_64/25.7
export PATH=$NVHPCSDKPATH/comm_libs/mpi/bin:$PATH
export MANPATH=$NVHPCSDKPATH/comm_libs/mpi/man:$MANPATH
export MANPATH=$NVHPCSDKPATH/compilers/man:$MANPATH
export PATH=$NVHPCSDKPATH/compilers/bin:$PATH
export PATH=$NVHPCSDKPATH/compilers/extras:$PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/compilers/extras/qd/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/cuda/12.9/targets/x86_64-linux/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/comm_libs/12.9/openmpi4/openmpi-4.1.5/lib:$LD_LIBRARY_PATH
export PATH=$NVHPCSDKPATH/comm_libs/12.9/openmpi4/openmpi-4.1.5/bin:$PATH
export CUDA_PATH=$NVHPCSDKPATH/cuda/12.9
export LD_LIBRARY_PATH=$NVHPCSDKPATH/math_libs/12.9/targets/x86_64-linux/lib:$LD_LIBRARY_PATH
```

--- 
## 3. Instalar HDF5 para GPU

- Baixar o instalador `hdf5-1.14.6.tar.gz` do site oficial: [https://www.hdfgroup.org/downloads/hdf5/source-code/#](https://www.hdfgroup.org/downloads/hdf5/source-code/)

- Copie o arquivo baixado para o interior dessa pasta
```bash
cp [caminho]/Downloads/hdf5-1.14.6.tar.gz ~/Programs/.
```

- Em seguida entre na pasta `Programs` com o comando
```bash
cd ~/Programs
```

- Agora precisamos descompactar o instalador. Para isso use o comando
```bash
tar -xzvf hdf5-1.14.6.tar.gz
```

- Isso vai criar uma pasta `hdf5-1.14.6` dentro de `Programs`. Faça uma cópia dessa pasta usando o comando
```bash
cp -r hdf5-1.14.6 hdf5-1.14.6-gpu
```

- Entre nessa nova pasta com o comando:
```bash
cd hdf5-1.14.6-gpu/  
```

- Agora crie uma pasta onde vamos compilar o programa
```bash
mkdir build
```

- Em seguida podemos compilar o instalador linkando com os compiladores NVidia usando o comando 
```bash
./configure CC=nvc CXX=nvc++ FC=nvfortran --prefix=/home/elvis/Programs/hdf5-1.14.6-gpu/build --enable-fortran --enable-cxx --enable-shared
```
**OBS**: Cuidado com o caminho da pasta `build`. No meu caso foi `/home/elvis/Programs/hdf5-1.14.6-gpu/build`

- Comando para a instalação
```bash
make -j4 && make install 
```

- Pronto! Agora só precisamos atualizar o arquivo `~/.bashrc`
```bash
# HDF5
export PATH=/home/elvis/Programs/hdf5-1.14.6-gpu/build/bin:$PATH
export LD_LIBRARY_PATH=/home/elvis/Programs/hdf5-1.14.6-gpu/build/lib:$LD_LIBRARY_PATH
```
**OBS**: Cuidado com o caminho da pasta `hdf5-1.14.6-gpu`. No meu caso foi `/home/elvis/Programs/hdf5-1.14.6-gpu`

- No final, seu arquivo `~/.bashrc` deve ter os seguintes PATHs
```bash
# oneAPI MKL
export PATH=/opt/intel/oneapi/mkl/2025.2:$PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2025.2/lib/intel64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/compiler/2025.2/lib:$LD_LIBRARY_PATH

# NVIDIA-HPC SDK
NVCOMPILERS=/opt/nvidia/hpc_sdk; export NVCOMPILERS
export NVHPCSDKPATH=$NVCOMPILERS/Linux_x86_64/25.7
export PATH=$NVHPCSDKPATH/comm_libs/mpi/bin:$PATH
export MANPATH=$NVHPCSDKPATH/comm_libs/mpi/man:$MANPATH
export MANPATH=$NVHPCSDKPATH/compilers/man:$MANPATH
export PATH=$NVHPCSDKPATH/compilers/bin:$PATH
export PATH=$NVHPCSDKPATH/compilers/extras:$PATH
export CUDA_PATH=$NVHPCSDKPATH/cuda/12.9
export LD_LIBRARY_PATH=$NVHPCSDKPATH/compilers/extras/qd/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/cuda/12.9/targets/x86_64-linux/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/math_libs/12.9/targets/x86_64-linux/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/comm_libs/12.9/openmpi4/openmpi-4.1.5/lib:$LD_LIBRARY_PATH
export PATH=$NVHPCSDKPATH/comm_libs/12.9/openmpi4/openmpi-4.1.5/bin:$PATH

# HDF5
export PATH=/home/elvis/Programs/hdf5-1.14.6-gpu/build/bin:$PATH
export LD_LIBRARY_PATH=/home/elvis/Programs/hdf5-1.14.6-gpu/build/lib:$LD_LIBRARY_PATH
```

---
## 4. Compilando o VASP para GPU 

- Copie o arquivo `vasp-6.5.1.tar.gz` para a pasta Programs
```bash
cp [caminho]/Downloads/vasp-6.5.1.tar.gz ~/Programs/. && cd ~/Programs/
```

- Descompacte o arquivo usando o comando 
```bash
tar -xzvf vasp-6.5.1.tar.gz
```

- Entre na pasta `vasp-6.5.1` criada
```bash
cd vasp-6.5.1/
```

- Copie o arquivo `makefile.include.nvhpc_ompi_mkl_omp_acc` e depois faça uma cópia com o nome `makefile.include` 
```bash
cp [caminho]/Downloads/makefile.include.nvhpc_ompi_mkl_omp_acc . && cp makefile.include.nvhpc_ompi_mkl_omp_acc makefile.include
```

- Em seguida, modifique as seguintes linhas do arquivo `makefile.include`
```bash
HDF5_ROOT  ?= /home/elvis/Programs/hdf5-1.14.6-gpu/build
```
**OBS**: Cuidado novamente com os caminhos!

- No final, seu arquivo `makefile.include` deve se parecer com
```bash
# Default precompiler options
CPP_OPTIONS = -DHOST=\"LinuxNV\" \
              -DMPI -DMPI_INPLACE -DMPI_BLOCK=8000 -Duse_collective \
              -DscaLAPACK \
              -DCACHE_SIZE=4000 \
              -Davoidalloc \
              -Dvasp6 \
              -Dtbdyn \
              -Dqd_emulate \
              -Dfock_dblbuf \
              -D_OPENMP \
              -DACC_OFFLOAD \
              -DNVCUDA \
              -DUSENCCL

CPP         = nvfortran -Mpreprocess -Mfree -Mextend -E $(CPP_OPTIONS) $*$(FUFFIX)  > $*$(SUFFIX)

# N.B.: you might need to change the cuda-version here
#       to one that comes with your NVIDIA-HPC SDK
CC          = mpicc  -acc -gpu=cc60,cc70,cc80,cuda12.9 -mp
FC          = mpif90 -acc -gpu=cc60,cc70,cc80,cuda12.9 -mp
FCL         = mpif90 -acc -gpu=cc60,cc70,cc80,cuda12.9 -mp -c++libs

FREE        = -Mfree

FFLAGS      = -Mbackslash -Mlarge_arrays

OFLAG       = -fast

DEBUG       = -Mfree -O0 -traceback

LLIBS       = -cudalib=cublas,cusolver,cufft,nccl -cuda

# Redefine the standard list of O1 and O2 objects
SOURCE_O1  := pade_fit.o minimax_dependence.o wave_window.o
SOURCE_O2  := pead.o

# For what used to be vasp.5.lib
CPP_LIB     = $(CPP)
FC_LIB      = $(FC)
CC_LIB      = $(CC)
CFLAGS_LIB  = -O -w
FFLAGS_LIB  = -O1 -Mfixed
FREE_LIB    = $(FREE)

OBJECTS_LIB = linpack_double.o

# For the parser library
CXX_PARS    = nvc++ --no_warnings

##
## Customize as of this point! Of course you may change the preceding
## part of this file as well if you like, but it should rarely be
## necessary ...
##
# When compiling on the target machine itself , change this to the
# relevant target when cross-compiling for another architecture
VASP_TARGET_CPU ?= -tp host
FFLAGS     += $(VASP_TARGET_CPU)

# Specify your NV HPC-SDK installation (mandatory)
#... first try to set it automatically
NVROOT      =$(shell which nvfortran | awk -F /compilers/bin/nvfortran '{ print $$1 }')

# If the above fails, then NVROOT needs to be set manually
#NVHPC      ?= /opt/nvidia/hpc_sdk
#NVVERSION   = 21.11
#NVROOT      = $(NVHPC)/Linux_x86_64/$(NVVERSION)

## Improves performance when using NV HPC-SDK >=21.11 and CUDA >11.2
#OFLAG_IN   = -fast -Mwarperf
#SOURCE_IN  := nonlr.o

# Software emulation of quadruple precsion (mandatory)
QD         ?= $(NVROOT)/compilers/extras/qd
LLIBS      += -L$(QD)/lib -lqdmod -lqd
INCS       += -I$(QD)/include/qd

# Intel MKL for FFTW, BLAS, LAPACK, and scaLAPACK
MKLROOT    ?= /opt/intel/oneapi/mkl/2025.2
#MKLLIBS     = -Mmkl
MKLLIBS     = -lmkl_intel_lp64 -lmkl_gnu_thread -lmkl_core -pgf90libs -mp -lpthread -lm -ldl

# If you want to use scaLAPACK from MKL
LLIBS_MKL   = -L$(MKLROOT)/lib -lmkl_scalapack_lp64 -lmkl_blacs_openmpi_lp64 $(MKLLIBS)

# Use a separate scaLAPACK installation (optional but recommended in combination with OpenMPI)
# Comment out the two lines below if you want to use scaLAPACK from MKL instead
#SCALAPACK_ROOT ?= /path/to/your/scalapack/installation
#LLIBS_MKL   = -L$(SCALAPACK_ROOT)/lib -lscalapack $(MKLLIBS)

LLIBS      += $(LLIBS_MKL)

INCS       += -I$(MKLROOT)/include/fftw

# Use cusolvermp (optional)
# supported as of NVHPC-SDK 24.1 (and needs CUDA-11.8)
#CPP_OPTIONS+= -DCUSOLVERMP -DCUBLASMP
#LLIBS      += -cudalib=cusolvermp,cublasmp -lnvhpcwrapcal

# HDF5-support (optional but strongly recommended, and mandatory for some features)
CPP_OPTIONS+= -DVASP_HDF5
HDF5_ROOT  ?= /home/elvis/Programs/hdf5-1.14.6-gpu/build
LLIBS      += -L$(HDF5_ROOT)/lib -lhdf5_fortran
INCS       += -I$(HDF5_ROOT)/include

# For the VASP-2-Wannier90 interface (optional)
#CPP_OPTIONS    += -DVASP2WANNIER90
#WANNIER90_ROOT ?= /path/to/your/wannier90/installation
#LLIBS          += -L$(WANNIER90_ROOT)/lib -lwannier

# For the fftlib library (hardly any benefit for the OpenACC GPU port, especially in combination with MKL's FFTs)
#CPP_OPTIONS+= -Dsysv
#FCL        += fftlib.o
#CXX_FFTLIB  = nvc++ -mp --no_warnings -std=c++11 -DFFTLIB_USE_MKL -DFFTLIB_THREADSAFE
#INCS_FFTLIB = -I./include -I$(MKLROOT)/include/fftw
#LIBS       += fftlib
#LLIBS      += -ldl

# For machine learning library vaspml (experimental)
#CPP_OPTIONS += -Dlibvaspml
#CPP_OPTIONS += -DVASPML_USE_CBLAS
#CPP_OPTIONS += -DVASPML_DEBUG_LEVEL=3
#CXX_ML      = mpic++ -mp
#CXXFLAGS_ML = -O3 -std=c++17 -Wall -Wextra
#INCLUDE_ML  =

# Add -gpu=tripcount:host to compiler commands for NV HPC-SDK > 25.1
NVFORTRAN_VERSION := $(shell nvfortran --version | sed -n '2s/^nvfortran \([0-9.]*\).*/\1/p')
 define greater_or_equal
$(shell printf '%s\n%s\n' '$(1)' '$(2)' | sort -V | head -n1 | grep -q '$(2)' && echo true || echo false)
endef
ifeq ($(call greater_or_equal,$(NVFORTRAN_VERSION),25.1),true)
    CC  += -gpu=tripcount:host
    FC  += -gpu=tripcount:host
endif
```

- Em seguida, compile com o comando
```bash
make DEPS=1 -j
```

- Atualize o `~/.bashrc` com o PATH do VASP
```bash
# VASP
export PATH=/home/elvis/Programs/vasp.6.5.1/bin:$PATH
``` 

- No final, seu arquivo `~/.bashrc` deve estar assim 
```bash
# oneAPI MKL
export PATH=/opt/intel/oneapi/mkl/2025.2:$PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2025.2/lib/intel64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/compiler/2025.2/lib:$LD_LIBRARY_PATH

# NVIDIA-HPC SDK
NVCOMPILERS=/opt/nvidia/hpc_sdk; export NVCOMPILERS
export NVHPCSDKPATH=$NVCOMPILERS/Linux_x86_64/25.7
export PATH=$NVHPCSDKPATH/comm_libs/mpi/bin:$PATH
export MANPATH=$NVHPCSDKPATH/comm_libs/mpi/man:$MANPATH
export MANPATH=$NVHPCSDKPATH/compilers/man:$MANPATH
export PATH=$NVHPCSDKPATH/compilers/bin:$PATH
export PATH=$NVHPCSDKPATH/compilers/extras:$PATH
export CUDA_PATH=$NVHPCSDKPATH/cuda/12.9
export LD_LIBRARY_PATH=$NVHPCSDKPATH/compilers/extras/qd/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/cuda/12.9/targets/x86_64-linux/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/math_libs/12.9/targets/x86_64-linux/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$NVHPCSDKPATH/comm_libs/12.9/openmpi4/openmpi-4.1.5/lib:$LD_LIBRARY_PATH
export PATH=$NVHPCSDKPATH/comm_libs/12.9/openmpi4/openmpi-4.1.5/bin:$PATH

# HDF5
export PATH=/home/elvis/Programs/hdf5-1.14.6-gpu/build/bin:$PATH
export LD_LIBRARY_PATH=/home/elvis/Programs/hdf5-1.14.6-gpu/build/lib:$LD_LIBRARY_PATH

# VASP
export PATH=/home/elvis/Programs/vasp.6.5.1/bin:$PATH
```

- Pode usar essa nova configuração através do comando 
```bash
source ~/.bashrc
```

- Temos então os programas `vasp_std`,`vasp_gam`e `vasp_ncl` na pasta `/bin`. Modifique os nomes dos executáveis do VASP que usarão GPU
```bash
mv bin/vasp_std bin/vasp_std_gpu
```

```bash
mv bin/vasp_gam bin/vasp_gam_gpu
```

```bash
mv bin/vasp_ncl bin/vasp_ncl_gpu
```

- Para rodar qualquer programa do VASP usando GPU Nvidia você deve usar o comando
```bash
mpirun -np 1 vasp_std_gpu
```

## 5. Compilando o VASP para CPU usando OpenMPI da NVIDIA

- Certifique-se de limpar o ambiente com 
```
make veryclean
```

- Copie o arquivo `makefile.include.nvhpc_ompi_mkl_omp` e depois faça uma cópia com o nome `makefile.include` 
```bash
cp [caminho]/Downloads/makefile.include.nvhpc_ompi_mkl_omp . && cp makefile.include.nvhpc_ompi_mkl_omp makefile.include
```

- Em seguida, modifique as seguintes linhas do arquivo `makefile.include`
```bash
HDF5_ROOT  ?= /home/elvis/Programs/hdf5-1.14.6-gpu/build
```
**OBS**: Cuidado novamente com os caminhos!

- No final, seu arquivo `makefile.include` deve se parecer com
```bash
# Default precompiler options
CPP_OPTIONS = -DHOST=\"LinuxNV\" \
              -DMPI -DMPI_BLOCK=8000 -Duse_collective \
              -DscaLAPACK \
              -DCACHE_SIZE=4000 \
              -Davoidalloc \
              -Dvasp6 \
              -Dtbdyn \
              -Dqd_emulate \
              -Dfock_dblbuf \
              -D_OPENMP

CPP         = nvfortran -Mpreprocess -Mfree -Mextend -E $(CPP_OPTIONS) $*$(FUFFIX)  > $*$(SUFFIX)

FC          = mpif90 -mp
FCL         = mpif90 -mp -c++libs

FREE        = -Mfree

FFLAGS      = -Mbackslash -Mlarge_arrays

OFLAG       = -fast

DEBUG       = -Mfree -O0 -traceback

# Redefine the standard list of O1 and O2 objects
SOURCE_O1  := pade_fit.o minimax_dependence.o
SOURCE_O2  := pead.o

# For what used to be vasp.5.lib
CPP_LIB     = $(CPP)
FC_LIB      = nvfortran
CC_LIB      = nvc -w
CFLAGS_LIB  = -O
FFLAGS_LIB  = -O1 -Mfixed
FREE_LIB    = $(FREE)

OBJECTS_LIB = linpack_double.o

# For the parser library
CXX_PARS    = nvc++ --no_warnings

##
## Customize as of this point! Of course you may change the preceding
## part of this file as well if you like, but it should rarely be
## necessary ...
##
# When compiling on the target machine itself , change this to the
# relevant target when cross-compiling for another architecture
VASP_TARGET_CPU ?= -tp host
FFLAGS     += $(VASP_TARGET_CPU)

# Specify your NV HPC-SDK installation (mandatory)
#... first try to set it automatically
NVROOT      =$(shell which nvfortran | awk -F /compilers/bin/nvfortran '{ print $$1 }')

# If the above fails, then NVROOT needs to be set manually
#NVHPC      ?= /opt/nvidia/hpc_sdk
#NVVERSION   = 21.11
#NVROOT      = $(NVHPC)/Linux_x86_64/$(NVVERSION)

# Software emulation of quadruple precsion (mandatory)
QD         ?= $(NVROOT)/compilers/extras/qd
LLIBS      += -L$(QD)/lib -lqdmod -lqd
INCS       += -I$(QD)/include/qd

# Intel MKL for FFTW, BLAS, LAPACK, and scaLAPACK
MKLROOT    ?= /opt/intel/oneapi/mkl/2025.2
#MKLLIBS     = -Mmkl
MKLLIBS     = -lmkl_intel_lp64 -lmkl_gnu_thread -lmkl_core -pgf90libs -mp -lpthread -lm -ldl

# If you want to use scaLAPACK from MKL
LLIBS_MKL   = -L$(MKLROOT)/lib -lmkl_scalapack_lp64 -lmkl_blacs_openmpi_lp64 $(MKLLIBS)

# Use a separate scaLAPACK installation (optional but recommended in combination with OpenMPI)
# Comment out the two lines below if you want to use scaLAPACK from MKL instead
#SCALAPACK_ROOT ?= /path/to/your/scalapack/installation
#LLIBS_MKL   = -L$(SCALAPACK_ROOT)/lib -lscalapack $(MKLLIBS)

LLIBS      += $(LLIBS_MKL)

INCS       += -I$(MKLROOT)/include/fftw

# HDF5-support (optional but strongly recommended, and mandatory for some features)
CPP_OPTIONS+= -DVASP_HDF5
HDF5_ROOT  ?= /home/elvis/Programs/hdf5-1.14.6-gpu/build
LLIBS      += -L$(HDF5_ROOT)/lib -lhdf5_fortran
INCS       += -I$(HDF5_ROOT)/include

# For the VASP-2-Wannier90 interface (optional)
#CPP_OPTIONS    += -DVASP2WANNIER90
#WANNIER90_ROOT ?= /path/to/your/wannier90/installation
#LLIBS          += -L$(WANNIER90_ROOT)/lib -lwannier

# For the fftlib library (hardly any benefit in combination with MKL's FFTs)
#CPP_OPTIONS+= -Dsysv
#FCL        += fftlib.o
#CXX_FFTLIB  = nvc++ -mp --no_warnings -std=c++11 -DFFTLIB_USE_MKL -DFFTLIB_THREADSAFE
#INCS_FFTLIB = -I./include -I$(MKLROOT)/include/fftw
#LIBS       += fftlib
#LLIBS      += -ldl

# For machine learning library vaspml (experimental)
#CPP_OPTIONS += -Dlibvaspml
#CPP_OPTIONS += -DVASPML_USE_CBLAS
#CPP_OPTIONS += -DVASPML_DEBUG_LEVEL=3
#CXX_ML      = mpic++ -mp
#CXXFLAGS_ML = -O3 -std=c++17 -Wall -Wextra
#INCLUDE_ML  =
```

- Compile usando
```bash
make DEPS=1 -j6
```

- Temos então os programas `vasp_std`,`vasp_gam`e `vasp_ncl` na pasta `/bin`. Modifique os nomes dos executáveis do VASP que usarão CPU
```bash
mv bin/vasp_std bin/vasp_std_cpu
```

```bash
mv bin/vasp_gam bin/vasp_gam_cpu
```

```bash
mv bin/vasp_ncl bin/vasp_ncl_cpu
```

- Para rodar qualquer programa do VASP usando CPU você deve usar o comando
```bash
mpirun -np 4 vasp_std_cpu
```

- Para rodar com todos os processadores e threads use o comando 
```bash
mpirun --use-hwthread -np N vasp_std_cpu
```

Pronto! Agora está pronto para rodar seus problemas usando o VASP com GPU ou CPU.

## 5. Instalando PseudoPotenciais

- Para os pseudopotenciais, crie um pasta `pp` dentro das pasta `vasp.6.5.1`
```bash
mkdir -p pp
```

- Copie os arquivos de pseudopotenciais para essa pasta
```bash
cp [caminho]/Downloads/potpaw_LDA.64.tgz pp/. && cp [caminho]/Downloads/potpaw_PBE.64.tgz pp/.
```

- Entre na pasta `pp`
```bash
cd pp
```

- Crie as seguintes pastas
```bash
mkdir potpaw potpaw_PBE
```

- Descompacte o arquivo `potpaw_LDA.64.tgz` para dentro da pasta `potpaw`
```bash
tar -xvzf potpaw_LDA.64.tgz -C potpaw
```

- Descompacte o arquivo `potpaw_PBE.64.tgz` para dentro da pasta `potpaw_PBE`
```bash
tar -xvzf potpaw_PBE.64.tgz -C potpaw_PBE
```
