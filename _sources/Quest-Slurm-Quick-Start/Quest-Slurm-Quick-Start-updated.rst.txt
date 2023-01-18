Quest Slurm Quick Start
=======================

This page contains information for getting started with the Slurm test
cluster on Quest.

| To view a short video demonstration visit `Quest: Modifying Moab
  scripts for
  Slurm <https://www.youtube.com/watch?v=QPWc1JOaBCo&feature=youtu.be>`__

| To view Slurm training videos, visit `Quest Slurm Scheduler Training
  Materials <https://kb.northwestern.edu/internal/page.php?id=89891>`__.

Quick Start
-----------

All job submission scripts that currently run on Quest must be modified
to run on the new Slurm scheduler. When submitting jobs to the Slurm
scheduler, use the allocations and queue names you already use.

Before you submit your Slurm job, modify your existing job submission
script to change the Moab directives into Slurm directives. Most
submission scripts will be straightforward to modify, but implementing
advanced features in Slurm can be expected to take additional time and
effort.

Simple Batch Job Submission Script Conversion Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The `sbatch <https://slurm.schedmd.com/sbatch.html>`__ command is used
for scheduler directives in job submission scripts as well as the job
submission command at the command line. To modify your job scripts to
work with Slurm, you’ll need to edit all lines that currently begin with
#MSUB. In addition to replacing each ``#MSUB`` with ``#SBATCH``, at a
minimum you’ll need to edit lines specifying the queue, nodes, and
walltime. If you have additional scheduler directives, please see the
full list of `Slurm job directive
commands <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1795>`__
and their analogues in Moab.

.. container:: table-responsive

   +-----------------------------------+-----------------------------------+
   | .. code:: filenameheader          | .. code:: filenameheader          |
   |                                   |                                   |
   |    E                              |    Ex                             |
   | xample Moab job submission script | ample Slurm job submission script |
   |                                   |                                   |
   | ::                                | ::                                |
   |                                   |                                   |
   |    #!/bin/bash                    |    #!/bin/bash                    |
   |    #MSUB                          |    #SBATCH -A b1042               |
   | -A b1042               ## account |            ## account (unchanged) |
   |    #MSUB -q g                     |    #SBATCH -p genomics            |
   | enomics            ## queue name  |           ## "-p" instead of "-q" |
   |    #MSUB -l nodes=1:ppn=1         |    #SBATCH -N 1                   |
   |      ## number of nodes and cores |                ## number of nodes |
   |    #MSUB -                        |    #SBATCH -n 1                   |
   | l walltime=00:10:00   ## walltime |                ## number of cores |
   |    #MSUB -N s                     |    #SBATCH                        |
   | ample_job          ## name of job |  -t 00:10:00          ## walltime |
   |                                   |    #SBATCH --                     |
   |    module load                    | job-name="test"    ## name of job |
   |  python           ## Load modules |                                   |
   |                                   |    module purge all               |
   |    python hel                     |      ## purge environment modules |
   | loworld.py         ## Run program |    module load python             |
   |                                   |       ## Load modules (unchanged) |
   |                                   |                                   |
   |                                   |    python helloworld.py           |
   |                                   |        ## Run program (unchanged) |
   |                                   |                                   |
   +-----------------------------------+-----------------------------------+

Environment
~~~~~~~~~~~

Note that when you submit your job Slurm passes your current environment
variables to the compute nodes, including any modules you’ve loaded on
the command line before the job was submitted. By comparison, Moab does
not pass environmental variables but does source a user’s ~/.bashrc file
on the compute node before running the job submission script. Because
Slurm and Moab use different methods to replicate a user’s environment
on the compute nodes, scripts that rely on environment variables may
behave in unexpected ways or fail.

Queues == Partition
~~~~~~~~~~~~~~~~~~~

Note that in Slurm, a “queue” is called a “partition”. Moving forward,
what we have historically called “partitions” on Quest - Quest7, Quest8,
Quest9 and Quest10 - will now be referred to as “architectures”.

