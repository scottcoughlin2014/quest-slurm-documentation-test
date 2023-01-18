Using Singularity on Quest
==========================

Container technology offers a way to package software along with its
dependencies in a format that can be run by any computer. The advantages
of containers include portability, reproducibility, and freedom for
users to run their software of choice on any system, without having to
rely on system administrators to install, configure, and maintain
software. Singularity is a software container technology designed for
scientific workloads on traditional HPC environments such as Quest. In
addition to the benefits offered by other container technologies,
Singularity has security features that limit the privileges of a
container to be the same as the user running the container. This
prevents containers from affecting shared resources such as hardware or
networking that could impact other users. Singularity also uses a simple
image format that makes moving images as easy as copying a single file.

Load the Singularity Module
---------------------------

Singularity is installed as a module on Quest. To use it, either
interactively or in a submission script, you must first load the module:

.. code:: code

   module load singularity

Getting Singularity Images On Quest
-----------------------------------

.. container::

   In many cases, the application that you are interested in using on
   Quest may already have been containerized and made available and
   downloadable from public repositories. We discuss some of these
   public respositories below and how to download these containers using
   ``singularity pull``.

Where Can I Find Container Images?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  `Docker Hub <https://hub.docker.com/>`__
-  `Biocontainers <https://biocontainers.pro/>`__
-  `NVIDIA <https://ngc.nvidia.com/catalog/containers>`__

.. _importingimagesfromdocker:

Pulling Images
~~~~~~~~~~~~~~

When pulling containers Singularity needs to know two pieces of
information, where the container is hosted and the name of the
container. In general the syntax looks like <host>://<name>. For
example,

.. code:: code

   singularity pull docker://ubuntu

will pull the `ubuntu <https://hub.docker.com/_/ubuntu>`__ docker image
into a Singularity Image File (SIF) called *ubuntu_latest.sif.*

By default, Singularity will pull the image with the tag *latest* but it
is possible to pull a specific tag:

.. code:: code

   singularity pull docker://ubuntu:groovy

This will create *ubuntu_groovy.sif*.

.. _buildingcustomsingularityimages:

Building Custom Singularity Images
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the application you are interested in using is not publically
available as a container or the configuration of the container that is
avaible does not suite your needs, then it is also possible to build
your own container image on a Linux computer where you have root access
or by using the `Singularity remote
builder <https://cloud.sylabs.io/builder>`__. You can then copy or pull
that container image to Quest to run it. To build a container using
singularity, you must first define a *recipe* file which is a set of
instructions that tells singularity all the commands that need to be run
in order to install your application and in what environment those
commands need to be run. Afterwards, you can use the
``singularity build`` command to use the recipe file to build a
container.

*In general, ``singularity build`` requires sudo privileges and thus
cannot be run on Quest.* You will need to run it on a Linux computer
with sudo privileges. A virtual machine created with the free Virtualbox
software or a cloud computing instance will work. You can also use
`Singularity’s remote builder <https://cloud.sylabs.io/builder>`__ via
``singularity build --remote``.

Below, we describe how you go about making a Singularity Recipe file and
what you should be thinking about as you construct it. Below we provide
a small example of a Singularity Recipe called *myrecipe.def* but
encourage you to look at the `Singularity
documentation <https://sylabs.io/guides/3.8/user-guide/definition_files.html>`__
for detailed description of the Recipe file format.

*myrecipe.def*

.. code:: code

   Bootstrap: docker   ### Where does the container live that you would like to build on top of.
   From: ubuntu:groovy    ### What is the name of the container that you would like to build on top of.

   ###%post: Imagine you had sudo privileges and a terminal window open, what commands would you run to install your program. Remember that you have access to all the functions and applications that exist in the container that you are building on top of. In our case, we are running on an Ubuntu OS container so we have access/the ability to install programs via apt-get.

   %post
     # we do not want any interactive prompts (we cannot relate to the building of the container interactively)
     export DEBIAN_FRONTEND=noninteractive

     # Upgrade system libraries for ubuntu 21.04 and install libraries we need for our program
     apt-get -y update && apt-get -y install python3 python3-pandas

     # Anything you could do on the command line, you can do here
     # lets make a directory
     mkdir -p /opt/bin

     # Make Python code
     echo "import pandas" > /opt/bin/example.py
     echo "print(\"I imported pandas...\")" >> /opt/bin/example.py


   #### %runscript Imagine you had just finished installing your program using the instructions in %post, what command(s) would you use to run it?

   %runscript 
     python3 /opt/bin/example.py

In order to build this container using the Singularity cloud builder,
you would run the following commands

