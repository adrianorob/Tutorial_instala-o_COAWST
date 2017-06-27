# Tutorial_instala-o_COAWST
Tutorial em Português instalação do modelo acoplado COAWST












MANUAL INSTALAÇÃO DO COAWST




LUIS FELIPE MENDONÇA

LFELIPEM@MSN.COM

















Versão 2.0
Março de 2016 
INTRODUÇÃO

O objetivo deste tutorial é ajudar novos usuários a instalar o modelo acoplado COAWST (desenvolvido por John Warner - USGS). A instalação garantirá que usuário possa rodar os casos teste e ter certeza que tudo está correto com a configuração do modelo.

Gostaria de enfatizar que o objetivo deste tutorial é ajudar de forma simples e objetiva, complementando o manual do COAWST, para que novos usuários brasileiros consigam acessar esta ferramenta numérica sensacional.

Inicialmente tenho duas considerações pessoais a fazer:

1 - Modelos baseados em linguagem FORTRAN, como os utilizados no COAWST (ROMS+WRF+SWAN), não são softwares de fácil instalação como estamos acostumados a utilizar diariamente em nossas máquinas. Compreender estes pacotes e sua linguagem demanda dedicação e seu sucesso está diretamente associado ao tempo que demandamos em cima dele. Logo, quanto mais você trabalhar com ele, maior será o seu conhecimento e domínio sobre o mesmo. Particularmente minha dica é que você abra uma cerveja e vá com calma.

2 – Por se tratar de um sistema “novo”, seus criadores ainda não o dominam completamente. Aversões do COAWST são atualizadas com muita frequência e as correções de erros são praticamente constantes. Pense que trabalhar com apenas um modelo é difícil, trabalhar com 3 é ainda mais. Não tenha medo nem vergonha de pedir ajuda, nem de procurar soluções no google. Os modelos ROMS e WRF são amplamente difundidos e possuem um suporte on line muito amplo e funcional.


Dica 1: Se possível comece com o Linux zerado (recém instalado), siga o passo a passo que está descrito no tutorial abaixo e deve dar tudo certo.

Lembre-se que as etapas são praticamente todas desenvolvidas via terminal do Linux.

Dica 2: Durante a compilação do modelo, passarão várias linhas de comando rapidamente pela tela. Minha dica é subir o Scroll do mouse e acompanhar vagarosamente os comandos executados a procura de erros. Assim é possível solucionar os problemas de configuração e compilação. Se você deixar os textos passarem rapidamente sem acompanhá-los, você não conseguirá seguir a evolução dos processos e consequentemente não consegui achar os erros.

Dica 3: Apareceu um erro??? Lembre-se: GOOGLE É MEU PASTOR E NADA ME

FALTARÁ.

Bom, chega de papo e vamos começar. 
1.	INSTALANDO OS COMPONENTES BÁSICOS

Crie uma pasta /Programas no seu home, é lá que baixaremos e descompactaremos todos os programas. Execute o comando abaixo com o nome de origem do seu home.

mkdir /home/luis/Programas

Para que o COAWST funcione corretamente precisamos instalar os seguinte pacotes: subversion, gfortran, g++, M4, make (já vem no ubuntu), perl (também já vem instalado), zlib, hdf5, netcdf para C, netcdf para fortran e finalmente baixar o COAWST.

Começamos com o básico, abrindo o Terminal e instalado o subversion, gfortra, g++ e o M4.

sudo apt-get install subversion

sudo apt-get install gfortran

sudo apt-get install g++

sudo apt-get M4

# o make deve ser 3.8 ou mais recente
make -v

# verifique se o perl está instalado
perl –v

1.1 - Instalando a biblioteca zlib:

1	cd ~/Programas

2	wget http://www.zlib.net/zlib-1.2.8.tar.gz

3	tar -xvzf zlib-1.2.8.tar.gz

4	cd zlib-1.2.8

5	./configure --prefix=/usr/local/

6	make

7	sudo make install
1.2 - Instalando a biblioteca hdf5:

9	cd ~/Programas

10	wget ftp://ftp.unidata.ucar.edu/pub/netcdf/netcdf-4/hdf5-1.8.9.tar.gz

11	tar xzf hdf5-1.8.9.tar.gz

12	cd hdf5*

13	./configure --with-zlib=/usr/local --prefix=/usr/local
14	make check install
15	sudo make install

1.3 - Agora instale o netcdf para C e depois para fortran:

1	cd ~/Programas

2	wget http://www.unidata.ucar.edu/downloads/netcdf/ftp/netcdf-4.4.0.tar.gz

3	tar xzf netcdf-4.4.0.tar.gz

4	cd netcdf*

5	./configure --prefix=/usr/local

6	make check

7	sudo make install


1.4 - Instalação netcdf Fortran

8	cd ~/Programas

9	wget http://www.unidata.ucar.edu/downloads/netcdf/ftp/netcdf-fortran-4.4.3.tar.gz

10	tar xzf netcdf-fortran-4.4.3.tar.gz

11	cd netcdf-fortran-4.4.3/

12	./configure --prefix=/usr/local

13	sudo make install




Agora vamos instalar o MPI (Message Passing Interface) para poder usar o COAWST em paralelo. Eu escolhi utilizar o Open-MPI.

1	cd /Programas

2	wget https://www.open-mpi.org/software/ompi/v1.10/downloads/openmpi-1.10.1.tar.gz

3	tar xvzf openmpi-1.10.1.tar.gz

4	cd openmpi-1.10.1

5	./configure –prefix=/usr/local

6	sudo make install

Se tudo deu certo até agora, vamos baixar a última versão do COAWST usando o subversion, mas para isso você precisa de uma conta no COAWST. Para adquiri-la mande um e-mail para (jcwarner@usgs.gov). Ao usar o svn será necessário usar seu usuário e senha. Escolha um local a seu gosto para instalar o COAWST. Minha dica é um ambiente que não necessite ROOT e seja de fácil acesso (ex: /home/luis/COAWST).

1	Download COAWST			
				
