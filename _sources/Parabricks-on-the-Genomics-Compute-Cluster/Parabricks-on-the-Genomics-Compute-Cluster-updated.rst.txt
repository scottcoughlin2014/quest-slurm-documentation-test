Parabricks on the Genomics Compute Cluster
==========================================

How to run Parabricks, the licensed GPU version of GATK 4, on the
Genomics Compute Cluster on Quest.NVIDIA’s Clara
`Parabricks <https://docs.nvidia.com/clara/>`__ is a licensed GPU
version of GATK 4 which runs 10x faster than the open-source CPU version
of GATK, and is available to genomics researchers at Northwestern who
are members of the `Genomics Compute
Cluster <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/genomics-compute-cluster.html>`__.
To run the CPU version of GATK 4, load the gatk/4.1.0 module.
Information on running Parabrick’s GPU version of GATK 4 is below.

Checking out Parabricks Licenses
--------------------------------

.. container::

   When running Parabricks, your job will require a license for each gpu
   card it runs on. We have two Parabricks licenses in the Genomics
   Compute Cluster (GCC). To check out a Parabricks license for your
   job, include the license directive (-L) in your job submission
   command:
   .. code:: code

      sbatch -L parabricks:2 <submission_script.sh>

   In this example, two Parabricks licenses are being checked out for
   this job. The scheduler will keep track of checked out licenses and
   your job will not begin unless licenses are available for it. Run
   your Parabricks job on two GPU cards, using two Parabricks licenses.

Running Parabricks on Quest
---------------------------

.. container::

   Parabricks requires Python and Singularity to run, so before running
   Parabricks load the following Quest modules

.. container::

   .. code:: code

      module load python/anaconda3.6
      module load singularity

.. container::

   Parabricks’s pbrun executable is installed in
   /projects/genomicsshare/software/parabricks_3_6/parabricks/pbrun.

.. container::

   Here’s an example of running the help command, which returns command
   options for Parabricks:

.. container::

   .. code:: code

      /projects/genomicsshare/software/parabricks_3_6/parabricks/pbrun --help

.. container::

.. container::

   Sample submission scripts are in /projects/b1042/Parabricks_Training.
   Researchers do not have write permissions into that directory so
   launch these job submission scripts from your own projects directory.

Fastq to Bam example script
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. container::

   cd to your projects directory before submitting this example script:

.. container::

   .. code:: code

      sbatch -L parabricks:2 /projects/b1042/Parabricks_Training/fq2bam_quest.sh

Deep Variant example script
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. container::

   cd to your projects directory; the output of the deep variant test
   script will be written to a new sub-directory called “deepvariant”.

.. container::

   .. code:: code

      sbatch -L parabricks:2 /projects/b1042/Parabricks_Training/dv.sh

.. container::

.. container::

   For additional information about running Parabricks on Quest, please
   see the `Quest Parabricks
   Training <https://northwestern.box.com/s/13p3lsulum23ddth8iqvfuedgbbpxz1e>`__\ video,
   or reach out to quest-help@northwestern.edu.
