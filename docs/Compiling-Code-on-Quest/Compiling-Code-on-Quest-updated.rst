Compiling Code on Quest
=======================

A introduction to compilations strategies on QUEST

.. container:: table-responsive

   +-----------------------------------------------------------------------+
   | Content                                                               |
   +-----------------------------------------------------------------------+
   | `1. Preamble <#Preamble>`__                                           |
   +-----------------------------------------------------------------------+
   | `2. Examples <#Examples>`__                                           |
   | `2.1 Makefile <#Makefile>`__                                          |
   | `2.2 Autoconf <#Autoconf>`__                                          |
   | `2.3 CMake <#CMake>`__                                                |
   | `2.4 Singularity <#Singularity>`__                                    |
   +-----------------------------------------------------------------------+

Basic Compiling Strategies
--------------------------

Quest users have the choice of GNU’s gcc/gfortran and Intel’s icc/ifort
compilers to compile their codes. In addition to being accessible as
modules, the GNU compilers are also provided by the operating system
(RedHat Enterprise Linux). These compilers are sometimes referred to as
native in the Quest documentation. More up-to-date versions are provided
as modules (see Modules on Quest). When attempting to compile code on
Quest, it is critical to follow these general steps

#. Identify an appropriate compiler
#. Identify external dependencies
#. Identify which libraries contain the external function calls we make
   from our program
#. Identify where these external dependencies live on the system

Compilers
~~~~~~~~~

-  GCC (The GNU Compiler Collection providing gcc and gfortran for
   compiling C and Fortran programs
-  Intel ® Cluster Toolkit (consisting of icc/ifort for compiling C and
   Fortran programs, as well as Intel MPI Library for compiling MPI
   parallelized codes)
-  MPI (Message Passing Interface) compilers such as OpenMPI and MPICH

See available versions of the compilers with

``module spider gcc``

``module spider intel``

``  module spider mpi``

Libraries/Dependencies
~~~~~~~~~~~~~~~~~~~~~~

After identifying the compiler that suites your needs, you must identify
what dependencies your program has, and where they can be found on
Quest. External dependencies can generally be found either on the system
(note recommended), by loading modules, or by installing these
dependencies yourself using a service like anaconda to create virtual
environments. A common pitfall when compiling code on a HPC cluster like
Quest, is accidentally linking against system libraries that may exist
on the log-in machines, but do not exist on the compute nodes where you
program actually runs.

Examples
--------

The example we will be using can be found
`here <https://raw.githubusercontent.com/CIERA-Northwestern/cmake_tutorial/enhanced_tutorial/Makefile/example.c>`__.
On Quest, you can run

``wget https://raw.githubusercontent.com/CIERA-Northwestern/cmake_tutorial/enhanced_tutorial/Makefile/example.c``

to get the example C code used for this documentation onto Quest. This C
program has the following dependencies.

-  An MPI compiler
-  HDF5 and HDF5_HL C Libraries
-  GSL C Libraries

Configuring a Makefile
~~~~~~~~~~~~~~~~~~~~~~

To make a Makefile, we will first construct a compilation command by
hand. To do this, we will follow the four steps outlined above, the
answers to questions 1-3 can be captured fairly easily. For the answer
to question 4, we explore the multiple possibilities in more details.

#. Identify an appropriate compiler: ``mpicc`` which is found by running
   ``module load mpi/openmpi-4.0.5-intel-19.0.5.281``
#. Identify external dependencies: GSL and HDF5 C Libraries and Headers
#. Identify which libraries contain the external function calls we make
   from our program: ``-lgsl -lgslcblas -lm -lhdf5 -lhdf5_hl``
#. Identify where these external dependencies live on the system

In general, there are three viable options for the answer to question 4
and we discuss them below.

**System Level:**\ Although there are some packages installed at the
system level on both the log-in nodes and the compute nodes, relying on
these is the least desirable outcome. This is for two reasons, 1.) it is
hard to know ahead of time which libraries exists on **both** log-in and
compute nodes and not just the log-in nodes and b.) they tend to be
quite old and not optimized versions of a given package. When we refer
to system level libraries, we refer to those packages which live in
standard locations such as

``/usr/lib:/usr/lib64/:/usr/local/lib``

and are installed via the package manager ``yum``.

**Modules:**\ Modules provide the optimal way to locate external
dependencies on Quest. This is true for two reasons. First, they are
installed in a shared location such that all users on all log-in and
compute nodes can access and use them. That is, if you compile a program
against modules, and send your program to a fellow colleague who also
uses Quest, they can run your program since they can load and use the
same libraries that you used. Second, these libraries tend to be more
recent versions of the relevant dependency and are optimally compiled
for running on Quest.