2	svn	checkout	--username	seuusername
	https://coawstmodel.sourcerepo.com/coawstmodel/COAWST	
		

Os pré-requitos básicos estão feitos, passamos a configuração do COAWST. Está etapa é apenas um acréscimo do que já foi descrito no manual do COAWST.

Gostaria de acrescentar que possuo algumas manias e atos que podem ser simplificados e feitos de maneira diferente, mas que para o correto funcionamento do modelo acho interessante serem seguidos, pelo menos em primeira estância.

 
2 – CONFIGURAÇÃO DO COAWST

Está etapa consiste em configurar o arquivo (.bashrc). Para os iniciantes em Linux o arquivo .bashrc é responsável por mostrar ao sistema raiz onde as pastas com os arquivos e bibliotecas serão chamados. O arquivo .bashrc encontra-se na pasta pessoal do usuário do Linux (Ex: /home/luis/.bashrc). Abrindo o arquivo no editor de texto do Linux (comando gedit .bashrc na sua pasta pessoal) deve abrir o arquivo de interesse. Desça ao fim do documento de texto e acrescente as seguintes linhas de comando:

COAWST_PATH=`coawst current directorie`

export MCT_INCDIR=${COAWST_PATH}/Lib/MCT/include export MCT_LIBDIR=${COAWST_PATH}/Lib/MCT/lib export NETCDF_INCDIR=/usr/local/include

export NETCDF_LIBDIR=/usr/local/lib export PATH=$PATH:/usr/local/bin export NETCDF=/usr/local

export JASPERLIB=/usr/local/lib export JASPERINC=/usr/local/include export PHDF5=/usr/local

export LD_LIBRARY_PATH="/usr/local/lib:/usr/local/lib/:${LD_LIBRARY_PATH}" export INCLUDE="/usr/local/include:/usr/local/include/:${INCLUDE}"

*****Lembre-se de mudar o nome da pais raiz, para a sua. (/home/luis/...)

Feche o editor de texto.

Para atualizar o .bashrc execute o seguinte comando:

source .bashrc

3 – CONFIGURAÇÃO DO MCT

Esta etapa é idêntica a do manual do COAWST, porém o Dr. Warner indica que copiemos as pastas mct/lib e mct/include para a pasta /usr/local. Eu não acho necessário este procedimento, então iremos direto ao ponto. Entre na pasta /COAWST/Lib/MCT e execute o seguinte comando:

./configure

Este comando irá gerar o arquivo Makefile.conf, configure-o da seguinte forma:

```

# COMPILER, LIBRARY, AND MACHINE MAKE VARIABLES

# FORTRAN COMPILER VARIABLES #

# FORTRAN COMPILER COMMAND
FC		= mpif90

# FORTRAN AND FORTRAN90 COMPILER FLAGS
FCFLAGS		= -O2  

# FORTRAN90 ONLY COMPILER FLAGS 
F90FLAGS        = 

# FORTRAN COMPILE FLAG FOR AUTOPROMOTION 
# OF NATIVE REAL TO 8 BIT REAL
REAL8           = -r8

# FORTRAN COMPILE FLAG FOR CHANGING BYTE ORDERING
ENDIAN          = -convert big_endian

# INCLUDE FLAG FOR LOCATING MODULES (-I, -M, or -p)
INCFLAG         = -I

# INCLUDE PATHS (PREPEND INCLUDE FLAGS -I, -M or -p)
INCPATH         = -I/usr/local/include -I/home/luis/COAWST/Lib/MCT/mpeu

# MPI LIBRARIES (USUALLY -lmpi)
MPILIBS         = /usr/local/lib

# PREPROCESSOR VARIABLES #

# COMPILER AND OS DEFINE FLAGS
DEFS            = -DSYSLINUX -DCPRGNU

# SET A SEPARATE PREPROCESSOR COMMAND IF FORTRAN COMPILER 
# DOES NOT HANDLE PREPROCESSING OF SOURCE FILES

# FORTRAN PREPROCESSOR COMMAND
FPP		= cpp

# FOTRAN PREPROCESSOR FLAGS
FPPFLAGS	= -P -C -N -traditional -I/usr/local/include

# C COMPILER VARIABLES #

# C COMPILER
CC		= cc

# C COMPILER FLAGS - APPEND CFLAGS
ALLCFLAGS       = -DFORTRAN_UNDERSCORE_ -DSYSLINUX -DCPRGNU -O 

# LIBRARY SPECIFIC VARIABLES #

# USED BY MCT BABEL BINDINGS
COMPILER_ROOT = 
BABELROOT     = 
PYTHON        = 
PYTHONOPTS    = 

# USED BY MPI-SERIAL LIBRARY

# SIZE OF FORTRAN REAL AND DOUBLE
FORT_SIZE = 

# SOURCE FILE TARGETS #

# ENABLE RULE FOR PROCESSING C SOURCE FILES
# BY SETTING TO .c.o
CRULE           = .c.o

# ENABLE RULE FOR PROCESSING F90 SOURCE FILES 
# BY SETTING TO .F90.o AND DISABLING F90RULECPP
F90RULE         = .F90.o

# ENABLE RULE FOR PROCESSING F90 SOURCE FILES
# WITH SEPARATE PREPROCESSING INVOCATION
# BY SETTING TO .F90.o AND DISABLING F90RULE
F90RULECPP      = .F90RULECPP

# INSTALLATION VARIABLES #

# INSTALL COMMANDS
INSTALL         = /home/luis/COAWST/Lib/MCT/install-sh -c
MKINSTALLDIRS   = /home/luis/COAWST/Lib/MCT/mkinstalldirs

# INSTALLATION DIRECTORIES
abs_top_builddir= /home/luis/COAWST/Lib/MCT
MCTPATH         = /home/luis/COAWST/Lib/MCT/mct
MPEUPATH        = /home/luis/COAWST/Lib/MCT/mpeu
EXAMPLEPATH     = /home/luis/COAWST/Lib/MCT/examples
MPISERPATH      = 
libdir          = /home/luis/COAWST/Lib/MCT/lib
includedir      = /home/luis/COAWST/Lib/MCT/include

# OTHER COMMANDS #
AR		= ar cq
RM		= rm -f
```