Submitting a Batch Job
~~~~~~~~~~~~~~~~~~~~~~

``sbatch`` or ``qsub`` replace ``msub`` for job submissions on the
command line. To use `sbatch <https://slurm.schedmd.com/sbatch.html>`__
to submit a job to the Slurm scheduler:

.. code:: code

   sbatch job_script.sh
   Submitted batch job 546723

or in cases where you only want the job number to be returned, you can
use qsub:

.. code:: code

   qsub job_scipt.sh
   546723

Both ``sbatch`` and ``qsub`` can be used to submit a batch script to
Slurm. Note that not all functionality of qsub is provided under Slurm,
making sbatch the preferred command for submitting jobs.

Slurm will reject the job at submission time if there are requests or
constraints within the job submission script that Slurm cannot meet.
This gives the user the opportunity to examine the rejected job request
and resubmit it with the necessary corrections. With Slurm, if a job
number is returned at the time of job submission, the job will run
although it may experience a wait time in the queue depending on how
busy the system is.

If your job submission receives an error, see `Debugging your Slurm
submission
script <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1808>`__.

Submitting an Interactive Job
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``srun`` replaces ``msub``. To launch a interactive job from the command
line use the `srun <https://slurm.schedmd.com/srun.html>`__ command:

.. code:: code

   srun --pty --account=<account> --time=<hh:mm:ss> --partition=<queue_name> --mem=<xG> bash

This will launch a terminal session on the compute node as a single core
job. To request additional cores for multi-threaded applications,
include the ``-N`` and ``-n`` flags:

.. code:: code

   srun --pty -N 1 -n 6 --account=<account> --time=<hh:mm:ss> --partition=<queue_name> --mem=<xG> bash

For additional information on interactive jobs under Slurm, please see
`Submitting a Job on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__.

Monitoring Jobs
~~~~~~~~~~~~~~~

.. container::
   :name: monitoring_jobs

   ``squeue`` replaces both ``showq`` and ``qstat``. To use
   `squeue <https://slurm.schedmd.com/squeue.html>`__ to see all jobs:

   .. code:: code

      squeue

   To see just your jobs:

   .. code:: code

      squeue -u <NetID>

   squeue returns information on jobs in the Slurm queue:

   .. code:: code

      JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
      546723     short slurm2.s  jon9348  R   INVALID      1 qnode4017
      546711     short high-thr  akh9585  R       2:34      3 qnode[4180-4181,4196]
      546712     short high-thr  akh9585  R       2:34      3 qnode[4078,4086,4196]

   .. container:: table-responsive

      +-----------+---------------------------------------------------------+
      | Field     | Description                                             |
      +===========+=========================================================+
      | JOBID     | Number assigned to the job upon submission              |
      +-----------+---------------------------------------------------------+
      | PARTITION | The queue, also called partition, that the job is       |
      |           | running in                                              |
      +-----------+---------------------------------------------------------+
      | NAME      | Name of the job submission script                       |
      +-----------+---------------------------------------------------------+
      | USER      | NetID of the user who submitted the job                 |
      +-----------+---------------------------------------------------------+
      | ST        | State of the job: “R” for Running or “PD” for Pending   |
      |           | (Idle)                                                  |
      +-----------+---------------------------------------------------------+
      | TIME      | hours:minutes:seconds a job has been running; can be    |
      |           | INVALID for the first few minutes.                      |
      +-----------+---------------------------------------------------------+
      | NODES     | Number of nodes the job resides on                      |
      +-----------+---------------------------------------------------------+
      | NODELIST  | Names of the nodes the job is running on                |
      +-----------+---------------------------------------------------------+

   .. rubric:: Cancelling Jobs
      :name: cancelling-jobs

   To cancel a single job use
   `scancel <https://slurm.schedmd.com/scancel.html>`__:

   .. code:: code

      scancel <job_ID_number>

   To cancel all of your jobs:

   .. code:: code

      scancel -u <netID>

   For additional job commands, please see `Common Job
   Commands <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1795>`__.
