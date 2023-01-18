Troubleshooting Jobs on Quest
=============================

This page contains steps to help you determine if something is wrong
with your job on Quest and how to go about fixing it.

There are two common issues with jobs on Quest: a job isn’t running or a
job ended before you expected. This page has some tips for figuring out
what may be going wrong. For additional help, contact
quest-help@northwestern.edu.

Why isn’t my job running?
-------------------------

Common reasons why a job or jobs may not be running are:

-  Your allocation is expired or it has been used extensively in the
   recent past beyond your “fair” share of the resources. Use the
   `checkproject <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1486>`__
   utility to check your allocation resources. See `this
   example <https://kb.northwestern.edu/78617>`__ of output from such a
   situation for more details.
-  You requested more resources (memory or cores) per node than are
   available on Quest. The scheduler will reject your submission and let
   you know which resource request is out of bounds according to system
   configuration and architecture. See `Managing
   Jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1544>`__
   for the ``checkjob`` command, and `Checking Processor and Memory
   Utilization for
   Jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1695>`__
   to check the resources used by your job.
-  You submitted a large number of jobs, and some are listed as
   “PENDING”. This is normal. You can submit up to 5000 jobs to the
   scheduler and 1000 of these can run concurrently. The pending jobs
   will wait until running jobs complete and resources become available.
-  Quest is busy.

   -  If you are running under a general access Research Allocation I or
      Research Allocation II, you are using shared resources. If you can
      keep your jobs under four hours of walltime, your jobs will have
      access to more nodes than jobs submitted to normal or long
      partitions.

   -  If you are running under a full access allocation with dedicated
      nodes, see `Full Access Allocation
      Management <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1552>`__
      for commands to check your nodes and jobs. Make sure you have
      specified the appropriate `buyin partition
      (queue) <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1549>`__
      when submitting your job.

   -  To see an estimate of how long it will take for your job to start,
      use the ``--test-only`` option with ``sbatch``:

      .. code:: code

         sbatch --test-only <job_submission_script.sh>

      This command will return an estimate of when this job would start
      if it were submitted:

      .. code:: code

         sbatch: Job 123456 to start at 2019-03-01T07:23:54

      Note this does not actually submit your job.

-  For interactive jobs, see `Why won’t my interactive job
   start? <https://kb.northwestern.edu/78609>`__

Check the Output File
---------------------

The first place to look is the output/error file for your job. See
`Checking the Job Output
File <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1671>`__
for an example. The job output file will be in the directory from which
you submitted your job. Even if you directed output from your
script/program to another location, there is still an output file with
information about the job itself. By default, the output file is named
``slurm-<jobID>.out`` and it contains both the standard output and
error. You can use the cat command to print the contents to the
terminal, or open the file in your preferred text editor.

Another important hint can be obtained from the job’s exit code.
``checkjob <jobID>`` command will provide this code. The ExitCode field
in ``checkjob`` report contains two values delineated by a colon (:).
The first value is the exit code and the second value will indicate the
unix termination signal if a signal was responsible for the termination
of the job. An “ExitCode=0:0” means a successfully completed job (as far
as the scheduler is concerned). Any other value indicates a potential
error. The exit value comes from Slurm and standard unix signals. The
list of codes is fairly cryptic, but you can contact
quest-help@northwestern.edu for assistance deciphering a non-zero exit
code.

The Slurm job state also provides information about the possible issues
you may encounter. If you encounter “FAILED” or “NODE_FAIL” job states,
there is a strong possibility that your job was not successfully
completed. We recommend checking the output for irregularities in these
cases.

.. container:: table-responsive

   +-----------------+------------+-------------------------------------+
   | Job State       | SLURM Code | Description                         |
   +=================+============+=====================================+
   | ``PENDING``     | ``PD``     | Job is queued; waiting to run       |
   +-----------------+------------+-------------------------------------+
   | ``PREEMPTED``   | ``PR``     | Job terminated due to preemption    |
   +-----------------+------------+-------------------------------------+
   | ``CONFIGURING`` | ``CF``     | Job has been allocated resources,   |
   |                 |            | but is waiting for them to become   |
   |                 |            | ready                               |
   +-----------------+------------+-------------------------------------+
   | ``RUNNING``     | ``R``      | Job has allocation and is running   |
   +-----------------+------------+-------------------------------------+
   | ``COMPLETING``  | ``CG``     | Job is in the process of            |
   |                 |            | completing; some nodes may still be |
   |                 |            | active                              |
   +-----------------+------------+-------------------------------------+
   | ``COMPLETED``   | ``CD``     | Job has terminated all processes    |
   |                 |            | succesfully                         |
   +-----------------+------------+-------------------------------------+
   | ``CANCELLED``   | ``CA``     | Job was explicitly cancelled by     |
   |                 |            | user or administrator before or     |
   |                 |            | during running                      |
   +-----------------+------------+-------------------------------------+
   | ``SUSPENDED``   | ``S``      | Job has allocation, but execution   |
   |                 |            | has been suspended                  |
   +-----------------+------------+-------------------------------------+
   | ``TIMEOUT``     | ``TO``     | Job has reached its time limit      |
   +-----------------+------------+-------------------------------------+
   | ``FAILED``      | ``F``      | Job terminated with non-zero exit   |
   |                 |            | code or other failure condition     |
   +-----------------+------------+-------------------------------------+
   | ``NODE_FAIL``   | ``NF``     | Job terminated due to failure of    |
   |                 |            | one or more allocated nodes         |
   +-----------------+------------+-------------------------------------+

If you have an exit code of 0:0 and a COMPLETED state but still think
your job ended early or had a problem, there may have been an error in
your program code/script. Check any additional output and files you may
have written from your code if there isn’t additional information in the
job output/error file.

Resources Exceeded
------------------

Besides errors in your script or hardware failure, your job may be
aborted by the system if it is still running when the walltime limit you
requested (or the upper walltime limit for the partition) is reached.
You will see ``TIMEOUT`` state for these jobs.

If you use more cores than you requested, the system will again stop the
job. This can happen with programs that are multi-threaded. Similarly,
if the job exceeds the requested memory, the job will be terminated. Due
to this, it is important to profile your code for the memory
requirement.

If you do not set the number of nodes/cores, memory or time in your job
submission script, the default values will be assigned by the scheduler.

Out of Disk Space
-----------------

Your job could also fail if you exceed your storage quote in your home
or projects directory.

Check how much space you are using in your home directory with

.. code:: code

   homedu

or

.. code:: code

   du -h --max-depth=0 ~

Check how much space is used in your projects directory with

.. code:: code

   checkproject <allocationID>

Examples
--------

-  `Checking the output
   file <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1671>`__
   when your job fails
-  `Why won’t my interactive job
   start? <https://kb.northwestern.edu/78609>`__