Feito isto, execute o comando:

make

make install


4 – CONFIGURANDO O COMPILERS

Antes de começarmos a configurar o arquivo bash do COAWST, vamos configurar o compilador. Entre na pasta COAWST/Compilers e procure o arquivo Linux-gfortran.mk. Abra-o e configure da seguinte maneira:

```
# svn $Id: Linux-gfortran.mk 655 2008-07-25 18:57:05Z arango $
#::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
# Copyright (c) 2002-2010 The ROMS/TOMS Group                           :::
#   Licensed under a MIT/X style license                                :::
#   See License_ROMS.txt                                                :::
#::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
#
# Include file for GNU Fortran compiler on Linux
# -------------------------------------------------------------------------
#
# ARPACK_LIBDIR  ARPACK libary directory
# FC             Name of the fortran compiler to use
# FFLAGS         Flags to the fortran compiler
# CPP            Name of the C-preprocessor
# CPPFLAGS       Flags to the C-preprocessor
# NETCDF_INCDIR  NetCDF include directory
# NETCDF_LIBDIR  NetCDF libary directory
# LD             Program to load the objects into an executable
# LDFLAGS        Flags to the loader
# RANLIB         Name of ranlib command
# MDEPFLAGS      Flags for sfmakedepend  (-s if you keep .f files)
#
# First the defaults
#
               FC := gfortran
           FFLAGS := -frepack-arrays
              CPP := /usr/bin/cpp
         CPPFLAGS := -P -traditional
          LDFLAGS :=
               AR := ar
          ARFLAGS := -r
            MKDIR := mkdir -p
               RM := rm -f
           RANLIB := ranlib
             PERL := perl
             TEST := test

        MDEPFLAGS := --cpp --fext=f90 --file=- --objdir=$(SCRATCH_DIR)

#
# Library locations, can be overridden by environment variables.
#

             LIBS :=
ifdef USE_NETCDF4
   NETCDF_INCDIR ?= /usr/local/include
   NETCDF_LIBDIR ?= /usr/local/lib
     HDF5_LIBDIR ?= /usr/local/lib
else
   NETCDF_INCDIR ?= /usr/local/include
   NETCDF_LIBDIR ?= /usr/local/lib
endif
#           LIBS += $(shell $(NC_CONFIG) --flibs)
            LIBS += -L$(NETCDF_LIBDIR) -lnetcdff -lnetcdf
ifdef USE_NETCDF4
            LIBS += -L$(HDF5_LIBDIR) -lhdf5_hl -lhdf5 -lz
endif

ifdef USE_ARPACK
    ARPACK_LIBDIR ?= /usr/local/lib
             LIBS += -L$(ARPACK_LIBDIR) -larpack
endif

ifdef USE_MPI
         CPPFLAGS += -DMPI
 ifdef USE_MPIF90
               FC := mpif90
 else
             LIBS += -lfmpi -lmpi
 endif
endif

ifdef USE_OpenMP
         CPPFLAGS += -D_OPENMP
           FFLAGS += -fopenmp
endif

ifdef USE_DEBUG
           FFLAGS += -g -fbounds-check
else
           FFLAGS += -O3 -ffast-math
	   FFLAGS +=  -ftree-vectorize -ftree-loop-linear -funroll-loops -w -ffree-form -ffree-line-length-none -fconvert=big-endian -frecord-marker=4
endif

ifdef USE_MCT
       MCT_INCDIR ?= /home/lfelipem/COAWST/Lib/MCT/include
       MCT_LIBDIR ?= /home/luis/COAWST/Lib/MCT/lib
           FFLAGS += -I$(MCT_INCDIR)
             LIBS += -L$(MCT_LIBDIR) -lmct -lmpeu
endif

ifdef USE_ESMF
      ESMF_SUBDIR := $(ESMF_OS).$(ESMF_COMPILER).$(ESMF_ABI).$(ESMF_COMM).$(ESMF_SITE)
      ESMF_MK_DIR ?= $(ESMF_DIR)/lib/lib$(ESMF_BOPT)/$(ESMF_SUBDIR)
                     include $(ESMF_MK_DIR)/esmf.mk
           FFLAGS += $(ESMF_F90COMPILEPATHS)
             LIBS += $(ESMF_F90LINKPATHS) -lesmf -lC
endif

ifdef USE_WRF
           FFLAGS += -I$(MCT_INCDIR) -I../WRF/main -I../WRF/external/esmf_time_f90
             LIBS += -L$(MCT_LIBDIR) -lmct -lmpeu
             LIBS += WRF/main/module_wrf_top.o
             LIBS += WRF/main/libwrflib.a
             LIBS += WRF/external/fftpack/fftpack5/libfftpack.a
             LIBS += WRF/external/io_grib1/libio_grib1.a
             LIBS += WRF/external/io_grib_share/libio_grib_share.a
             LIBS += WRF/external/io_int/libwrfio_int.a
             LIBS += WRF/external/esmf_time_f90/libesmf_time.a
             LIBS += WRF/external/RSL_LITE/librsl_lite.a
             LIBS += WRF/frame/module_internal_header_util.o
             LIBS += WRF/frame/pack_utils.o
             LIBS += WRF/external/io_netcdf/libwrfio_nf.a
#            LIBS += WRF/external/io_netcdf/wrf_io.o
endif

#
# Use full path of compiler.
#
               FC := $(shell which ${FC})
               LD := $(FC)

#
# Turn off bounds checking for function def_var, as "dimension(*)"
# declarations confuse Gnu Fortran 95 bounds-checking code.
#

$(SCRATCH_DIR)/def_var.o: FFLAGS += -fno-bounds-check

#
# Allow integer overflow in ran_state.F.  This is not allowed
# during -O3 optimization. This option should be applied only for
# Gfortran versions >= 4.2.
#

FC_TEST := $(findstring $(shell ${FC} --version | head -1 | cut -d " " -f 5 | \
                              cut -d "." -f 1-2),4.0 4.1)

ifeq "${FC_TEST}" ""
$(SCRATCH_DIR)/ran_state.o: FFLAGS += -fno-strict-overflow
endif

#
# Set free form format in source files to allow long string for
# local directory and compilation flags inside the code.
#

$(SCRATCH_DIR)/mod_ncparam.o: FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/mod_strings.o: FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/analytical.o: FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/biology.o: FFLAGS += -ffree-form -ffree-line-length-none
ifdef USE_ADJOINT
$(SCRATCH_DIR)/ad_biology.o: FFLAGS += -ffree-form -ffree-line-length-none
endif
ifdef USE_REPRESENTER
$(SCRATCH_DIR)/rp_biology.o: FFLAGS += -ffree-form -ffree-line-length-none
endif
ifdef USE_TANGENT
$(SCRATCH_DIR)/tl_biology.o: FFLAGS += -ffree-form -ffree-line-length-none
endif

#
# Supress free format in SWAN source files since there are comments
# beyond column 72.
#

ifdef USE_SWAN

$(SCRATCH_DIR)/ocpcre.o:   FFLAGS += -ffixed-form
$(SCRATCH_DIR)/ocpids.o:   FFLAGS += -ffixed-form
$(SCRATCH_DIR)/ocpmix.o:   FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swancom1.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swancom2.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swancom3.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swancom4.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swancom5.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swanmain.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swanout1.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swanout2.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swanparll.o: FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swanpre1.o:  FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swanpre2.o:  FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swanser.o:   FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swmod1.o:    FFLAGS += -ffixed-form
$(SCRATCH_DIR)/swmod2.o:    FFLAGS += -ffixed-form
$(SCRATCH_DIR)/m_constants.o:           FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/m_fileio.o:              FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/mod_xnl4v5.o:            FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/serv_xnl4v5.o:           FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/SwanCompUnstruc.o:       FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/SwanInterpolateAc.o:     FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/SwanInterpolateOutput.o: FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/SwanPropvelS.o:          FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/waves_control.o:         FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/SwanConvStopc.o:         FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/SwanCompdata.o: FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/SwanGriddata.o: FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/nctablemd.o:    FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/agioncmd.o:     FFLAGS += -ffree-form -ffree-line-length-none
$(SCRATCH_DIR)/swn_outnc.o:    FFLAGS += -ffree-form -ffree-line-length-none

endif
```
Dica: Lembre-se que esta é a configuração para o meu notebook e as pastas estão setadas para os meus destinos virtuais, não tente dar uma de malandro e copiar as minhas configurações que o seu computador não vai achar as origens. 

