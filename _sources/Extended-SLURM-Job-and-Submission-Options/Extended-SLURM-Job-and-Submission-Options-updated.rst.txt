Extended SLURM Job and Submission Options
=========================================

For more detailed information, see https://slurm.schedmd.com/sbatch.html

Job Submission Options
----------------------

.. container:: table-responsive

   +-----------------------------------+-----------------------------------+
   | Option                            | Slurm (sbatch)                    |
   +===================================+===================================+
   | Script directive                  | ``#SBATCH``                       |
   +-----------------------------------+-----------------------------------+
   | Job name                          | ``--job-name=<name>-J <name>``    |
   +-----------------------------------+-----------------------------------+
   | Account                           | ``                                |
   |                                   | --account=<account>-A <account>`` |
   +-----------------------------------+-----------------------------------+
   | Queue                             | ``--partition=<queue>``           |
   +-----------------------------------+-----------------------------------+
   | Wall time limit                   | ``                                |
   |                                   | --time=<hh:mm:ss>-t``\ <hh:mm:ss> |
   +-----------------------------------+-----------------------------------+
   | Node count                        | ``--nodes=<count>-N <count>``     |
   +-----------------------------------+-----------------------------------+
   | Core count                        | ``-n <count>``                    |
   +-----------------------------------+-----------------------------------+
   | Process count per node            | ``--ntasks-per-node=<count>``     |
   +-----------------------------------+-----------------------------------+
   | Core count (per process)          | ``--cpus-per-task=<cores>``       |
   +-----------------------------------+-----------------------------------+
   | Memory limit                      | ``--mem=<limit>`` (Memory per     |
   |                                   | node in MB)                       |
   +-----------------------------------+-----------------------------------+
   | Minimum memory per processor      | ``--mem-per-cpu=<memory>``        |
   +-----------------------------------+-----------------------------------+
   | Request GPUs                      | ``--gres=gpu:<count>``            |
   +-----------------------------------+-----------------------------------+
   | Request specific nodes            | ``-w, --nodelist=<node>[,node2[,. |
   |                                   | ..]]>-F, --nodefile=<node file>`` |
   +-----------------------------------+-----------------------------------+
   | Job array                         | ``-a <array indices>``            |
   +-----------------------------------+-----------------------------------+
   | Standard output file              | ``--output=<file path>`` (path    |
   |                                   | must exist)                       |
   +-----------------------------------+-----------------------------------+
   | Standard error file               | ``--error=<file path>`` (path     |
   |                                   | must exist)                       |
   +-----------------------------------+-----------------------------------+
   | Combine stdout/stderr to stdout   | ``--output=<                      |
   |                                   | combined out and err file path>`` |
   +-----------------------------------+-----------------------------------+
   | Architecture constraint           | ``--constraint=                   |
   |                                   | <architecture>-C <architecture>`` |
   +-----------------------------------+-----------------------------------+
   | Copy environment                  | ``--export=ALL`` (default)        |
   |                                   | ``--export=NONE`` to not export   |
   |                                   | environment                       |
   +-----------------------------------+-----------------------------------+
   | Copy environment variable         | ``--export=<variable[=v           |
   |                                   | alue][,variable2=value2[,...]]>`` |
   +-----------------------------------+-----------------------------------+
   | Job dependency                    | ``--dependency=after:j            |
   |                                   | obID[:jobID...]--dependency=after |
   |                                   | ok:jobID[:jobID...]--dependency=a |
   |                                   | fternotok:jobID[:jobID...]--depen |
   |                                   | dency=afterany:jobID[:jobID...]`` |
   +-----------------------------------+-----------------------------------+
   | Request event notification        | ``--mail-type=<events>``          |
   |                                   | Note: multiple mail-type requests |
   |                                   | may be specified in a comma       |
   |                                   | separated list:                   |
   |                                   | ``--mail-type=BEGIN,END,FAIL``    |
   +-----------------------------------+-----------------------------------+
   | Email address                     | ``--mail-user=<email address>``   |
   +-----------------------------------+-----------------------------------+
   | Defer job until the specified     | ``--begin=<date/time>``           |
   | time                              |                                   |
   +-----------------------------------+-----------------------------------+
   | Node exclusive job                | ``--exclusive``                   |
   +-----------------------------------+-----------------------------------+

Common Job Commands
-------------------