**Virtual/Anaconda Environments:** Anaconda is a software management
tool which allows users to create isolated folders, called
`environments <https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>`__
that contain working (and generally more recent) versions of Python,
C/C++/Fortran and R libraries. Moreover, these environments can be
installed in locations on the filesystem in which the user has
permissions to read/write/execute files.

We now will demonstrate compiling our example code by hand/with a
Makefile with respect to these three options for identifying where your
program’s external dependencies live on the system.

System Level
^^^^^^^^^^^^

How do you know when you have linked against a library installed on the
system and what do that look like? After loading the MPI module that
contains our compiler we need, we can go ahead and run the compile
command without specifying any specific location for where the libraries
live

``mpicc example.c -lgsl -lgslcblas -lm -lhdf5 -lhdf5_hl -o example``

This command will compile, and the reason for this is because HDF5 and
GSL happen to be installed at the system level on the log-in nodes. We
can verify the libraries that your program is linked against by running
the command ldd.

::

   [quser22 Makefile]$ ldd example
       libgsl.so.0 => /usr/lib64/libgsl.so.0 (0x00002b6117685000)
       libgslcblas.so.0 => /usr/lib64/libgslcblas.so.0 (0x00002b6117aae000)
       libm.so.6 => /usr/lib64/libm.so.6 (0x00002b6117ceb000)
       libhdf5.so.8 => /usr/lib64/libhdf5.so.8 (0x00002b6117fed000)
       libhdf5_hl.so.8 => /usr/lib64/libhdf5_hl.so.8 (0x00002b6118493000)

Modules
^^^^^^^

Let us compile this same code, using modules. First, we search for
viable modules for these two dependencies. Second. when deciding which
one to use, it is important to try to load a module that was compiled
using the same compiler and version as the one you choose to compiler
your program. In our case ``intel-19.0.5.281``

``$ module spider hdf5``

``$ module spider gsl``

Looking at the options, we see two modules that fit the bill:

| ``module load hdf5/1.10.7-openmpi-4.0.5-intel-19.0.5.281``
| ``module load gsl/2.5-intel-19.0.5.281``

Let’s use the same compile command to compile the code now.

``mpicc example.c -lgsl -lgslcblas -lm -lhdf5 -lhdf5_hl -o example``

That is it, right? Well, if we check to see which libraries ``example``
is linked against, we see that they have not changed:

.. code:: code

   [quser22 Makefile]$ ldd example
       libgsl.so.0 => /usr/lib64/libgsl.so.0 (0x00002b6117685000)
       libgslcblas.so.0 => /usr/lib64/libgslcblas.so.0 (0x00002b6117aae000)
       libm.so.6 => /usr/lib64/libm.so.6 (0x00002b6117ceb000)
       libhdf5.so.8 => /usr/lib64/libhdf5.so.8 (0x00002b6117fed000)
       libhdf5_hl.so.8 => /usr/lib64/libhdf5_hl.so.8 (0x00002b6118493000)

The reason for this is because the libraries live in non-standard paths
on the system. Therefore, we must explicitly tell the compiler where the
directories which have the header files (``-I``) and which have the
libraries (``-L``) are on the system. Let us try this with the HDF5
libraries and see the difference. To find the PATHS to the libraries and
header files, we can run

``module show hdf5/1.10.7-openmpi-4.0.5-intel-19.0.5.281``

Having done this, we can now try running the command as follows.

``[quser24 Makefile]$ mpicc -I/hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/include example.c -L/hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/lib/ -lgsl -lgslcblas -lm -lhdf5 -lhdf5_hl -o example``

Now when we check which hdf5 libraries example linked against, they are,
in fact, the libraries provided by the modules.

.. code:: code

   [quser22 Makefile]$ ldd example
       ....
       ....
       libhdf5.so.103 => /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/lib/libhdf5.so.103 (0x00002ac8fcee1000)
       libhdf5_hl.so.100 => /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/lib/libhdf5_hl.so.100 (0x00002ac8fd72c000)

Similarly, we can add the ``-I`` and ``-L`` for the location of the gsl
libraries. You may be thinking that if you had a lot of external
dependencies for your code, that constructing the correct command line
by hand would be difficult and time consuming, and you would be right.
That is why most packages are built using tools like autoconf or CMake.

Anaconda
^^^^^^^^

To see if anaconda contains the software you need, and to see what the
package is called on anaconda, you can search anaconda cloud. An example
of searching anaconda cloud for the HDF5 library can be found
`here <https://anaconda.org/search?q=hdf5>`__. To start, we load the
anaconda module and create a virtual environment.