5 – CONFIGURANDO O WRF

Eu, diferentemente do John, gosto de configurar o WRF sempre antes de compilar o COAWST. Então se você quiser fazer como eu siga sempre estas etapas.

Abra a pasta COAWST/WRF.

Execute o comando:

./clean –a (este comando irá limpar todas as suas configurações anteriores).

./configure (este comando irá gerar o arquivo configure.wrf)

Execute o editor de texto do Linux:

gedit configure.wrf

Neste arquivo devemos mudar apenas duas coisas muito importantes e fundamentais para que o modelo funcione.

1 – Configure a Flag MCT_LIBDIR para a sua pasta lib do MCT.

MCT_LIBDIR ?= /home/luis/COAWST/Lib/MCT/Lib

2- Essa é uma dica minha para o John que ele precisa colocar em seu tutorial, 90% dos problemas dos usuários não conseguirem compilar o COAWT está associado a este comando. Se você usa OpenMPI deve ser acrescentado a FLAG “DM_CC” a linha de comando - DMPI2_SUPPORT .

DM_CC	=	mpicc -DMPI2_SUPPORT


6 – CONFIGURANDO O BASH

Passamos a etapa de configuração do arquivo coaswt.bash. Segue abaixo a copia do meu arquivo para que vocês possam configurar de acordo com as suas pastas.
```
#	
#	#!/bin/bash
#	#
#	# svn $Id: build.bash 429 2009-12-20 17:30:26Z jcwarner $
#	#::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
#	# Copyright (c) 2002-2010 The ROMS/TOMS Group                           :::
#	#   Licensed under a MIT/X style license                                :::
#	#   See License_ROMS.txt                                                :::
#	#::::::::::::::::::::::::::::::::::::::::::::::::::::: Hernan G. Arango :::
#	#::::::::::::::::::::::::::::::::::::::::::::::::::::: John C. Warner   :::
#	#                                                                       :::
#	# ROMS/TOMS Compiling Script                                            :::
#	# Modified to configure the COAWST Modeling System                      :::
#	#                                                                       :::
#	# Script to identify locations of application-specific files for        :::
#	# compiling the modeling sytem.                                         :::
#	#                                                                       :::
#	# Q: How/why does this script work?                                     :::
#	#                                                                       :::
#	# A: The ROMS makefile configures user-defined options with a set of    :::
#	#    flags such as ROMS_APPLICATION. Browse the makefile to see these.  :::
#	#    If an option in the makefile uses the syntax ?= in setting the     :::
#	#    default, this means that make will check whether an environment    :::
#	#    variable by that name is set in the shell that calls make. If so   :::
#	#    the environment variable value overrides the default (and the      :::
#	#    user need not maintain separate makefiles, or frequently edit      :::
#	#    the makefile, to run separate applications).                       :::
#	#                                                                       :::
#	# Usage:                                                                :::
#	#                                                                       :::
#	#    ./build.bash [options]                                             :::
#	#                                                                       :::
#	# Options:                                                              :::
#	#                                                                       :::
#	#    -j [N]       Compile in parallel using N CPUs                      :::
#	#                  omit argument for all available CPUs                 :::
#	#    -noclean     Do not clean already compiled roms objects            :::
#	#    -nocleanwrf  Do not clean already compiled wrf objects             :::
#	#                                                                       :::
#	# Notice that sometimes the parallel compilation fail to find MPI       :::
#	# include file "mpif.h".                                                :::
#	#                                                                       :::
#	#::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
#	
#	parallel=0
#	clean=1
#	cleanwrf=1
#	
#	while [ $# -gt 0 ]
#	do
#	  case "$1" in
#	    -j )
#	      shift
#	      parallel=1
#	      test=`echo $1 | grep -P '^\d+$'`
#	      if [ "$test" != "" ]; then
#	        NCPUS="-j $1"
#	        shift
#	      else
#	        NCPUS="-j"
#	      fi
#	      ;;
#	
#	    -noclean )
#	      shift
#	      clean=0
#	      ;;
#	
#	    -nocleanwrf )
#	      shift
#	      cleanwrf=0
#	      ;;
#	
#	    * )
#	      echo ""
#	      echo "$0 : Unknown option [ $1 ]"
#	      echo ""
#	      echo "Available Options:"
#	      echo ""
#	      echo "-j [N]      Compile in parallel using N CPUs"
#	      echo "              omit argument for all avaliable CPUs"
#	      echo "-noclean       Do not clean already compiled objects"
#	      echo "-nocleanwrf    Do not clean already compiled wrf objects"
#	      echo ""
#	      exit 1
#	      ;;
#	  esac
#	done
#	
#	# Set the CPP option defining the particular application. This will
#	# determine the name of the ".h" header file with the application
#	# CPP definitions.
#	
#	export   ROMS_APPLICATION=JOE_TC
#	
#	# Set number of Nested grids for ROMS and or SWAN. This feature is activated
#	# with the cpp option REFINED_GRID. If you are using both ROMS and SWAN, 
#	# you need to have the same number of grids for both models.
#	
#	export     NestedGrids=1
#	
#	# Set a local environmental variable to define the path to the directories
#	# where all this project's files are kept.
#	
#	export     MY_ROOT_DIR=/home/luis/COAWST
#	export     MY_PROJECT_DIR=${MY_ROOT_DIR}
#	
#	# The path to the user's local current ROMS source code.
#	#
#	# If using svn locally, this would be the user's Working Copy Path (WCPATH).
#	# Note that one advantage of maintaining your source code locally with svn
#	# is that when working simultaneously on multiple machines (e.g. a local
#	# workstation, a local cluster and a remote supercomputer) you can checkout
#	# the latest release and always get an up-to-date customized source on each
#	# machine. This script is designed to more easily allow for differing paths
#	# to the code and inputs on differing machines.
#	
#	export        MY_ROMS_SRC=${MY_ROOT_DIR}/
#	
#	# Set path of the directory containing makefile configuration (*.mk) files.
#	# The user has the option to specify a customized version of these files
#	# in a different directory than the one distributed with the source code,
#	# ${MY_ROMS_SCR}/Compilers. If this is the case, the you need to keep
#	# these configurations files up-to-date.
#	
#	export         COMPILERS=${MY_ROMS_SRC}/Compilers
#	
#	# Set tunable CPP options.
#	#
#	# Sometimes it is desirable to activate one or more CPP options to run
#	# different variants of the same application without modifying its header
#	# file. If this is the case, specify each options here using the -D syntax.
#	# Notice also that you need to use shell's quoting syntax to enclose the
#	# definition.  Both single or double quotes works. For example, to write
#	# time-averaged fields set:
#	#
#	#export      MY_CPP_FLAGS="-DAVERAGES"
#	
#	# Other user defined environmental variables. See the ROMS makefile for
#	# details on other options the user might want to set here. Be sure to
#	# leave the switched meant to be off set to an empty string or commented
#	# out. Any string value (including off) will evaluate to TRUE in
#	# conditional if-stamentents.
#	
#	 export           USE_MPI=on
#	 export        USE_MPIF90=on
#	#export              FORT=ifort
#	 export              FORT=gfortran
#	#export              FORT=pgi
#	
#	#export        USE_OpenMP=on
#	
#	 export         USE_DEBUG=
#	 export         USE_LARGE=on
#	#export       USE_NETCDF4=on
#	
#	# There are several MPI libraries out there. The user can select here the
#	# appropriate "mpif90" script to compile, provided that the makefile
#	# macro file (say, Linux-pgi.mk) in the Compilers directory has:
#	#
#	               FC := mpif90
#	#
#	# "mpif90" defined without any path. Recall that you still need to use the
#	# appropriate "mpirun" to execute. Also notice that the path where the
#	# MPI library is installed is computer dependent.
#	
#	if [ -n "${USE_MPIF90:+1}" ]; then
#	  case "$FORT" in
#	    ifort )
#	#     export PATH=/opt/intelsoft/mpich/bin:$PATH
#	      export PATH=/opt/intelsoft/mpich2/bin:$PATH
#	#     export PATH=/opt/intelsoft/openmpi/bin:$PATH
#	      ;;
#	
#	    pgi )
#	#     export PATH=/opt/pgisoft/mpich/bin:$PATH
#	      export PATH=/opt/pgisoft/mpich2/bin:$PATH
#	#     export PATH=/opt/pgisoft/openmpi/bin:$PATH
#	      ;;
#	
#	    gfortran )
#	#     export PATH=/usr/local/bin:$PATH
#	      export PATH=/usr/bin:$PATH
#	      ;;
#	
#	    g95 )
#	#     export PATH=/opt/g95soft/mpich2/bin:$PATH
#	      export PATH=/opt/g95soft/openmpi/bin:$PATH
#	      ;;
#	
#	  esac
#	fi
#	
#	# The path of the libraries required by ROMS can be set here using
#	# environmental variables which take precedence to the values
#	# specified in the makefile macro definitions file (Compilers/*.mk).
#	# If so desired, uncomment the local USE_MY_LIBS definition below
#	# and edit the paths to your values. For most applications, only
#	# the location of the NetCDF library (NETCDF_LIBDIR) and include
#	# directorry (NETCDF_INCDIR) are needed!
#	#
#	# Notice that when the USE_NETCDF4 macro is activated, we need a
#	# serial and parallel version of the NetCDF-4/HDF5 library. The
#	# parallel library uses parallel I/O through MPI-I/O so we need
#	# compile also with the MPI library. This is fine in ROMS
#	# distributed-memory applications.  However, in serial or
#	# shared-memory ROMS applications we need to use the serial
#	# version of the NetCDF-4/HDF5 to avoid conflicts with the
#	# compiler. Recall also that the MPI library comes in several
#	# flavors: MPICH, MPICH2, OpenMPI.
#	
#	#export           USE_MY_LIBS=on
#	
#	if [ -n "${USE_MY_LIBS:+1}" ]; then
#	  case "$FORT" in
#	    ifort )
#	      export           ESMF_DIR=/opt/intelsoft/esmf-3.1.0
#	      export            ESMF_OS=Linux
#	      export      ESMF_COMPILER=ifort
#	      export          ESMF_BOPT=O
#	      export           ESMF_ABI=64
#	      export          ESMF_COMM=mpich
#	      export          ESMF_SITE=default
#	      export         MCT_INCDIR=/opt/intelsoft/mct/include
#	      export         MCT_LIBDIR=/opt/intelsoft/mct/lib
#	
#	      export      ARPACK_LIBDIR=/opt/intelsoft/serial/ARPACK
#	      if [ -n "${USE_MPI:+1}" ]; then
#	#       export   PARPACK_LIBDIR=/opt/intelsoft/mpich/PARPACK
#	        export   PARPACK_LIBDIR=/opt/intelsoft/mpich2/PARPACK
#	#       export   PARPACK_LIBDIR=/opt/intelsoft/openmpi/PARPACK
#	      fi
#	
#	      if [ -n "${USE_NETCDF4:+1}" ]; then
#	        if [ -n "${USE_MPI:+1}" ]; then
#	#         export  NETCDF_INCDIR=/opt/intelsoft/mpich/netcdf4/include
#	#         export  NETCDF_LIBDIR=/opt/intelsoft/mpich/netcdf4/lib
#	#         export    HDF5_LIBDIR=/opt/intelsoft/mpich/hdf5/lib
#	
#	          export  NETCDF_INCDIR=/opt/intelsoft/mpich2/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/intelsoft/mpich2/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/intelsoft/mpich2/hdf5/lib
#	
#	#         export  NETCDF_INCDIR=/opt/intelsoft/openmpi/netcdf4/include
#	#         export  NETCDF_LIBDIR=/opt/intelsoft/openmpi/netcdf4/lib
#	#         export    HDF5_LIBDIR=/opt/intelsoft/openmpi/hdf5/lib
#	        else
#	          export  NETCDF_INCDIR=/opt/intelsoft/serial/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/intelsoft/serial/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/intelsoft/serial/hdf5/lib
#	        fi
#	      else
#	        export    NETCDF_INCDIR=/opt/intelsoft/serial/netcdf3/include
#	        export    NETCDF_LIBDIR=/opt/intelsoft/serial/netcdf3/lib
#	      fi
#	      ;;
#	
#	    pgi )
#	      export           ESMF_DIR=/opt/pgisoft/esmf-3.1.0
#	      export            ESMF_OS=Linux
#	      export      ESMF_COMPILER=pgi
#	      export          ESMF_BOPT=O
#	      export           ESMF_ABI=64
#	      export          ESMF_COMM=mpich
#	      export          ESMF_SITE=default
#	      export         MCT_INCDIR=/opt/pgisoft/mct/include
#	      export         MCT_LIBDIR=/opt/pgisoft/mct/lib
#	
#	      export      ARPACK_LIBDIR=/opt/pgisoft/serial/ARPACK
#	      if [ -n "${USE_MPI:+1}" ]; then
#	        export   PARPACK_LIBDIR=/opt/pgisoft/mpich/PARPACK
#	#       export   PARPACK_LIBDIR=/opt/pgisoft/mpich2/PARPACK
#	#       export   PARPACK_LIBDIR=/opt/pgisoft/openmpi/PARPACK
#	      fi
#	
#	      if [ -n "${USE_NETCDF4:+1}" ]; then
#	        if [ -n "${USE_MPI:+1}" ]; then
#	#         export  NETCDF_INCDIR=/opt/pgisoft/mpich/netcdf4/include
#	#         export  NETCDF_LIBDIR=/opt/pgisoft/mpich/netcdf4/lib
#	#         export    HDF5_LIBDIR=/opt/pgisoft/mpich/hdf5/lib
#	
#	          export  NETCDF_INCDIR=/opt/pgisoft/mpich2/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/pgisoft/mpich2/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/pgisoft/mpich2/hdf5/lib
#	
#	#         export  NETCDF_INCDIR=/opt/pgisoft/openmpi/netcdf4/include
#	#         export  NETCDF_LIBDIR=/opt/pgisoft/openmpi/netcdf4/lib
#	#         export    HDF5_LIBDIR=/opt/pgisoft/openmpi/hdf5/lib
#	        else
#	          export  NETCDF_INCDIR=/opt/pgisoft/serial/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/pgisoft/serial/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/pgisoft/serial/hdf5/lib
#	        fi
#	      else
#	        export    NETCDF_INCDIR=/opt/pgisoft/serial/netcdf3/include
#	        export    NETCDF_LIBDIR=/opt/pgisoft/serial/netcdf3/lib
#	      fi
#	      ;;
#	
#	    gfortran )
#	      export         MCT_INCDIR=/opt/gfortransoft/mct/include
#	      export         MCT_LIBDIR=/opt/gfortransoft/mct/lib
#	
#	      export      ARPACK_LIBDIR=/opt/gfortransoft/serial/ARPACK
#	      if [ -n "${USE_MPI:+1}" ]; then
#	#       export   PARPACK_LIBDIR=/opt/gfortransoft/mpich/PARPACK
#	        export   PARPACK_LIBDIR=/opt/gfortransoft/mpich2/PARPACK
#	#       export   PARPACK_LIBDIR=/opt/gfortransoft/openmpi/PARPACK
#	      fi
#	
#	      if [ -n "${USE_NETCDF4:+1}" ]; then
#	        if [ -n "${USE_MPI:+1}" ]; then
#	#         export  NETCDF_INCDIR=/opt/gfortransoft/mpich/netcdf4/include
#	#         export  NETCDF_LIBDIR=/opt/gfortransoft/mpich/netcdf4/lib
#	#         export    HDF5_LIBDIR=/opt/gfortransoft/mpich/hdf5/lib
#	
#	          export  NETCDF_INCDIR=/opt/gfortransoft/mpich2/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/gfortransoft/mpich2/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/gfortransoft/mpich2/hdf5/lib
#	
#	#         export  NETCDF_INCDIR=/opt/gfortransoft/openmpi/netcdf4/include
#	#         export  NETCDF_LIBDIR=/opt/gfortransoft/openmpi/netcdf4/lib
#	#         export    HDF5_LIBDIR=/opt/gfortransoft/openmpi/hdf5/lib
#	        else
#	          export  NETCDF_INCDIR=/opt/gfortransoft/serial/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/gfortransoft/serial/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/gfortransoft/serial/hdf5/lib
#	        fi
#	      else
#	          export  NETCDF_INCDIR=/opt/gfortransoft/serial/netcdf3/include
#	          export  NETCDF_LIBDIR=/opt/gfortransoft/serial/netcdf3/lib
#	      fi
#	      ;;
#	
#	    g95 )
#	      export         MCT_INCDIR=/opt/g95soft/mct/include
#	      export         MCT_LIBDIR=/opt/g95soft/mct/lib
#	
#	      export      ARPACK_LIBDIR=/opt/g95soft/serial/ARPACK
#	      if [ -n "${USE_MPI:+1}" ]; then
#	#       export   PARPACK_LIBDIR=/opt/g95soft/mpich/PARPACK
#	        export   PARPACK_LIBDIR=/opt/g95soft/mpich2/PARPACK
#	        export   PARPACK_LIBDIR=/opt/g95soft/openmpi/PARPACK
#	      fi
#	
#	      if [ -n "${USE_NETCDF4:+1}" ]; then
#	        if [ -n "${USE_MPI:+1}" ]; then
#	          export  NETCDF_INCDIR=/opt/g95soft/mpich2/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/g95soft/mpich2/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/g95soft/mpich2/hdf5/lib
#	
#	#         export  NETCDF_INCDIR=/opt/g95soft/openmpi/netcdf4/include
#	#         export  NETCDF_LIBDIR=/opt/g95soft/openmpi/netcdf4/lib
#	#         export    HDF5_LIBDIR=/opt/g95soft/openmpi/hdf5/lib
#	        else
#	          export  NETCDF_INCDIR=/opt/g95soft/serial/netcdf4/include
#	          export  NETCDF_LIBDIR=/opt/g95soft/serial/netcdf4/lib
#	          export    HDF5_LIBDIR=/opt/g95soft/serial/hdf5/lib
#	        fi
#	      else
#	        export    NETCDF_INCDIR=/opt/g95soft/serial/netcdf3/include
#	        export    NETCDF_LIBDIR=/opt/g95soft/serial/netcdf3/lib
#	      fi
#	      ;;
#	
#	  esac
#	fi
#	
#	# The rest of this script sets the path to the users header file and
#	# analytical source files, if any. See the templates in User/Functionals.
#	#
#	# If applicable, use the MY_ANALYTICAL_DIR directory to place your
#	# customized biology model header file (like fennel.h, nemuro.h, ecosim.h,
#	# etc).
#	
#	  export     MY_HEADER_DIR=${MY_PROJECT_DIR}/Projects/JOE_TCs
#	  export MY_ANALYTICAL_DIR=${MY_PROJECT_DIR}/Projects/JOE_TCs
#	
#	# Put the binary to execute in the following directory.
#	
#	# export            BINDIR=${MY_PROJECT_DIR}
#	  export            BINDIR=./
#	
#	# Put the f90 files in a project specific Build directory to avoid conflict
#	# with other projects.
#	
#	# export       SCRATCH_DIR=${MY_PROJECT_DIR}/Build
#	  export       SCRATCH_DIR=./Build
#	
#	# Go to the users source directory to compile. The options set above will
#	# pick up the application-specific code from the appropriate place.
#	
#	 cd ${MY_ROMS_SRC}
#	
#	# Remove build directory.
#	
#	if [ $clean -eq 1 ]; then
#	  make clean
#	fi
#	
#	# Compile (the binary will go to BINDIR set above).
#	
#	  export WRF_DIR=${MY_ROMS_SRC}/WRF
#	if [ $cleanwrf -eq 1 ]; then
#	  make wrfclean
#	  cd ${MY_ROMS_SRC}
#	fi
#	make wrf
#	
#	if [ $parallel -eq 1 ]; then
#	  make $NCPUS
#	else
#	  make
#	fi
#	export NETCDF_LIBDIR=/opt/pgisoft/mpich/netcdf4/lib

#	export   HDF5_LIBDIR=/opt/pgisoft/mpich/hdf5/lib

export NETCDF_INCDIR=/opt/pgisoft/mpich2/netcdf4/include export NETCDF_LIBDIR=/opt/pgisoft/mpich2/netcdf4/lib export HDF5_LIBDIR=/opt/pgisoft/mpich2/hdf5/lib

#	export NETCDF_INCDIR=/opt/pgisoft/openmpi/netcdf4/include

#	export NETCDF_LIBDIR=/opt/pgisoft/openmpi/netcdf4/lib
#	export   HDF5_LIBDIR=/opt/pgisoft/openmpi/hdf5/lib
else

export NETCDF_INCDIR=/opt/pgisoft/serial/netcdf4/include export NETCDF_LIBDIR=/opt/pgisoft/serial/netcdf4/lib export HDF5_LIBDIR=/opt/pgisoft/serial/hdf5/lib

fi else

export NETCDF_INCDIR=/opt/pgisoft/serial/netcdf3/include export NETCDF_LIBDIR=/opt/pgisoft/serial/netcdf3/lib

fi
;;

gfortran )	
export	MCT_INCDIR=/opt/gfortransoft/mct/include
export	MCT_LIBDIR=/opt/gfortransoft/mct/lib
export	ARPACK_LIBDIR=/opt/gfortransoft/serial/ARPACK
if [ -n "${USE_MPI:+1}" ]; then

#	export PARPACK_LIBDIR=/opt/gfortransoft/mpich/PARPACK export PARPACK_LIBDIR=/opt/gfortransoft/mpich2/PARPACK
#	export  PARPACK_LIBDIR=/opt/gfortransoft/openmpi/PARPACK

fi

if [ -n "${USE_NETCDF4:+1}" ]; then if [ -n "${USE_MPI:+1}" ]; then
#	export NETCDF_INCDIR=/opt/gfortransoft/mpich/netcdf4/include

#	export NETCDF_LIBDIR=/opt/gfortransoft/mpich/netcdf4/lib

#	export   HDF5_LIBDIR=/opt/gfortransoft/mpich/hdf5/lib

export NETCDF_INCDIR=/opt/gfortransoft/mpich2/netcdf4/include export NETCDF_LIBDIR=/opt/gfortransoft/mpich2/netcdf4/lib export HDF5_LIBDIR=/opt/gfortransoft/mpich2/hdf5/lib

#	export NETCDF_INCDIR=/opt/gfortransoft/openmpi/netcdf4/include

#	export NETCDF_LIBDIR=/opt/gfortransoft/openmpi/netcdf4/lib
#	export   HDF5_LIBDIR=/opt/gfortransoft/openmpi/hdf5/lib

else

export NETCDF_INCDIR=/opt/gfortransoft/serial/netcdf4/include export NETCDF_LIBDIR=/opt/gfortransoft/serial/netcdf4/lib export HDF5_LIBDIR=/opt/gfortransoft/serial/hdf5/lib

fi else

export NETCDF_INCDIR=/opt/gfortransoft/serial/netcdf3/include export NETCDF_LIBDIR=/opt/gfortransoft/serial/netcdf3/lib
fi
;;

g95 )
export	MCT_INCDIR=/opt/g95soft/mct/include

export	MCT_LIBDIR=/opt/g95soft/mct/lib

export	ARPACK_LIBDIR=/opt/g95soft/serial/ARPACK

if [ -n "${USE_MPI:+1}" ]; then
#	export	PARPACK_LIBDIR=/opt/g95soft/mpich/PARPACK

export	PARPACK_LIBDIR=/opt/g95soft/mpich2/PARPACK
export	PARPACK_LIBDIR=/opt/g95soft/openmpi/PARPACK
fi

if [ -n "${USE_NETCDF4:+1}" ]; then if [ -n "${USE_MPI:+1}" ]; then

export NETCDF_INCDIR=/opt/g95soft/mpich2/netcdf4/include export NETCDF_LIBDIR=/opt/g95soft/mpich2/netcdf4/lib export HDF5_LIBDIR=/opt/g95soft/mpich2/hdf5/lib

#	export NETCDF_INCDIR=/opt/g95soft/openmpi/netcdf4/include
#	export NETCDF_LIBDIR=/opt/g95soft/openmpi/netcdf4/lib

#	export   HDF5_LIBDIR=/opt/g95soft/openmpi/hdf5/lib
else

export NETCDF_INCDIR=/opt/g95soft/serial/netcdf4/include export NETCDF_LIBDIR=/opt/g95soft/serial/netcdf4/lib export HDF5_LIBDIR=/opt/g95soft/serial/hdf5/lib

fi else

export NETCDF_INCDIR=/opt/g95soft/serial/netcdf3/include export NETCDF_LIBDIR=/opt/g95soft/serial/netcdf3/lib
fi

;;

esac fi

#	The rest of this script sets the path to the users header file and

#	analytical source files, if any. See the templates in User/Functionals.

#	
#	If applicable, use the MY_ANALYTICAL_DIR directory to place your
#	customized biology model header file (like fennel.h, nemuro.h, ecosim.h,

#	etc).

export MY_HEADER_DIR=${MY_PROJECT_DIR}/Projects/JOE_TCs export MY_ANALYTICAL_DIR=${MY_PROJECT_DIR}/Projects/JOE_TCs

#	Put the binary to execute in the following directory.

#	export BINDIR=${MY_PROJECT_DIR} export BINDIR=./

#	Put the f90 files in a project specific Build directory to avoid conflict

#	with other projects.

#	exportSCRATCH_DIR=${MY_PROJECT_DIR}/Build
export	SCRATCH_DIR=./Build
#	Go to the users source directory to compile. The options set above will
#	pick up the application-specific code from the appropriate place.

cd ${MY_ROMS_SRC}

# Remove build directory.

if [ $clean -eq 1 ]; then make clean
fi

# Compile (the binary will go to BINDIR set above).

export WRF_DIR=${MY_ROMS_SRC}/WRF if [ $cleanwrf -eq 1 ]; then
make wrfclean

cd ${MY_ROMS_SRC} fi
make wrf

if [ $parallel -eq 1 ]; then make $NCPUS

else make
fi
```

7 – COMPILAR O COAWST.BASH

Após tudo configurado, passemos a etapa de compilação do modelo.

No terminal execute o seguinte comando:

./coawst.bash –nocleanwrf

Aguarde o final da compilação e veja se o arquivo coawstM foi gerado.

8 – RODAR O COAWSTM

Se tudo deu certo até aqui (e torço para que tenha dado) vamos rodar o coawstM com o mpirun e ver se os ventos estão a nosso favor.

Antes de executar o mpirun, entre na pasta /COAWST/Projects/JOE_TCs e copie os arquivos wrfinput wrfbdy e o namelistinput. e cole-os na pasta raiz do COAWST.

Feito isto, execute a seguinte linha de comando:

mpirun –np 3 ./coawstM Projects/JOE_Ts/coupling_joe_tc.in

*** -np é o número de processadores utilizados para a compilação.

Se tudo der certo ele deve começar a rodar. E os resultados deverão ser gerados.

Espero que este tutorial seja útil, ele está em versão de atualização e estou aberto a sugestões e criticas para que eu possa melhorar as próximas versões.
