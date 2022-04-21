SLURM Commands and Job Management
---------------------------------

In this section, we discuss how to manage batch jobs after they’ve been
submitted on Quest. This includes how to monitor jobs currently pending
or running, how to cancel jobs, and how to check on the status of past
jobs.

.. _section-common-slurm-commands:

Table of Common Slurm Commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   +-----------------------------------+-----------------------------------+
   | Option                            | Slurm (sbatch)                    |
   +===================================+===================================+
   | Submit a job                      | sbatch <job script>               |
   +-----------------------------------+-----------------------------------+
   | Delete a job                      | scancel <job ID>                  |
   +-----------------------------------+-----------------------------------+
   | Job status (by job)               | squeue -j <job ID>                |
   +-----------------------------------+-----------------------------------+
   | Job status (by user)              | squeue -u <netID>                 |
   +-----------------------------------+-----------------------------------+
   | Job status (detailed)             | scontrol show job -dd <job ID>    |
   |                                   | checkjob <job ID>                 |
   +-----------------------------------+-----------------------------------+
   | Show expected start time          | squeue -j <job ID> --start        |
   +-----------------------------------+-----------------------------------+
   | Queue list / info                 | scontrol show partition [queue]   |
   +-----------------------------------+-----------------------------------+
   | Hold a job                        | scontrol hold <job ID>            |
   +-----------------------------------+-----------------------------------+
   | Release a job                     | scontrol release <job ID>         |
   +-----------------------------------+-----------------------------------+
   | Start an interactive job          | salloc <args>                     |
   |                                   | srun --pty <args>                 |
   +-----------------------------------+-----------------------------------+
   | X forwarding                      | srun --pty <args> --x11           |
   +-----------------------------------+-----------------------------------+
   | Monitor or review a job's         | sacct -j <job_num> --format       |
   | resource usage                    | JobID,jobname,NTask               |
   |                                   | s,nodelist,CPUTime,ReqMem,Elapsed |
   |                                   | (see sacct for all format         |
   |                                   | options)                          |
   +-----------------------------------+-----------------------------------+
   | View job batch script             | scontrol write batch_script       |
   |                                   | <jobID> [filename]                |
   +-----------------------------------+-----------------------------------+

.. _h_65825072313811650315528091:

The ``squeue`` Command
~~~~~~~~~~~~~~~~~~~~~~

The ``squeue`` command can be used display information about your
current jobs on Quest.

======================== =============================================
Command                  Description
======================== =============================================
squeue -u <NetID>        Show only jobs belonging to user specified
squeue -A <AllocationID> Show only jobs belonging to account specified
squeue -j <JobID>        Display the status of the specified job
squeue -t R              Show running jobs
squeue -t PD             Show pending jobs
squeue --help            See documentation and additional options
======================== =============================================

.. _h_65825072313811650315528091:

The ``sacct`` Command
~~~~~~~~~~~~~~~~~~~~~

The ``sacct`` command can be used display information about your past
and current jobs on Quest.

Like ``sstat``, the standard output of sacct may not provide the
information we want. To remedy this, we can use the ``--format`` flag to
choose what we want in our output. Similarly, the format flag is handled
by a list of comma separated variables which specify output data:

.. container::

   .. container::

      ::

         $ sacct --user=your_rc-username --format=var_1,var_2, ... ,var_N

A chart of some variables is provided below:

.. container::

   +--------------+--------------------------------------------------------------+
   | Variable     | Description                                                  |
   +==============+==============================================================+
   | account      | Account the job ran under.                                   |
   +--------------+--------------------------------------------------------------+
   | avecpu       | Average CPU time of all tasks in job.                        |
   +--------------+--------------------------------------------------------------+
   | averss       | Average resident set size of all tasks in the job.           |
   +--------------+--------------------------------------------------------------+
   | cputime      | Formatted (Elapsed time \* CPU) count used by a job or step. |
   +--------------+--------------------------------------------------------------+
   | elapsed      | Jobs elapsed time formated as DD-HH:MM:SS.                   |
   +--------------+--------------------------------------------------------------+
   | exitcode     | The exit code returned by the job script or salloc.          |
   +--------------+--------------------------------------------------------------+
   | jobid        | The id of the Job.                                           |
   +--------------+--------------------------------------------------------------+
   | jobname      | The name of the Job.                                         |
   +--------------+--------------------------------------------------------------+
   | maxdiskread  | Maximum number of bytes read by all tasks in the job.        |
   +--------------+--------------------------------------------------------------+
   | maxdiskwrite | Maximum number of bytes written by all tasks in the job.     |
   +--------------+--------------------------------------------------------------+
   | maxrss       | Maximum resident set size of all tasks in the job.           |
   +--------------+--------------------------------------------------------------+
   | ncpus        | Amount of allocated CPUs.                                    |
   +--------------+--------------------------------------------------------------+
   | nnodes       | The number of nodes used in a job.                           |
   +--------------+--------------------------------------------------------------+
   | ntasks       | Number of tasks in a job.                                    |
   +--------------+--------------------------------------------------------------+
   | priority     | Slurm priority.                                              |
   +--------------+--------------------------------------------------------------+
   | qos          | Quality of service.                                          |
   +--------------+--------------------------------------------------------------+
   | reqcpu       | Required number of CPUs                                      |
   +--------------+--------------------------------------------------------------+
   | reqmem       | Required amount of memory for a job.                         |
   +--------------+--------------------------------------------------------------+
   | user         | Username of the person who ran the job.                      |
   +--------------+--------------------------------------------------------------+