.. container:: table-responsive

   +-----------------------------------+-----------------------------------+
   | Option                            | Slurm (sbatch)                    |
   +===================================+===================================+
   | Submit a job                      | ``sbatch <job script>``           |
   +-----------------------------------+-----------------------------------+
   | Delete a job                      | ``scancel <job ID>``              |
   +-----------------------------------+-----------------------------------+
   | Job status (all)                  | ``squeue``                        |
   +-----------------------------------+-----------------------------------+
   | Job status (by job)               | ``squeue -j <job ID>``            |
   +-----------------------------------+-----------------------------------+
   | Job status (by user)              | ``squeue -u <netID>``             |
   +-----------------------------------+-----------------------------------+
   | Job status (detailed)             | ``scontrol show jo                |
   |                                   | b -dd <job ID>checkjob <job ID>`` |
   +-----------------------------------+-----------------------------------+
   | Show expected start time          | ``squeue -j <job ID> --start``    |
   +-----------------------------------+-----------------------------------+
   | Queue list / info                 | ``                                |
   |                                   | scontrol show partition [queue]`` |
   +-----------------------------------+-----------------------------------+
   | Hold a job                        | ``scontrol hold <job ID>``        |
   +-----------------------------------+-----------------------------------+
   | Release a job                     | ``scontrol release <job ID>``     |
   +-----------------------------------+-----------------------------------+
   | Start an interactive job          | `                                 |
   |                                   | `salloc <args>srun --pty <args>`` |
   +-----------------------------------+-----------------------------------+
   | X forwarding                      | ``srun --pty <args> --x11``       |
   +-----------------------------------+-----------------------------------+
   | Monitor or review a job’s         | ``sacct -j <job_nu                |
   | resource usage                    | m> --format JobID,jobname,NTasks, |
   |                                   | nodelist,CPUTime,ReqMem,Elapsed`` |
   |                                   | (see sacct for all format         |
   |                                   | options)                          |
   +-----------------------------------+-----------------------------------+
   | View job batch script             | ``scontrol write                  |
   |                                   | batch_script <jobID> [filename]`` |
   +-----------------------------------+-----------------------------------+

Script Variables
----------------

.. container:: table-responsive

   +-----------------------+-----------------------+-----------------------+
   | Info                  | Slurm                 | Notes                 |
   +=======================+=======================+=======================+
   | Version               | –                     | Can extract from      |
   |                       |                       | ``sbatch --version``  |
   +-----------------------+-----------------------+-----------------------+
   | Job name              | ``$SLURM_JOB_NAME``   |                       |
   +-----------------------+-----------------------+-----------------------+
   | Job ID                | ``$SLURM_JOB_ID``     |                       |
   +-----------------------+-----------------------+-----------------------+
   | Submit directory      | ``$SLURM_SUBMIT_DIR`` | Slurm jobs starts     |
   |                       |                       | from the submit       |
   |                       |                       | directory by default  |
   +-----------------------+-----------------------+-----------------------+
   | Node list             | ``                    | The Slurm variable    |
   |                       | $SLURM_JOB_NODELIST`` | has a different       |
   |                       |                       | format to the PBS     |
   |                       |                       | one.                  |
   |                       |                       | To get a list of      |
   |                       |                       | nodes use:            |
   |                       |                       | ``sco                 |
   |                       |                       | ntrol show hostnames  |
   |                       |                       | $SLURM_JOB_NODELIST`` |
   +-----------------------+-----------------------+-----------------------+
   | Job array index       | ``$                   |                       |
   |                       | SLURM_ARRAY_TASK_ID`` |                       |
   +-----------------------+-----------------------+-----------------------+
   | Queue name            | ``$                   |                       |
   |                       | SLURM_JOB_PARTITION`` |                       |
   +-----------------------+-----------------------+-----------------------+
   | Number of nodes       | ``$SLURM_JOB_NUM      |                       |
   | allocated             | _NODES$SLURM_NNODES`` |                       |
   +-----------------------+-----------------------+-----------------------+
   | Number of processes   | ``$SLURM_NTASKS``     |                       |
   +-----------------------+-----------------------+-----------------------+
   | Number of processes   | ``$S                  |                       |
   | per node              | LURM_TASKS_PER_NODE`` |                       |
   +-----------------------+-----------------------+-----------------------+
   | Requested tasks per   | ``$SL                 |                       |
   | node                  | URM_NTASKS_PER_NODE`` |                       |
   +-----------------------+-----------------------+-----------------------+
   | Requested CPUs per    | ``$                   |                       |
   | task                  | SLURM_CPUS_PER_TASK`` |                       |
   +-----------------------+-----------------------+-----------------------+
   | Scheduling priority   | ``                    |                       |
   |                       | $SLURM_PRIO_PROCESS`` |                       |
   +-----------------------+-----------------------+-----------------------+
   | Job user              | ``$SLURM_JOB_USER``   |                       |
   +-----------------------+-----------------------+-----------------------+
   | Hostname              | ``$HOSTNAME ==        | Unless a shell is     |
   |                       |  $SLURM_SUBMIT_HOST`` | invoked on an         |
   |                       |                       | allocated resource,   |
   |                       |                       | the ``$HOSTNAME``     |
   |                       |                       | variable is           |
   |                       |                       | propagated (copied)   |
   |                       |                       | from the submit       |
   |                       |                       | machine environment   |
   |                       |                       | to all allocated      |
   |                       |                       | nodes.                |
   +-----------------------+-----------------------+-----------------------+

| 
