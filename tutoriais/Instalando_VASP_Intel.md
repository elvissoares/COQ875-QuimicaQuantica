
Para seguir esse tutorial você deve ter baixado os arquivos:
- `vasp-6.5.1.tar.gz` -> Instalador do vasp
- `potpaw_LDA.64.tgz` -> Arquivos de Pseudo-potenciais LDA
- `potpaw_PBE.64.tgz` -> Arquivos de Pseudo-potenciais PBE

Em seguida, abra um *terminal do Ubuntu* e siga os comandos a seguir. 

--- 
## 1. Instalar oneAPI base e HPC Toolkit da Intel 

Os comandos a seguir foram obtidos do website oficial: https://www.intel.com/content/www/us/en/developer/tools/oneapi/toolkits.html#base-kit

- Instalando pacote a partir do repositório oficial
```bash
sudo apt install intel-oneapi-base-toolkit intel-oneapi-hpc-toolkit
```

- Como padrão, os pacotes devem ter sido instalados em `/opt/intel`. Teste a instalação usando o comando
```bash
ls /opt/intel
```

- Abra o arquivo `.bashrc` usando o comando 
```bash
nano .bashrc
```

- Escreva os novos PATHs usando as seguintes linhas no arquivo `.bashrc`
```bash
# oneAPI MKL
export PATH=/opt/intel/oneapi/mkl/2025.2:$PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2025.2/lib/intel64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/compiler/2025.2/lib:$LD_LIBRARY_PATH
```
**OBS:** Para salvar o arquivo no `nano` use `Ctrl+S` e para fechar o arquivo use `Ctrl+X`

- Ative a nova configuração do arquivo `.bashrc`
```bash
source .bashrc
```

- E ative as configurações do `oneapi` com o comando
```bash
source /opt/intel/oneapi/setvars.sh
```

## 2. Instalando OpenMPI com oneAPI

- Crie uma pasta `Programs` no diretório raiz com o comando
```bash
mkdir ~/Programs && cd ~/Programs
```

- Faça o download do OpenMPI diretamente do site oficial
```bash
wget https://download.open-mpi.org/release/open-mpi/v5.0/openmpi-5.0.8.tar.gz
  ```

-  Descompacte o arquivo
```bash
tar -xvzf openmpi-5.0.8.tar.gz
```

- Entre na pasta criada 
```bash
cd openmpi-5.0.8
```

- Crie uma pasta `build`
```bash
mkdir build 
```

- Compile com o comando 
```bash
./configure CC=icx CXX=icpx FC=ifx --prefix=/home/elvis/Programs/openmpi-5.0.8/build
```
OBS: cuidado com o caminho `/home/elvis/Programs/openmpi-5.0.8/build`

- Comando para instalação
```bash
make -j && make install  
```

- Exporte o PATH para o arquivo `.bashrc`
```bash
# OpenMPI
export PATH=/home/elvis/Programs/openmpi-5.0.8/build/bin:$PATH  
export LD_LIBRARY_PATH=/home/elvis/Programs/openmpi-5.0.0/build/lib:$LD_LIBRARY_PATH
```

- Depois pode usar nova configuração com
```bash
source ~/.bashrc
```

- Em seguida pode testar os compiladores `mpi` com 
```bash
mpirun --version
mpif90 --version
```

--- 
## 3. Instalar HDF5 

- Antes de tudo, instale o zlib
```bash
sudo apt install zlib1g
```

