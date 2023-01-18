Using QIIME2 on Quest
=====================

QIIME2 (Quantitative Insights into Microbial Ecology version 2) is an
open-source computing platform for processing and analyzing DNA amplicon
sequence data. This page describes how users may download and use QIIME2
on Quest.

Installing QIIME2 on Quest
~~~~~~~~~~~~~~~~~~~~~~~~~~

`QIIME2 <https://qiime2.org/>`__ is available as a `system-wide
module <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1550>`__
on Quest. To explore the versions currently installed on Quest, run:
``module avail qiime2``. If a specific version is required, users can
also install QIIME2 themselves within a `Singularity
container <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1748>`__
or a `conda <https://www.anaconda.com>`__ virtual environment. The
installation route users choose will be dependent on their day-to-day
use of QIIME2 and familiarity with creating virtual environments. As a
new user to Quest, installing QIIME2 via Singularity is generally
easier, although the software will be limited to the plugins included in
the core `QIIME2
distribution <https://docs.qiime2.org/2022.2/install/#qiime-2-core-2022-2-distribution>`__.
The system-wide QIIME2 modules on Quest also only contain the core
QIIME2 plugins. This is not a problem for most users; however, if you
plan to use `third-party
plugins <https://library.qiime2.org/plugins/>`__ in your QIIME2
workflow, we strongly recommend installing QIIME2 in a conda
environment. In addition to being much more flexible with adding other
plugins, calling QIIME2 from a conda environment generally results in
more readable code as compared to calling it from a Singularity
container.

Installation and use via Singularity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

DockerHub maintains an image of the most up-to-date version of QIIME2
that can be downloaded via Singularity onto Quest. Navigate to your
project directory on Quest and run the following command to pull the
latest version of QIIME2 (version 2022.2 at the time of writing):

.. code:: code

   module load singularity

   singularity pull docker://quay.io/qiime2/core:2022.2

This will create a Singularity Image File called ``core_2022.2.sif`` in
your working directory. This file will have to be explicitly called
before every QIIME2 command with ``singularity exec``. For example, to
run the ``qiime --help`` command, submit the following:

.. code:: code

   singularity exec core_2022.2.sif qiime --help

It should be noted that if the above command is submitted outside of the
user’s working directory as written, ``core_2022.2.sif``\ will not be
found. Please refer to our `Singularity Knowledge Base
page <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1748>`__
or the `Singularity user
documentation <https://syslabs.io/guides/2.5/user-guide/index.html>`__
for more information on how to specify file paths with Singularity.

Installation and use with conda
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create a QIIME2 conda environment, first download the configuration
file provided by the developers into your working directory:

.. code:: code

   wget https://data.qiime2.org/distro/core/qiime2-2022.2-py38-linux-conda.yml

This config file lists all the software dependencies that QIIME2
requires. Once it is downloaded, load the
`miniconda <https://docs.conda.io/en/latest/miniconda.html>`__ module
and create a QIIME2 conda environment. Please note that it is not
unusual for this step to require over an hour to complete:

.. code:: code

   module purge all
   module load python-miniconda3
   conda env create -n qiime2-2022.2 --file qiime2-2022.2-py38-linux-conda.yml

As an optional step, you can remove the config file from your working
directory:

.. code:: code

   rm qiime2-2022.2-py38-linux-conda.yml

Once the conda environment is created, use ``source activate`` to call
QIIME2 (``qiime2-2022.2``). Afterwards, you can begin using QIIME2 by
simply calling ``qiime``:

.. code:: code

   source activate qiime2-2022.2
   qiime --help

Also note that you can install the conda environment in your project
directory by replacing the ``-n qiime2-2022.2``\ option with
``--prefix /path/to/allocation`` and activating the environment from
that path. Installing conda environments in your project directories can
save storage space in your home folder. Please refer to the “Anaconda
Virtual Environments” section of `this
page <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1672>`__\ for
more information.

.. code:: code

   conda env create --prefix /projects/p12345/qiime2-2022.2 --file qiime2-2022.2-py38-linux-conda.yml
   source activate /projects/p12345/qiime2-2022.2
