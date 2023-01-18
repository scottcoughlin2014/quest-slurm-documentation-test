Getting Started on the Genomics Compute Cluster (b1042) on Quest
================================================================

All Genomics researchers at Northwestern are welcome to join the
Genomics Compute Cluster.

About the Genomics Compute Cluster on Quest
-------------------------------------------

Northwestern IT provides the `Genomics Compute
Cluster <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/genomics-compute-cluster.html>`__
(GCC), a 6,500-core allocation on Quest, consisting of 113 compute
nodes, 3 high-memory nodes, 2 GPU nodes, and 355 TB of dedicated scratch
space. The GCC can be used to align sequencing data, run RNA-Seq,
ChIP-seq, single-cell analysis, whole genome analysis, custom code,
utilize GPUs for machine learning and model training, and more.

How do I start using the Genomics Compute Cluster?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After your `application to join the
GCC <https://app.smartsheet.com/b/form/f6e96bd561114be8a33dc778bc00b919>`__
has been approved,\ `log in to
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__.

Moving files onto Quest
^^^^^^^^^^^^^^^^^^^^^^^

If you have sequencing done by NUSeq Core, they can deliver your FASTQ
files to genomics scratch in /projects/b1042. For information about
transferring files from your local machine,
`RDSS <https://nuinfo-proto9.northwestern.edu/departments/it-services-support/research/data-storage/index.html>`__,
FSMResFiles, OneDrive, or
`Globus <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1557>`__
to Quest please visit the `Transferring Files on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
web page.

Using genomicssoftware available on Quest
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For a complete list of genomics software globally installed on Quest,
see our `Quest Software and
Applications <https://it.northwestern.edu/departments/it-services-support/research/computing/quest-software-and-applications.html>`__
page and select the “genomics” tag.

If you would like to use software packages not currently installed on
Quest, please see `Installing Software on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1742>`__.

Running jobs on the Genomics Compute Cluster: Submission Scripts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On Quest, the GCC is allocation b1042. In your job submission script,
include these sbatch directives:

| #SBATCH -A b1042
| #SBATCH -p <appropriate GCC partition as described below>

To learn more about creating job submission scripts and submitting jobs,
see\ `Everything You Need to Know about Using Slurm on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__.

**Genomics Compute Cluster Partitions**

Partitions are defined for different types of jobs and have different
resource limits. Note that Partitions are also called queues.

Partitions for Feinberg and Weinberg Researchers:

-  **genomics:** For jobs that require less than 48 hours to run.
   Researchers may submit multiple jobs to the genomics partition at one
   time and utilize many cores simultaneously. Jobs can request up to
   256 GB of RAM, and 64 cores.
   Script example: ``#SBATCH -p genomics``
-  **genomicslong:** For jobs that require between 48 - 240 hours to
   run. Priority is lower on this partition than the genomics partition
   to reserve more resources for shorter jobs. Jobs may request up to
   235GB of RAM, and 64 cores.
   Script example: ``#SBATCH -p genomicslong``
-  **genomics-gpu**: Submits jobs to the NVIDIA A100 gpu cards. See
   Using Genomics Compute Cluster GPUs on the `GPUs on
   Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1112>`__
   page for instructions on how to submit jobs to these gpus.
   Script example: ``#SBATCH -p genomics-gpu``
-  **genomics-himem**: Submits jobs to the 2TB high-memory nodes. Jobs
   may request up to 2TB of RAM, and 64 cores.
   Script example: ``#SBATCH -p genomics-himem``
-  **genomics-burst:** For special projects, large jobs requiring more
   than 5 nodes or that run for more than 48 hours may use the
   genomics-burst partition by contacting quest-help@northwestern.edu.
   *Before using genomics-burst partition, users must meet with a
   Research Computing Services consultant to confirm that code has been
   reviewed for efficiency. These appointments will be set up after
   reservations are requested and are intended to ensure best practices
   for this shared resource. Because of the resources involved, jobs may
   need to wait for availability when requesting the genomics-burst
   partition. It is advised to schedule at least three (3) weeks in
   advance.*

Partitions for all other Researchers:

-  **genomicsguest: For jobs that require less than 48 hours to run, a**
   lower-priority version of the genomics partition. Jobs can request up
   to 180GB of RAM, and 52 cores.
   Script example: ``#SBATCH -p genomicsguest``
-  **genomicsguestex:** For jobs that require between 48 - 240 hours to
   run, a lower-priority version of the genomicslong partition. Jobs can
   request up to 180GB of RAM, and 52 cores.
   Script example: ``#SBATCH -p genomicsguestex``
-  **genomicsguest-gpu**: Submits jobs to the NVIDIA A100 gpu cards. See
   Using Genomics Compute Cluster GPUs on the `GPUs on
   Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1112>`__
   page for instructions on how to submit jobs to these gpus.
   Script example: ``#SBATCH -p genomics-guest-gpu``
-  **genomics-himem**: Submits jobs to the 2TB high-memory nodes. Jobs
   may request up to 2TB of RAM, and 64 cores.
   Script example: ``#SBATCH -p genomics-himem``

For example job scripts see the `GitHub repository of example
jobs <https://github.com/nuitrcs/examplejobs>`__.

Reach out to quest-help@northwestern.edu with any questions.