.. code:: code

   # Authenticate against the remote builder. Will request that you create a token and copy the token in order to authenticate.
   $ singularity remote login
   # Build the container in the remote builder
   $ singularity build --remote myname.sif myrecipe.def

.. _runningsingularitycontainers:

Running Singularity Containers
------------------------------

There are multiple ways to run a Singularity container.

.. _interactiveshell:

Interactive shell
~~~~~~~~~~~~~~~~~

To run an interactive shell in a container (in order to explore the
applications installed inside of it), use the singularity shell command.
When you use the singularity shell command, it is best to imagine that
you have logged into a completely new computer. In many instances, you
will find that programs are installed inside of the container in the
same location as they are on Quest and even have the same name. Despite
this, these applications are completely unique and different than those
installed on Quest. For example,

.. code:: code

   # The following is run on the Quest login node. The application "grep" exists on the Quest login nodes and is version 2.20
   [quser24 part1]$ which grep
   /usr/bin/grep
   [quser24 part1]$ grep --version
   grep (GNU grep) 2.20

   # We start an interactive shell inside of the container. Even though "grep" is installed in the container in the same location as on the Quest login node, it is a completely different version of the application.
   [quser24 part1]$ singularity shell ubuntu_hirsute.sif
   Singularity> which grep
   /usr/bin/grep
   Singularity> grep --version
   grep (GNU grep) 3.6

By default all environment variables (such as $HOME) from the host shell
session where you execute singularity shell will be passed into the
container shell environment unless they have been defined in the
container already (e.g. by a Singularity recipe file or Dockerfile). The
-e flag to singularity shell will run a shell in the container with a
“clean” environment, that is without passing in environment variables
from the host. For more details, see the `Singularity
documentation <https://sylabs.io/guides/3.8/user-guide/environment_and_metadata.htmlnd_metadata.html>`__.

.. _runthedefaultcommandwithrun:

Run The Default Command With run
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Singularity recipes have a %runscript section which defines exactly
which command will run when the container is run with the singularity
run command or directly as an executable file. When an image is imported
from Docker, the Dockerfile’s ENTRYPOINT instruction is used as the
runscript. For example, the
`nuitrcs/hello-world <https://github.com/nuitrcs/hello-world>`__
container’s runscript is a shell script that prints “Hello world!”:

.. code:: code

   singularity run shub://nuitrcs/hello-world
   Hello world!

.. _specifythecommandtorunwithexec:

Specify The Command To Run With exec
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Some containers do not have a runscript or may have multiple executables
that you need to choose from. **The singularity exec command is used to
specify the exact command to run inside the container and is the
preferred way to run singularity containers**:

.. code:: code

   [tempuser03@quser21 part1]$ grep --version
   grep (GNU grep) 2.20
   [tempuser03@quser21 part1]$ singularity exec ubuntu_hirsute.sif grep --version
   grep (GNU grep) 3.6

.. _bindingdirectories:

Binding Directories
~~~~~~~~~~~~~~~~~~~

Singularity allows you to map directories on the host system to
directories within your container using bind mounts. This allows you to
read and write data on the host system with ease. On Quest, $HOME, /tmp,
and /proc are mapped by default from the host into the container.

The –bind or -B option to the Singularity shell, run, and exec commands
also allows the user to specify additional directories to map into the
container. This option’s argument is a comma-delimited string of bind
path specifications in the format src[:dest[:opts]], where src and dest
are outside (that is, on the host) and inside (in the container) paths.
If dest is not given, it is set equal to src. Mount options (opts) may
be specified as ro (read-only) or rw (read/write, which is the default).
The –bind/-B option can be specified multiple times, or a
comma-delimited string of bind path specifications can be used:

.. code:: code

   # A folder which exists on Quest but is not one of /home, /tmp or /proc
   [tempuser03@quser21 part1]$ ls /projects/intro
   test.sh whereami.sh

   # Without specifying --bind/-B, no folders in /projects are mapped onto the container by default and therefore cannot be found.
   [tempuser03@quser21 part1]$ singularity shell ubuntu_hirsute.sif
   Singularity> ls /projects/intro
   ls: cannot access '/projects/intro': No such file or directory

   # If you map the Quest directory /projects into the container then you can list the contents of the folder just like you can on the Quest login node.
   [tempuser03@quser21 part1]$ singularity shell -B /projects:/projects ubuntu_hirsute.sif
   Singularity> ls /projects/intro
   test.sh whereami.sh

In this example, any files and folders you can normally access in
/projects will also be accessible inside of the container.