- Baixar o instalador `hdf5-1.14.6.tar.gz` do site oficial: [https://www.hdfgroup.org/downloads/hdf5/source-code/#](https://www.hdfgroup.org/downloads/hdf5/source-code/)

- Copie o arquivo baixado para o interior da pasta `Programs`
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
cp -r hdf5-1.14.6 hdf5-1.14.6-intel
```

- Entre nessa nova pasta com o comando:
```bash
cd hdf5-1.14.6-intel/  
```

- Agora crie uma pasta onde vamos compilar o programa
```bash
mkdir build
```

- Em seguida podemos compilar o instalador linkando os compiladores Intel usando o comando 
```bash
./configure CC=icx CXX=icpx FC=ifx --prefix=/home/elvis/Programs/hdf5-1.14.6-intel/build --enable-fortran --enable-cxx --enable-shared
```

```bash
./configure CC=mpiicx CXX=mpiicpx FC=mpiifx --prefix=/home/elvis/Programs/hdf5-1.14.6-intel/build --enable-parallel --enable-fortran --enable-shared
```

- Comando para a instalação
```bash
make -j4 && make install
```

- Pronto! Agora só precisamos atualizar o arquivo `.bashrc` (Cuidado com o caminho da pasta `hdf5-1.14.6-intel`. No meu caso foi `/home/elvis/Programs/hdf5-1.14.6-intel`)
```bash
# HDF5
export PATH=/home/elvis/Programs/hdf5-1.14.6-intel/build/bin:$PATH
export LD_LIBRARY_PATH=/home/elvis/Programs/hdf5-1.14.6-intel/build/lib:$LD_LIBRARY_PATH
```

- No final, seu arquivo `.bashrc` deve ter os seguintes novos PATHs
```bash
# oneAPI MKL
export PATH=/opt/intel/oneapi/mkl/2025.2:$PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/mkl/2025.2/lib/intel64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/opt/intel/oneapi/compiler/2025.2/lib:$LD_LIBRARY_PATH

# OpenMPI
export PATH=/home/elvis/Programs/openmpi-5.0.8/build/bin:$PATH  
export LD_LIBRARY_PATH=/home/elvis/Programs/openmpi-5.0.8/build/lib:$LD_LIBRARY_PATH

# HDF5
export PATH=$HOME/Programs/hdf5-1.14.6-intel/build/bin:$PATH
export LD_LIBRARY_PATH=$HOME/Programs/hdf5-1.14.6-intel/build/lib:$LD_LIBRARY_PATH
```

- Pode fazer nova configuração funcionar com o comando
```bash
source ~/.bashrc
```

---
## 4. Compilando o VASP com CPU Intel

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

- Copie o arquivo `makefile.include.intel_ompi_mkl_omp` e renomeie-o para um arquivo `makefile.include` 
```bash
cp [caminho]/Downloads/makefile.include.intel_ompi_mkl_omp ~/Programs/. && cp makefile.include.intel_ompi_mkl_omp makefile.include
```

- Em seguida, modifique as seguintes linhas do arquivo `makefile.include`
```bash
CC_LIB      = icx
CXX_PARS    = icpx

MKLROOT    ?= /opt/intel/oneapi/mkl/2025.2
LLIBS      += -L$(MKLROOT)/lib/intel64 -lmkl_scalapack_lp64 -lmkl_blacs_intelmpi_lp64

HDF5_ROOT  ?= /home/elvis/Programs/hdf5-1.14.6-intel/build
```
**OBS**: Cuidado novamente com os caminhos!

- Comente as seguintes linhas do arquivo `makefile.include`:
```bash
# SCALAPACK_ROOT ?= /path/to/your/scalapack/installation
# LLIBS      += -L${SCALAPACK_ROOT}/lib -lscalapack
```

- No final, seu arquivo `makefile.include` deve estar assim. 
```bash
# Default precompiler options
CPP_OPTIONS = -DHOST=\"LinuxIFC\" \
              -DMPI -DMPI_BLOCK=8000 -Duse_collective \
              -DscaLAPACK \
              -DCACHE_SIZE=4000 \
              -Davoidalloc \
              -Dvasp6 \
              -Dtbdyn \
              -Dfock_dblbuf \
              -D_OPENMP

CPP         = fpp -f_com=no -free -w0  $*$(FUFFIX) $*$(SUFFIX) $(CPP_OPTIONS)

FC          = mpif90 -qopenmp
FCL         = mpif90

FREE        = -free -names lowercase

FFLAGS      = -assume byterecl -w

OFLAG       = -O2
OFLAG_IN    = $(OFLAG)
DEBUG       = -O0

# For what used to be vasp.5.lib
CPP_LIB     = $(CPP)
FC_LIB      = $(FC)
CC_LIB      = icx
CFLAGS_LIB  = -O
FFLAGS_LIB  = -O1
FREE_LIB    = $(FREE)

OBJECTS_LIB = linpack_double.o

# For the parser library
CXX_PARS    = icpx
LLIBS       = -lstdc++

##
## Customize as of this point! Of course you may change the preceding
## part of this file as well if you like, but it should rarely be
## necessary ...
##

# When compiling on the target machine itself, change this to the
# relevant target when cross-compiling for another architecture
VASP_TARGET_CPU ?= -xHOST
FFLAGS     += $(VASP_TARGET_CPU)
 
# Intel MKL for FFTW, BLAS, LAPACK, and scaLAPACK
# (Note: for Intel Parallel Studio's MKL use -mkl instead of -qmkl)
FCL        += -qmkl
MKLROOT    ?= /opt/intel/oneapi/mkl/2025.2
LLIBS      += -L$(MKLROOT)/lib/intel64 -lmkl_scalapack_lp64 -lmkl_blacs_intelmpi_lp64
INCS        =-I$(MKLROOT)/include/fftw

# Use a separate scaLAPACK installation (optional but recommended in combination with OpenMPI)
# Comment out the two lines below if you want to use scaLAPACK from MKL instead
# SCALAPACK_ROOT ?= /path/to/your/scalapack/installation
# LLIBS      += -L${SCALAPACK_ROOT}/lib -lscalapack

# HDF5-support (optional but strongly recommended, and mandatory for some features)
CPP_OPTIONS+= -DVASP_HDF5
HDF5_ROOT  ?= /home/elvis/Programs/hdf5-1.14.6-intel/build
LLIBS      += -L$(HDF5_ROOT)/lib -lhdf5_fortran
INCS       += -I$(HDF5_ROOT)/include

# For the VASP-2-Wannier90 interface (optional)
#CPP_OPTIONS    += -DVASP2WANNIER90
#WANNIER90_ROOT ?= /path/to/your/wannier90/installation
#LLIBS          += -L$(WANNIER90_ROOT)/lib -lwannier

# For the fftlib library (hardly any benefit in combination with MKL's FFTs)
#CPP_OPTIONS+= -Dsysv
#FCL         = mpif90 fftlib.o -qmkl
#CXX_FFTLIB  = icpc -qopenmp -std=c++11 -DFFTLIB_USE_MKL -DFFTLIB_THREADSAFE
#INCS_FFTLIB = -I./include -I$(MKLROOT)/include/fftw
#LIBS       += fftlib

# For machine learning library vaspml (experimental)
#CPP_OPTIONS += -Dlibvaspml
#CPP_OPTIONS += -DVASPML_USE_CBLAS
#CPP_OPTIONS += -DVASPML_USE_MKL
#CPP_OPTIONS += -DVASPML_DEBUG_LEVEL=3
#CXX_ML      = mpic++ -qopenmp
#CXXFLAGS_ML = -O3 -std=c++17 -Wall
#INCLUDE_ML  =
```

- Instale usando o comando
```
make DEPS=1 -j4
```

- Temos então os programas `vasp_std`,`vasp_gam`e `vasp_ncl` na pasta `/bin`. Modifique os nomes dos executáveis do VASP que usarão CPU
```bash
mv bin/vasp_std bin/vasp_std_intel
```

```bash
mv bin/vasp_gam bin/vasp_gam_intel
```

```bash
mv bin/vasp_ncl bin/vasp_ncl_intel
```

- Devemos adicionar um PATH ao `.bashrc` modificando o arquivo e adicionando as linhas abaixo
```bash
# VASP
export PATH=/home/elvis/Programs/vasp.6.5.1/bin:$PATH
```

- Atualize sue ambiente 
```bash
source ~/.bashrc
```

- Para rodar qualquer programa usando o vasp você deve usar o comando
```bash
mpirun -np 4 vasp_std_intel
```

- Para rodar com todos os processadores e threads use o comando 
```bash
mpirun --use-hwthread -np N vasp_std_intel
```

Pronto! Agora está pronto para rodar seus problemas usando o VASP com compilador Intel.

## 5. Instalando PseudoPotenciais

- Para os pseudopotenciais, crie duas pastas `pp/potpaw` e `pp/potpaw_PBE` dentro das pasta `vasp.6.5.1`
```bash
mkdir -p pp/potpaw && mkdir -p pp/potpaw_PBE
```

- Agora descompacte o arquivo `potpaw_LDA.64.tgz`
```bash
tar -xvzf potpaw_LDA.64.tgz -C pp/potpaw
```

- E o arquivo `potpaw_PBE.64.tgz` 
```bash
tar -xvzf potpaw_PBE.64.tgz -C pp/potpaw_PBE
```

