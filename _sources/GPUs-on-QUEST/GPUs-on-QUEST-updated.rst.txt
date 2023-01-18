GPUs on QUEST
=============

Information about GPUs on QUEST

What GPUs are available on QUEST?
---------------------------------

There are 12 GPU nodes available to the Quest `General
Access <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__
allocations. These nodes run CUDA version 11.7 and Driver Version:
515.43.04:

-  26 x 40GB Tesla A100 GPUs available on 13 nodes (two GPUs, 52 CPU
   cores, and 192 GB RAM on each node)

.. container::

   There are 2 GPU nodes in the `Genomics Compute
   Cluster <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/genomics-compute-cluster.html>`__
   (b1042). These nodes run CUDA version 11.7 and Driver Version:
   515.43.04:

.. container::

   -  8 x 40GB Tesla A100 GPUs available on 2 nodes (four GPUs, 52 CPU
      cores, and 192 GB RAM on each node)

Using General Access GPUs
~~~~~~~~~~~~~~~~~~~~~~~~~

The maximum run time is 48 hours for a job on these nodes. To submit
jobs to general access GPU nodes, you should set gengpu as the partition
and state the number of GPUs in your job submission command or script.
You can also identify the type of GPU you want in your job submission.
For instance to request one A100 GPU, you should add the following lines
in your job submission script:

::

   #SBATCH -A <allocationID>
   #SBATCH -p gengpu
   #SBATCH --gres=gpu:a100:1
   #SBATCH -N 1
   #SBATCH -n 1
   #SBATCH -t 1:00:00
   #SBATCH --mem=XXG

Note that the memory you request here is for CPU memory. You are
automatically given access to the entire memory of the GPU, but you will
also need CPU memory as you will be copying memory from the CPU to the
GPU.

To schedule another type of GPU, e.g. P100, you should change the a100
designation to the other GPU type, e.g. p100.

Using Genomics Compute Cluster GPUs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container::

   The maximum run time is 48 hours for a job on these nodes. Feinberg
   members of the Genomics Compute Cluster should use the partition
   genomics-gpu, while non-Feinberg members should use
   genomicsguest-gpu. To submit a job to these GPUs, include the
   appropriate partition name and specify the type and number of GPUs:

.. container::

   ::

      #SBATCH -A b1042
      #SBATCH -p genomics-gpu
      #SBATCH --gres=gpu:a100:1
      #SBATCH -N 1
      #SBATCH -n 1
      #SBATCH -t 1:00:00
      #SBATCH --mem=XXG

Note that the memory you request here is for CPU memory. You are
automatically given access to the entire memory of the GPU, but you will
also need CPU memory as you will be copying memory from the CPU to the
GPU.

Interactive GPU jobs
--------------------

.. container::

   If you want to start an interactive session on a GPU instead of a
   batch submission, you can use a run command similar to the one below
   - these examples both request a A100:

   ``srun -A pXXXXX -p gengpu --mem=XX --gres=gpu:a100:1 -N 1 -n 1 -t 1:00:00 --pty bash -l``

   ``salloc -A pXXXXX -p gengpu --mem=XX --gres=gpu:a100:1 -N 1 -n 1 -t 1:00:00``

What GPU software is available on QUEST?
----------------------------------------

CUDA
~~~~

To see which versions of CUDA are available on Quest, run the command:

module spider cuda

NOTE: You cannot use code or applications which require a CUDA toolkit
or module that is newer than the CUDA versions listed above. However,
CUDA modules and toolkits that are older than the CUDA versions listed
at the top of this page should still work.

Anaconda
~~~~~~~~

We strongly encourage people to use anaconda to create virtual
environments in order to use software that utilize GPUs, especially when
using Python. Please see `Using Python on
QUEST <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1672>`__
for more information on anaconda virtual environments. Below we provide
instructions for creating a local anaconda virtual environment
containing…

a. Tensorflow
b. PyTorch
c. CUpy
d. Rapids

Please run the command that come after the ``$``.

Tensorflow
^^^^^^^^^^

.. container:: panel panel-default

   .. container:: panel-heading

      Tensorflow 2.6

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      1. Load anaconda on QUEST

      ``$ module load python-miniconda3/4.12.0``

      2. Create a virtual environment with\ ``tensorflow`` and
      ``cudatoolkit==11.2``. We are going to name our environment
      ``tensorflow-2.6-py38``. On QUEST, by default, all anaconda
      environments go into a folder in your HOME directory called
      ``~/.conda/envs/``. Therefore, once these steps are completed, all
      of the necessary packages will live in a folder whose PATH is
      ``~/.conda/envs/tensorflow-2.6-py38``.

      ``$ conda create --name tensorflow-2.6-py38 -c conda-forge tensorflow[build=cuda112*] cudatoolkit=11.2``

      3. Activate virtual environment

      ``$ source (conda) activate tensorflow-2.6-py38``

PyTorch
^^^^^^^

.. container:: panel panel-default

   .. container:: panel-heading

      PyTorch 1.11.X

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      1. Load anaconda on QUEST

      ``$ module load python-miniconda3/4.12.0``

      2. Create a virtual environment with ``pytorch`` and
      ``cudatoolkit==11.2``. We are going to name our environment
      ``pytorch-1.11-py38``. On QUEST, by default, all anaconda
      environments go into a folder in your HOME directory called
      ``~/.conda/envs/``. Therefore, once these steps are completed, all
      of the necessary packages will live in a folder whose PATH is
      ``~/.conda/envs/pytorch-1.11-py38``.

      ``$ conda create --name pytorch-1.11-py38 -c conda-forge pytorch=1.11[build=cuda112*] numpy python=3.8 cudatoolkit=11.2 --yes``

      3. Activate virtual environment

      ``$ source (conda) activate pytorch-1.11-py38``

CUpy
^^^^

.. container:: panel panel-default

   .. container:: panel-heading

      CUpy 11.2

   .. container:: panel panel-body js-panelnormalswitches2 collapse

      1. Load anaconda on QUEST

      ``$ module load python-miniconda3/4.12.0``

      | 2. Create a virtual environment and install Python into it. We
        are going to name our environment ``CUpy-py38``. On QUEST, by
        default, all anaconda environments go into a folder in your HOME
        directory
      | called ``~/.conda/envs/``. Therefore, once these steps are
        completed, all of the necessary packages will live in a folder
        whose PATH is ``~/.conda/envs/CUpy-py38``.

      ``$ conda create -n CUpy-py38 python=3.8 cudatoolkit=11.2 -c nvidia --yes``

      3. Activate virtual environment

      ``$ source (conda) activate CUpy-py38``

      4. Install the CUpy binary that is pre-compiled against CUDA 11.2

      ``$ python3 -m pip install cupy-cuda112``

      **Note:** CUpy will only import correctly on a GPU node and will
      not import on a CPU only node.

Rapids
^^^^^^

.. container:: panel panel-default

   .. container:: panel-heading

      Rapids 22.06

   .. container:: panel panel-body js-panelnormalswitches3 collapse

      1. Load anaconda on QUEST

      ``$ module load python-miniconda3/4.12.0``

      | 2. Create a virtual environment and install Python into it. We
        are going to name our environment ``rapids-22.06``. On QUEST, by
        default, all anaconda environments go into a folder in your HOME
        directory
      | called ``~/.conda/envs/``. Therefore, once these steps are
        completed, all of the necessary packages will live in a folder
        whose PATH is ``~/.conda/envs/rapids-22.06``.

      ``$conda create -n rapids-22.06 -c rapidsai -c nvidia -c conda-forge rapids=22.06 python=3.9 cudatoolkit=11.4 jupyterlab --yes``

      3. Activate virtual environment

      ``$ source (conda) activate rapids-22.06``

      **Note:** Please see `getting started with
      rapids <https://rapids.ai/start.html>`__ for more details.