As an example, suppose you want to find information about jobs that were
run on March 12, 2018. You want to show information regarding the job
name, the number of nodes used in the job, the number of cpus, the
maxrss, and the elapsed time. Your command would look like this:

.. container::

   .. container::

      ::

         $ sacct --jobs=your_job-id --starttime=2018-03-12 --format=jobname,nnodes,ncpus,maxrss,elapsed

As another example, suppose you would like to pull up information on
jobs that were run on February 21, 2018. You would like information on
job ID, job name, QoS, Number of Nodes used, Number of CPUs used,
Maximum RSS, CPU time, Average CPU time, and elapsed time. Your command
would look like this:

.. container::

   .. container::

      ::

         $ sacct –-jobs=your_job-id –-starttime=2018-02-21 --format=jobid,jobname,qos,nnodes,ncpu,maxrss,cputime,avecpu,elapsed

A full list of variables that specify data handled by sacct can be found
with the ``--helpformat`` flag or by `visiting the slurm page on
sacct <https://slurm.schedmd.com/sacct.html>`__.

.. _h_30279704940681650315796740:

The checkjob Command
~~~~~~~~~~~~~~~~~~~~

The checkjob command displays detailed information about a submitted
job’s status and diagnostic information that can be useful for
troubleshooting submission issues. It can also be used to obtain useful
information about completed jobs such as the allocated nodes, resources
used, and exit codes.

Example usage:

.. code:: code

   checkjob <JobID>

where you can get your <JobID> using the squeue commands above.

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
-  The paths to job's working directory (WorkDir), standard error
   (StdErr) and output (StdOut) files are given.
-  If a batch job script is used for submission, the script is presented
   at the end.

.. _h_62538097251311650316072591:

Cancelling Jobs
~~~~~~~~~~~~~~~

You can cancel one or all of your jobs with scancel. Proceed with
caution, as this cannot be undone, and you will not be prompted for
confirmation after issuing the command.

================== ===============================
Command            Description
================== ===============================
scancel <JobID>    Cancel the job with given ID
scancel -u <NetID> Cancel all the jobs of the user
================== ===============================

Holding, Releasing, or Modifying Jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Users can place their jobs in a "JobHeldUser" state while submitting the
job or after the job has been queued. Running jobs cannot be placed on
hold.

===================== =============================================
Command               Description
===================== =============================================
#SBATCH -H            Place hold within job script
sbatch -H <jobscript> Place hold while submitting from command line
scontrol hold <jobID> Place hold on a queued job from command line
===================== =============================================

The job status will be shown in the output of monitoring commands such
as squeue or checkjob.

To release a job from user hold state:

.. code:: code

   scontrol release <JobID>

The job control command (scontrol) can also be used for changing the
parameters of a submitted job before it starts running. The following
parameters can be modified safely:

-  Job dependency (change to "none")
-  Partition (queue)
-  Job name
-  Wall clock limit
-  Allocation

The table below contains some useful examples of using scontrol to
change a job's parameters.

+----------------------------------+----------------------------------+
| Command                          | Description                      |
+==================================+==================================+
| scontrol update job=<JobID>      | Change job to depend successful  |
| dependency=afterok:1000          | completion of the job 1000       |
+----------------------------------+----------------------------------+
| scontrol update job=<JobID>      | Change partition to short        |
| partition=short                  |                                  |
+----------------------------------+----------------------------------+
| scontrol update job=<JobID>      | Change name to myjob             |
| name=myjob                       |                                  |
+----------------------------------+----------------------------------+
| scontrol update job=<JobID>      | Set job time limit to 2 hours    |
| timelimit=2:00:00                |                                  |
+----------------------------------+----------------------------------+
| scontrol update job=<JobID>      | Change the allocation to p12345  |
| account=p12345                   |                                  |
+----------------------------------+----------------------------------+

For a complete listing of scontrol options, see the official `scontrol
documentation <https://slurm.schedmd.com/scontrol.html>`__.

Probing Priority
~~~~~~~~~~~~~~~~

Slurm implements a multi-factor priority scheme for ordering the queue
of jobs wauting to be run. sprio command is used to see the contribution
of different factors to a pending job's scheduling priority.

+------------------+--------------------------------------------------+
| Command          | Description                                      |
+==================+==================================================+
| sprio            | Show scheduling priority for all pending jobs    |
|                  | for the user                                     |
+------------------+--------------------------------------------------+
| sprio -j <jobID> | Show scheduling priority of the defined job      |
+------------------+--------------------------------------------------+

For running jobs, you can see the starting priority using checkjob
<jobID> command.