Creating Modules to Wrap Calls to Containers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It can be very tedious to call containers by the full command
*singularity exec -B /projects:/projects
PATH/TO/CONTAINER/FILE/anaconda3_latest.sif*. To elevate this burden,
you can create your own module which will create a conveience shell/bash
function such that calling programs that actually live inside of the
container will look and feel like calling programs that exist on Quest
itself. Below we demonstrate how to create a module that will create a
bash function *python3* which actually runs the Python3 installed inside
of an anaconda3 container.

.. code:: code

   $ mkdir -p modules/python3
   $ touch modules/grep/3.8.8.lua
   $ cat modules/python3/3.8.8.lua
   help([[
   Python 3.8.8 from anaconda
   ]])

   local pkgName = "python3"
   local version = "3.8.8"

   whatis("Name: " .. pkgName)
   whatis("Version: " .. version)

   depends_on("singularity")

   local bashStr = 'singularity exec -B /projects:/projects /projects/w10001/tempuser03/Singularity/part4/anaconda3_latest.sif python3 $@'
   set_shell_function("python3",bashStr)

Once you have created the .lua module file, you can then add this module
to your PATH and can see how loading it will create a shell funtion
called *python3* which really wraps a singularity exec call.

.. code:: code

   [tempuser03@quser21 part4]$ module use modules/
   [tempuser03@quser21 part4]$ module load python3/3.8.8
   ty[tempuser03@quser21 part4]$ type python3
   python3 is a function
   python3 ()
   {
     singularity exec -B /projects:/projects /projects/w10001/tempuser03/Singularity/part4/anaconda3_latest.sif python3 $@
   }
   [tempuser03@quser21 part4]$ python3 --version
   Python 3.8.8

.. _singularitycontainersinbatchjobs:

Singularity Containers In Batch Jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Singularity commands can be run in a batch submission script just like
any other command. One approach to Singularity is to pull images at
runtime and run them from a temporary directory, so they do not take up
space on the system when not being used. Here is one such example job
submission script:

.. code:: filenameheader

   jobscript.sh

.. code:: code

   #!/bin/bash
   #SBATCH -A <allocation ID>
   #SBATCH -p <partition name>
   #SBATCH -t 02:00:00
   #SBATCH -N 1
   #SBATCH -n 1
   #SBATCH --mem=4G
   #SBATCH --job-name="<job name>"

   module load singularity

   # Use a temporary directory when pulling container image
   export SINGULARITY_CACHEDIR=$TMPDIR

   # Run the container
   singularity exec -B /project/pXXXXX/data:/data docker://biocontainers/blast blastx -query /data/seqs.fasta -out /data/output.blast.txt

This script assumes the seqs.fasta file is in the /project/pXXXXX/data
directory, which is mapped into the container as /data using the -B
option. The output will be written to
/project/pXXXXX/data/output.blast.txt.

.. _gpusupport:

GPU Support
~~~~~~~~~~~

Singularity supports GPU hardware via the –nv option, which uses the
Nvidia drivers installed on the host system inside the container. If you
have access to a GPU allocation on Quest, you can utilize it within a
Singularity container. For more information, please see the Singularity
Container section of `Using GPUs on
QUEST <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1112>`__
for an example of running a GPU enabled container on Quest and
`Singularity’s GPU
Support <https://sylabs.io/guides/3.8/user-guide/gpu.html>`__ page for
more information.

.. _mpisupport:

MPI Support
~~~~~~~~~~~

Singularity does integrate with the Message Passing Interface (MPI) and
OpenMPI version 4.0.5 is supported out of the box. Quest’s
mpi/openmpi-4.0.5-gcc-10.2.0 module should be used for best results, and
*OpenMPI will need to be installed inside the container*. Please see the
`Singularity documentation on Open MPI Hybrid
Container <https://sylabs.io/guides/3.8/user-guide/mpi.html#open-mpi-hybrid-container>`__\ for
more information. Assuming you have installed OpenMPI 4.0.5 inside of
your container following the instructions from Singularity, then you
should be able to run the following:
` <https://sylabs.io/guides/3.8/user-guide/mpi.html#open-mpi-hybrid-container>`__

.. code:: code

   mpirun -np 4 singularity exec my_mpi_container.simg /script/to/run

.. _bestpractices:

Best Practices
--------------

-  Custom container images should contain a single application. Do not
   try to bundle many different executables in your image.
-  Use singularity exec to specify which command to run in your
   container rather than relying on singularity run.
-  When creating custom Singularity images, don’t put anything in $TMP
   or $HOME as those directories will be bind mounted at runtime.
-  Read the `Singularity user
   documentation <https://sylabs.io/guides/3.8/user-guide/index.html>`__
   for more detail on all Singularity functionality.
