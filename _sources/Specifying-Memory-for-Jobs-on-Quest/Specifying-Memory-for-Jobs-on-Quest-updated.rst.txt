Specifying Memory for Jobs on Quest
===================================

This document outlines how to modify job submission scripts to request
the correct amount of memory for jobs on Quest.

**Why should I reserve memory (RAM) in my submission script?
**\ Jobs need access to memory on the compute nodes to run successfully.
If your job does not explicitly reserve memory in its submission script,
it will be given 3.25 GB of memory for each core reserved. This default
amount of memory may not be enough for your job, and jobs that do not
reserve enough memory fail.

| **How can I tell if my job needs more memory to run successfully?**
| Use the ``sacct -X`` command to see information about your recent
  jobs, for example:

.. code:: code

   $ sacct -X
          JobID    JobName  Partition    Account  AllocCPUS      State ExitCode 
   ------------ ---------- ---------- ---------- ---------- ---------- -------- 
   1273539      lammps-te+      short     p1234          40  COMPLETED      0:0 
   1273543      vasp-open+      short     p1234          40 OUT_OF_ME+    0:125

| 
| The “State” field is the status of your job when it finished. Jobs
  with a “COMPLETED” state have run without system errors. Jobs with an
  “OUT_OF_ME+” state have run out of memory and failed. “OUT_OF_ME+”
  jobs need to request more memory in their job submission scripts to
  complete successfully.
| If the job you’re investigating is not recent enough to be listed
  by\ ``sacct -X``, add date fields to the command to see jobs between
  specific start and end dates. For example, to see all jobs between
  September 15, 2019 and September 16, 2019:

.. code:: code

   $ sacct -X --starttime 091519 --endtime 091619

| 
| Specify the date using MMDDYY. More information on ``sacct`` is
  available `here <https://slurm.schedmd.com/sacct.html>`__.

**My job ran out of memory and failed, now what?**

First, determine how much memory your job needs following the steps
outlined below. Once you know how much memory your job needs, edit your
job submission script to reserve that amount of memory + 10% for your
job.

| **How much memory does my job need?**
| To determine out how much memory your job uses on a compute node:
| 1) create a test job by editing your job’s submission script to
  reserve all of the memory of the node it runs on
| 2) run your test job
| 3) confirm your test job has completed successfully
| 4) use ``seff`` to see how much memory your job actually used.
| 1) *Create a test job*
| To profile your job’s memory usage, create a test job by modifying
  your job’s submission script to include the lines:

.. code:: code

   #SBATCH --mem=0
   #SBATCH --nodes=1

| 
| Setting ``--mem=0`` reserves all of the memory on the node for your
  job; if you already have a ``--mem=`` directive in your job submission
  script, comment it out. Now your job will not run out of memory unless
  your job needs more memory than is on the node.
| Setting ``--nodes=1`` reserves a single node for your job. For jobs
  that run on multiple nodes such as MPI-based programs, request the
  number of nodes that your job runs on. Be sure to specify a value for
  ``#SBATCH --nodes=`` or the cores your job submission script reserves
  could end up on as many nodes as cores requested. Be aware that by
  setting ``--mem=0``, you will be reserving all the memory on each of
  those nodes that your cores are reserved on.

| 2) *Run your test job*
| Submit your test job to the cluster with the ``sbatch`` command. For
  interactive jobs, use ``srun`` or ``salloc``. More information on
  submitting jobs to Quest is available
  `here <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__.
| 3) *Did your test job complete successfully?*
| When your job has stopped running use the ``sacct -X``\ command to
  confirm your job finished with state “COMPLETED”. If your test job
  finishes with an “OUT_OF_ME+” state, confirm that you are submitting
  the modified job submission script that requests all of the memory on
  the node. If the “OUT_OF_ME+” errors persist, your job may require
  more memory than is available on the compute node it ran on. In this
  case, please email quest-help@northwestern.edu for assistance.
| 4) *How much memory did your job actually use?*
| To see how much memory it used run the command:
  ``seff <test_job_id_number>``. This returns output similar to:

.. code:: code

   Job ID: 767731
   Cluster: quest
   User/Group: abc123/abc123
   State: COMPLETED (exit code 0)
   Cores: 1
   CPU Utilized: 00:10:00
   CPU Efficiency: 100.00% of 00:10:00 core-walltime
   Job Wall-clock time: 00:10:00
   Memory Utilized: 60.00 GB
   Memory Efficiency: 50.00% of 120.00 GB

| Check the job State reported in the 4th line. If it is “COMPLETED
  (exit code 0)”, look at the last two lines. “Memory Utilized” is the
  amount of memory your job used, in this case 60Gb.

| If the job State is FAILED or CANCELLED, the Memory Efficiency
  percentage reported by ``seff`` will be extremely inaccurate. The
  ``seff`` command only works on jobs that have COMPLETED successfully.
| **How much memory should I reserve in my job script?**
| It’s a good idea to reserve slightly more memory than your job
  utilized since the same job may require slightly different amounts of
  memory depending on variations in data it processes in each run of the
  job. To correctly reserve memory for this job, edit your test job
  submission script to modify the ``#SBATCH --mem=`` directive to
  reserve 10% more than 60Gb in the job submission script:

.. code:: code

   #SBATCH --mem=66G

| 
| For jobs that use MPI, remove the ``#SBATCH --mem=`` directive from
  your job submission script. Now specify the amount of memory you’d
  like to reserve per core instead. For example, if your job uses 100Gb
  of memory total and runs on 10 cores, reserve 10Gb plus a safety
  factor per cpu:

.. code:: code

   #SBATCH --mem-per-cpu=11G

If it doesn’t matter how many nodes your cores are distributed on you
may remove the ``#SBATCH --nodes=`` directive as well.

Be careful not to reserve significant amounts of memory beyond what your
job requires as your job’s wait time will increase and reserving
excessive memory also wastes shared resources that could be used by
other researchers.