.. code:: code

   module purge all
   module load python/anaconda3.6
   conda create --name makefile-test gsl hdf5 openmpi-mpicc --yes
   source activate makefile-test

Let’s use the same compile command to compile the code now.

``mpicc example.c -lgsl -lgslcblas -lm -lhdf5 -lhdf5_hl -o example``

Unlike with modules, no further work is needed to specify special
non-standard directories for where the libraries live. This is because
you have installed a compiler directly into the virtual environment,
and, therefore, the compiler considers all directories inside of the
environmnet to be “standard locations”.

.. code:: code

   (makefile-test) [quser22 Makefile]$ ldd example
       libgsl.so.23 => /home/netid/.conda/envs/makefile-test/lib/libgsl.so.23 (0x00002ba14e1f7000)
       libgslcblas.so.0 => /home/netid/.conda/envs/makefile-test/lib/libgslcblas.so.0 (0x00002ba14e67a000)
       libhdf5.so.103 => /home/netid/.conda/envs/makefile-test/lib/libhdf5.so.103 (0x00002ba14ebc3000)
       libhdf5_hl.so.100 => /home/netid/.conda/envs/makefile-test/lib/libhdf5_hl.so.100 (0x00002ba14e025000)

| 

Autoconf
~~~~~~~~

`Autoconf <https://www.gnu.org/software/autoconf/autoconf.html>`__ is a
tool which produces shell scripts that automatically configure your
package. To start, we create a ``configure.ac`` which holds the answers
to questions 1-3 of our guidelines to compiling code

-  Identify an appropriate compiler
-  Identify external dependencies
-  Identify which libraries contain the external function calls we make
   from our program

``configure.ac``

.. code:: code

   AC_INIT(example, 1.0)

   # Step 1: Appropriate (working) compiler
   AC_PROG_CC([mpicc])

   # Step 2a: Ensure that you can find the external headers
   AC_CHECK_HEADERS(hdf5.h)
   AC_CHECK_HEADERS(gsl/gsl_sf_bessel.h)

   # Step 3: Make sure that the external libraries contain the function calls that you make
   # and check where those libraries live on the system
   AC_CHECK_LIB(hdf5, H5open)
   AC_CHECK_LIB(hdf5_hl, H5TBread_table)
   AC_CHECK_LIB(gsl, gsl_sf_bessel_J0)

   # Process Makefile.in to create Makefile
   AC_OUTPUT(Makefile)

In order to propagate the information that is found by configure.ac, we
need to create a template Makefile called ``Makefile.in`` that contains
the variables which will be auto-populated during the ``./configure``
stage of the build process.

``Makefile.in``

.. code:: code

   CC=@CC@
   LD=@CC@
   CFLAGS=@CFLAGS@
   LDFLAGS=@LDFLAGS@
   LIBS=@LIBS@

   example: example.o
       $(LD) -o $@ $^ $(LDFLAGS) $(LIBS)

With only loading the MPI module, we can see how autoconf populates
these variables.

.. code:: code

   [quser22 AutoConf]$ autoconf && ./configure
   ...
   ...
   [quser22 AutoConf]$ cat Makefile
   CC=mpicc
   LD=mpicc
   CFLAGS=-g -O2
   LDFLAGS=
   LIBS=-lgsl -lhdf5_hl -lhdf5 

   example: example.o
       $(LD) -o $@ $^ $(LDFLAGS) $(LIBS)

   .PHONY: clean
   clean:
       rm -f example *.o

You can see that most of the information is populated automatically,
including what libraries contain the external function calls that our
program makes. However, similar to our hand-constructed Makefile, we can
send in alternative locations for where our libraries live on the system
by defining the ``LDFLAGS`` variable before we called ``./configure``.

