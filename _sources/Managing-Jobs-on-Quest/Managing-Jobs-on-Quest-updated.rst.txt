Managing Jobs on Quest
======================

How to manage batch jobs after they’ve been
`submitted <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
on Quest.

Please note: Job resource requirements, such as the number of cores or
node or the amount of memory, in the submission script are recorded by
the scheduler. Any changes made to the `job
script <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
(``jobscript.sh`` in the example on `Submitting a Job on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__)
after the job has been submitted with ``sbatch`` will have no effect on
the job. After submitting a job, you can only Hold, Release, Kill, and
Modify job parameters using the the commands in the list below.

If your job isn’t running or ended before you expected, see
`Troubleshooting
Jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1545>`__.
for some tips.

Job ID
------

When you submit jobs on Quest using ``sbatch``, the scheduler returns
the job ID and queues it for execution.

For example, if a user submitted ``jobscript.sh`` using ``sbatch``:

.. code:: code

   [abc123@quser21 ~]$ sbatch jobscript.sh

If the job was submitted successfully, a job ID will be returned:

.. code:: code

   Submitted batch job 548609

If you do not want the text to be displayed before job ID, you could use
``--parsable`` flag with ``sbatch`` command:

.. code:: code

   [abc123@quser21 ~]$ sbatch --parsable jobscript.sh

The successful submission will produce only the integer job ID:

.. code:: code

   548610

You can use this job ID later to monitor the job.

Job Status
----------

After submitting a job, you can execute the ``squeue`` command or
``checkjob`` command to check the status of your job. On the cluster,
submitted jobs are analyzed and queued by Slurm scheduler. If the job
submission script has a typo, the scheduler will indicate the
unrecognized flag. The job can still be scheduled however defaults
options will be set for the unrecognized flag.

If you have an active allocation, the job is forwarded to the scheduler
to be put in queue. It is important to note that if there is a typo in
your job submission script in defining the required resources, the
scheduler will notify you about the wrong flag. The job will still be
submitted with the default setting for the wrong flag.

When the scheduler receives a job, it will give your job relative
priority compared to other jobs. This priority is influenced by the wait
time, usage history, job size, partition and allocation type. Detailed
information about priority calculation can be found
`here <https://slurm.schedmd.com/priority_multifactor.html>`__.

Generally, the more resources that a job requires, the longer a job may
sit in the queue until the necessary resources become free and can be
scheduled. Full access nodes are `dedicated
resources <https://it.northwestern.edu/secure/purchasing-resources.html>`__
thus the access criteria, partitions, job duration and job size limits
for these nodes are different. See `Full Access Job
Commands <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1552>`__
for specialized information.

Commonly Used Commands
----------------------

The ``squeue`` Command
~~~~~~~~~~~~~~~~~~~~~~

The ``squeue`` command (without any options), displays your jobs on
Quest.

.. container:: table-responsive

   +------------------------------+--------------------------------------+
   | Command                      | Description                          |
   +==============================+======================================+
   | ``squeue -u <NetID>``        | Show only jobs belonging to user     |
   |                              | specified                            |
   +------------------------------+--------------------------------------+
   | ``squeue -A <AllocationID>`` | Show only jobs belonging to account  |
   |                              | specified                            |
   +------------------------------+--------------------------------------+
   | ``squeue -j <JobID>``        | Display the status of the specified  |
   |                              | job                                  |
   +------------------------------+--------------------------------------+
   | ``squeue -t R``              | Show running jobs                    |
   +------------------------------+--------------------------------------+
   | ``squeue -t PD``             | Show pending jobs                    |
   +------------------------------+--------------------------------------+
   | ``squeue --help``            | See documentation and additional     |
   |                              | options                              |
   +------------------------------+--------------------------------------+

A user can submit 5000 jobs for scheduling. If there are available
resources, 1000 of these jobs can be run concurrently.

The ``checkjob`` Command
~~~~~~~~~~~~~~~~~~~~~~~~

The ``checkjob`` command displays detailed information about a submitted
job’s status and diagnostic information that can be useful for
troubleshooting submission issues. It can also be used to obtain useful
information about completed jobs such as the allocated nodes, resources
used, and exit codes.

Example usage:

.. code:: code

   checkjob <JobID>

where you can get your <JobID> using the ``squeue`` commands above.

Example for a Successfully Running Job
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: code

   [abc123@quser21 ~]$ checkjob 548867
   --------------------------------------------------------------------------------------------------------------------
   JOB INFORMATION
   --------------------------------------------------------------------------------------------------------------------
   JobId=548867 JobName=high-throughput-cpu_000094
      UserId=abc123(123123) GroupId=abc123(123) MCS_label=N/A
      Priority=1315 Nice=0 Account=p12345 QOS=normal
      JobState=RUNNING Reason=None Dependency=(null)
      Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
      RunTime=00:13:13 TimeLimit=00:40:00 TimeMin=N/A
      SubmitTime=2019-01-22T12:51:42 EligibleTime=2019-01-22T12:51:43
      AccrueTime=2019-01-22T12:51:43
      StartTime=2019-01-22T15:52:20 EndTime=2019-01-22T16:32:20 Deadline=N/A
      PreemptTime=None SuspendTime=None SecsPreSuspend=0
      LastSchedEval=2019-01-22T15:52:20
      Partition=short AllocNode:Sid=quser21:15454
      ReqNodeList=(null) ExcNodeList=(null)
      NodeList=qnode[5056-5060]
      BatchHost=qnode5056
      NumNodes=5 NumCPUs=120 NumTasks=120 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
      TRES=cpu=120,mem=360G,node=5,billing=780
      Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
      MinCPUsNode=1 MinMemoryCPU=3G MinTmpDiskNode=0
      Features=(null) DelayBoot=00:00:00
      OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
      Command=(null)
      WorkDir=/projects/p12345/high-throughput
      StdErr=/projects/p12345/high-throughput/lammps.error
      StdIn=/dev/null
      StdOut=/projects/p12345/high-throughput/lammps.output
      Power=
   --------------------------------------------------------------------------------------------------------------------
   JOB SCRIPT
   --------------------------------------------------------------------------------------------------------------------
   #!/bin/bash
   #SBATCH --account=p12345
   #SBATCH --partition=normal
   #SBATCH --job-name=high-throughput-cpu
   #SBATCH --ntasks=120
   #SBATCH --mem-per-cpu=3G
   #SBATCH --time=00:40:00
   #SBATCH --error=lammps.error
   #SBATCH --output=lammps.output

   module purge
   module load lammps/lammps-22Aug18

   mpirun -n 120 lmp -in in.fcc

Note in the output above that:

-  The JobState is listed as RUNNING.
-  The time passed after job start and the total walltime request are
   given with RunTime and TimeLimit.
-  The node name(s) are listed after NodeList.
-  The paths to job’s working directory (WorkDir), standard error
   (StdErr) and output (StdOut) files are given.
-  If a batch job script is used for submission, the script is presented
   at the end.

Cancelling Jobs
~~~~~~~~~~~~~~~

You can cancel one or all of your jobs with ``scancel``. Proceed with
caution, as this cannot be undone, and you will not be prompted for
confirmation after issuing the command.

.. container:: table-responsive

   ====================== ===============================
   Command                Description
   ====================== ===============================
   ``scancel <JobID>``    Cancel the job with given ID
   ``scancel -u <NetID>`` Cancel all the jobs of the user
   ====================== ===============================

Holding, Releasing, or Modifying Jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Users can place their jobs in a “JobHeldUser” state while submitting the
job or after the job has been queued. Running jobs cannot be placed on
hold.

.. container:: table-responsive

   ========================= =============================================
   Command                   Description
   ========================= =============================================
   ``#SBATCH -H``            Place hold within job script
   ``sbatch -H <jobscript>`` Place hold while submitting from command line
   ``scontrol hold <jobID>`` Place hold on a queued job from command line
   ========================= =============================================

The job status will be shown in the output of monitoring commands such
as ``squeue`` or ``checkjob``.

To release a job from user hold state:

.. code:: code

   scontrol release <JobID>

The job control command (``scontrol``) can also be used for changing the
parameters of a submitted job before it starts running. The following
parameters can be modified safely:

-  Job dependency (change to “none”)
-  Partition (queue)
-  Job name
-  Wall clock limit
-  Allocation

The table below contains some useful examples of using scontrol to
change a job’s parameters.

.. container:: table-responsive

   +----------------------------------+----------------------------------+
   | Command                          | Description                      |
   +==================================+==================================+
   | ``scontrol update job=<          | Change job to depend successful  |
   | JobID> dependency=afterok:1000`` | completion of the job 1000       |
   +----------------------------------+----------------------------------+
   | ``scontrol upda                  | Change partition to short        |
   | te job=<JobID> partition=short`` |                                  |
   +----------------------------------+----------------------------------+
   | ``scontrol                       | Change name to myjob             |
   |  update job=<JobID> name=myjob`` |                                  |
   +----------------------------------+----------------------------------+
   | ``scontrol update                | Set job time limit to 2 hours    |
   |  job=<JobID> timelimit=2:00:00`` |                                  |
   +----------------------------------+----------------------------------+
   | ``scontrol upd                   | Change the allocation to p12345  |
   | ate job=<JobID> account=p12345`` |                                  |
   +----------------------------------+----------------------------------+

For a complete listing of scontrol options, see the official
```scontrol``
documentation <https://slurm.schedmd.com/scontrol.html>`__.

Probing Priority
~~~~~~~~~~~~~~~~

Slurm implements a multi-factor priority scheme for ordering the queue
of jobs wauting to be run. ``sprio`` command is used to see the
contribution of different factors to a pending job’s scheduling
priority.

.. container:: table-responsive

   +----------------------+----------------------------------------------+
   | Command              | Description                                  |
   +======================+==============================================+
   | ``sprio``            | Show scheduling priority for all pending     |
   |                      | jobs for the user                            |
   +----------------------+----------------------------------------------+
   | ``sprio -j <jobID>`` | Show scheduling priority of the defined job  |
   +----------------------+----------------------------------------------+

For running jobs, you can see the starting priority using
``checkjob <jobID>`` command.