Singularity
~~~~~~~~~~~

NVIDIA provides a whole host of `GPU
containers <https://ngc.nvidia.com/catalog/containers/>`__ that are
suitable for different applications. Docker images cannot be used
directly on Quest due to security risks, but they can be pulled to
generate Singularity containers. Below we provide an examples of using
Singularity to pull the NVIDIA Tensorflow Docker image and the NVIDIA
PyTorch Docker image.

.. _tensorflow-1:

Tensorflow
^^^^^^^^^^

For most NVIDIA containers, there are many different versions which come
with specific versions of the relevant libraries and packages. See
NVIDIA’s `TensorFlow
documentation <https://docs.nvidia.com/deeplearning/frameworks/tensorflow-release-notes/overview.html#overview>`__
for further information about the version of Tensorflow that is shipped
with each version of the Tensorflow Docker container.

.. container:: panel panel-default

   .. container:: panel-heading

      Tensorflow 2.5

   .. container:: panel panel-body js-panelnormalswitches4 collapse

      ::

         module purge all
         module load singularity
         singularity pull docker://nvcr.io/nvidia/tensorflow:21.07-tf2-py3

      We can then use the command below to call this NVIDIA GPU
      container to run a simple TensorFlow training example.

      ``singularity exec --nv -B /projects:/projects tensorflow_21.07-tf2-py3.sif python training.py``

.. _pytorch-1:

PyTorch
^^^^^^^

For most NVIDIA containers, there are many different versions which come
with specific versions of the relevant libraries and packages. See
NVIDIA’s `PyTorch
documentation <https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/index.html>`__
for further information about the version of PyTorch that is shipped
with each version of the PyTorch Docker container.

.. container:: panel panel-default

   .. container:: panel-heading

      PyTorch 1.10

   .. container:: panel panel-body js-panelnormalswitches5 collapse

      ::

         module purge all
         module load singularity
         singularity pull docker://nvcr.io/nvidia/pytorch:21.07-py3

      We can then use the command below to call this NVIDIA GPU
      container to run a simple TensorFlow training example.

      ``singularity exec --nv -B /projects:/projects pytorch_21.07-py3.sif python training_pytorch.py``

NOTE: A key difference between calling a non-GPU container versus a GPU
container is passing the ``--nv`` argument to the ``exec`` command. A
reminder that ``-B /projects:/projects`` mounts the projects folder into
the singularity environment. By default, ``/projects`` is not mounted or
discoverable by the container. Please see our page `containers on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1748>`__
for more information on containers in general.