.. code:: code

   [quser22 AutoConf]$ autoconf && LDFLAGS=$(echo $LD_LIBRARY_PATH | echo "-L" $(sed 's/:/ -L /g')) ./configure
   ...
   ...
   [quser22 AutoConf]$ cat Makefile
   CC=mpicc
   LD=mpicc
   CFLAGS=-g -O2
   LDFLAGS=-L /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/gsl-2.5-ioov7cjw6tqqvtzuukzfkt3evh5odjkl/lib -L /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/lib -L /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/libszip-2.1.1-vzorkozqk5vkfxzcybqasxzapt3iw2a5/lib -L /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/openmpi-4.0.5-phpnhxevtv3uq37z7rugq4rwfbxbdcgg/lib -L /hpc/software/spack/opt/spack/linux-rhel7-x86_64/gcc-4.8.5/zlib-1.2.11-ytzoecnlatvnfsvkaist7ddqu3wc2mo3/lib -L /usr/local/ucx-1.8.1/lib64 -L /usr/local/ucx-1.8.1/lib -L /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/numactl-2.0.12-guzt226gnmybpkje5iqja3efcprwsrjx/lib -L /usr/local/spack/opt/spack/linux-rhel7-x86_64/gcc-4.8.5/hwloc-2.1.0-xvch25aylytjrzei5gf26fwrugybnhlr/lib -L /software/intel/2019.5/debugger_2019/libipt/intel64/lib -L /software/intel/2019.5/compilers_and_libraries_2019.5.281/linux/daal/../tbb/lib/intel64_lin/gcc4.4 -L /software/intel/2019.5/compilers_and_libraries_2019.5.281/linux/daal/lib/intel64_lin -L /software/intel/2019.5/compilers_and_libraries_2019.5.281/linux/tbb/lib/intel64/gcc4.7 -L /software/intel/2019.5/compilers_and_libraries_2019.5.281/linux/mkl/lib/intel64_lin -L /software/intel/2019.5/compilers_and_libraries_2019.5.281/linux/compiler/lib/intel64_lin -L /software/intel/2019.5/compilers_and_libraries_2019.5.281/linux/ipp/lib/intel64_lin
   LIBS=-lgsl -lhdf5_hl -lhdf5 

   example: example.o
       $(LD) -o $@ $^ $(LDFLAGS) $(LIBS)

   .PHONY: clean
   clean:
       rm -f example *.o

Now, just as with the Makefile we made by hand, we simply run ``make``
and ensure that the program is linked to the right libraries using the
``ldd`` command.

.. code:: code

   [quser22 AutoConf]$ make
   [quser22 AutoConf]$ ldd example
       libgsl.so.23 => /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/gsl-2.5-ioov7cjw6tqqvtzuukzfkt3evh5odjkl/lib/libgsl.so.23 (0x00002b85863b0000)
       libhdf5_hl.so.100 => /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/lib/libhdf5_hl.so.100 (0x00002b8586dad000)
       libhdf5.so.103 => /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/lib/libhdf5.so.103 (0x00002b8586fda000)

CMake
~~~~~

`CMake <https://cmake.org/>`__ is similar to autoconf, but has a more
abstract and arguably more intuitive structure to its configuration
files. As with all of these compilation options, we do the following:

-  Identify an appropriate compiler
-  Identify external dependencies
-  Identify which libraries contain the external function calls we make
   from our program

``CMakeLists.txt``

.. code:: code

   # CMakeTutorial
   cmake_minimum_required(VERSION 3.10 FATAL_ERROR)

   # step 1 what language is this code in (this determines what compiler is looked for by CMake)
   project(CMakeTutorial VERSION 1.0.0 LANGUAGES C)

   # Step 2: Identify external packages and whether they are required to be discoverable and usable for compilation.
   find_package(MPI REQUIRED)
   find_package(GSL REQUIRED)
   find_package(HDF5 COMPONENTS C HL REQUIRED)

   # Where is the source code
   add_subdirectory(src)

This is the top level configuration files for CMake and it does a lot of
the heavy lifting. We then point at subsequent CMakeLists.txt
configuration files (i.e. the configuration file in
``add_subdirectory(src)``) which then explicitly links the automatically
discovered libraries to our program. With CMake, it is enough to load
the modules containing our dependencies, as CMake will automatically
search ``LD_LIBRARY_PATH`` in order to discover the module libraries
before it discoveries the libraries installed on the system.

``./src/CMakeLists.txt``

.. code:: code

   # CMakeTutorial
   # add the executable
   add_executable(example example.c)

   # add the automatically found path to the external header files needed
   include_directories(SYSTEM ${HDF5_INCLUDE_DIRS})
   include_directories(SYSTEM ${MPI_INCLUDE_DIRS})
   include_directories(SYSTEM ${GSL_INCLUDE_DIRS})

   # link against the automatically discovered libraries (which includes the library and path to where it is on the system)
   target_link_libraries(example ${HDF5_LIBRARIES})
   target_link_libraries(example ${HDF5_HL_LIBRARIES})
   target_link_libraries(example ${GSL_LIBRARIES})
   target_link_libraries(example ${MPI_LIBRARIES} ${MPI_LINK_FLAGS})

   # When you run make install where would you like the executable to go
   install(TARGETS example DESTINATION bin)

To compile, we invoke the ``cmake`` command and point to the top-level
directory. It is common practice to not run the cmake configuartion and
build steps in the same directory as the top-level CMake configuration
file, and instead, to make a new directory and run the commands from
there.

.. code:: code

   module purge all
   module load cmake/3.15.4 
   module load mpi/openmpi-4.0.5-intel-19.0.5.281 
   module load hdf5/1.10.7-openmpi-4.0.5-intel-19.0.5.281 
   module load gsl/2.5-intel-19.0.5.281

   # make a directory to run the configuration and build steps in
   mkdir build
   cd build
   # You can overide the C compiler to be used by CMake by modifying the environmnetal variable CC. Also, you can tell CMake where you would like to install the code (the default location is somewhere where Quest users do not have permissions to install) by passing a relative or absolutel path to the argument -DCMAKE_INSTALL_PREFIX
   CC=mpicc cmake .. -DCMAKE_INSTALL_PREFIX=.
   make install

   -- The C compiler identification is Intel 19.0.5.20190815
   -- Check for working C compiler: /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/openmpi-4.0.5-phpnhxevtv3uq37z7rugq4rwfbxbdcgg/bin/mpicc
   -- Check for working C compiler: /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/openmpi-4.0.5-phpnhxevtv3uq37z7rugq4rwfbxbdcgg/bin/mpicc -- works
   -- Detecting C compiler ABI info
   -- Detecting C compiler ABI info - done
   -- Detecting C compile features
   -- Detecting C compile features - done
   -- Found MPI_C: /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/openmpi-4.0.5-phpnhxevtv3uq37z7rugq4rwfbxbdcgg/bin/mpicc (found version "3.1") 
   -- Found MPI: TRUE (found version "3.1")  
   -- Found PkgConfig: /usr/bin/pkg-config (found version "0.27.1") 
   -- Found GSL: /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/gsl-2.5-ioov7cjw6tqqvtzuukzfkt3evh5odjkl/include (found version "2.5") 
   -- HDF5: Using hdf5 compiler wrapper to determine C configuration
   -- Found HDF5: /hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/hdf5-1.10.7-slwfvnxqgb353tf4ltetdeka6ryccz7y/lib/libhdf5.so;/hpc/software/spack/opt/spack/linux-rhel7-x86_64/intel-19.0.5.281/libszip-2.1.1-vzorkozqk5vkfxzcybqasxzapt3iw2a5/lib/libsz.so;/hpc/software/spack/opt/spack/linux-rhel7-x86_64/gcc-4.8.5/zlib-1.2.11-ytzoecnlatvnfsvkaist7ddqu3wc2mo3/lib/libz.so;/usr/lib64/libdl.so;/usr/lib64/libm.so (found version "1.10.7") found components:  C HL 
   -- Configuring done
   -- Generating done
   -- Build files have been written to: /projects/a9009/sbc538/documentation/108820/cmake_tutorial/cmake/build

Here you can see that in the configuration stage of CMake, you will
already be told which libraries (system, module or otherwise) that CMake
will link your program to, and so using the ``ldd`` command is
unnecessary.

| 

Singularity
~~~~~~~~~~~

Last, but not least, building your software inside of a container can be
an impactful and long term solution for building and distributing your
software. The benefit of containers is that at the time of building the
container, you have ``sudo`` privileges, and so, within the container
you can install all of your dependencies into the system library
locations. Also, once constructed, your container is entirely portable
as is, because the versions of the dependecies that you install inside
of the container will live inside the container forever, untouched.
Below, we start with a container that has Ubuntu 20.10 already installed
(i.e. we bootstrap our container from this container), and use
``apt-get`` to install only the bare minimum extra packages that we need
to compile our code.

``example.def``

.. code:: code

   Bootstrap: docker
   From: ubuntu:groovy-20210225

   %post
       # Upgrade system libraries for ubuntu 20.10 and install libraries we need for our program
       apt-get -y update && apt-get -y install libopenmpi-dev openmpi-bin libhdf5-serial-dev libgsl-dev libgslcblas0 cmake git
       # Clone source code
       git clone https://github.com/CIERA-Northwestern/cmake_tutorial.git /cmake_tutorial
       cd /cmake_tutorial
       # checkout branch of code
       git checkout enhanced_tutorial
       # run cmake commands
       cd cmake
       mkdir -p build
       cd build
       CC=mpicc cmake .. -DCMAKE_INSTALL_PREFIX=/opt/local/
       make install
   %runscript
       /opt/local/bin/example

To build this recipe, we run

.. code:: code

   module load singularity
   singularity build --remote example.simg example.def

For more information on containers, please see `using singularity on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1748>`__.
