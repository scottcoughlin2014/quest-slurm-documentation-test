Everything You Need to Know about Using Slurm on Quest
======================================================

This page contains in-depth information on submitting jobs on Quest
through Slurm.

The program that schedules jobs and manages resources on Quest is called
Slurm. This page is designed to help facilitate a deeper understanding
of how Slurm works and to contextualize the things that you can do with
Slurm. For those who are brand new to Quest, we recommend visiting our
`Research Computing How-to
Videos <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=2004>`__
page and watching our *Introduction to Quest* video before proceeding
with this documentation. This page covers:

-  A simple Slurm job submission script.
-  The key configuration settings of the Slurm submission script as well
   as details on additional settings that may be useful to specify.
-  How to monitor and manage your job submissions
-  Special types of job submissions and when they are useful.
-  Diagnosing issues with your job submission.

.. container::
   :name: container

   .. container::
      :name: tableOfContents

      -  `The Job Submission Script <#section-job-submission-script>`__

         -  `Example Submission
            Script <#section-example-submission-script>`__
         -  `Submitting Your Batch
            Job <#section-submitting-your-batch-job>`__

      -  `Slurm Configuration
         Settings <#key-slurm-configuration-settings>`__

         -  `Account/Allocation <#section-account>`__
         -  `Partition/Queue <#section-partitions>`__
         -  `Walltime/Length of the job <#section-walltime>`__
         -  `Number of nodes <#section-number-of-nodes>`__
         -  `Number of cores <#section-number-of-cores>`__
         -  `Required memory <#section-required-memory>`__
         -  `Standard output/error <#section-output-error>`__
         -  `Job Name <#section-job-name>`__
         -  `E-Mail Alerts <#section-email>`__
         -  `Constraint <#section-constraints>`__
         -  `All Slurm Configuration
            Settings <#section-all-slurm-options>`__
         -  `Environmental Variables Set by
            Slurm <#section-slurm-environmental-variables>`__

      -  `SLURM Commands and Job
         Management <#all-slurm-commands-and-submission-options>`__

         -  `Table of Common Slurm
            Commands <#section-common-slurm-commands>`__
         -  `squeue <#squeue>`__
         -  `sacct <#sacct>`__
         -  `seff <#seff>`__
         -  `checkjob <#checkjob>`__
         -  `scancel <#scancel>`__

      -  `Special Types of Job
         Submissions <#section-special-job-types>`__

         -  `Interactive Jobs <#section-interactive-jobs>`__

            -  `Without Graphical User
               Interface <#section-section-interactive-jobs-non-gui>`__
            -  `With Graphical User
               Interface <#section-section-interactive-jobs-gui>`__

         -  `Job Array <#section-job-array>`__
         -  `Dependent Jobs <#section-dependent-jobs>`__

      -  `Factors Affecting Job Scheduling on
         Quest <#section-job-scheduling>`__
      -  `Diagnosing Issues with Your Job Submission Script and/or Your
         Job Itself <#section-diagnosing-jobs>`__

         -  `Debugging a Job Rejected by the
            Scheduler <#section-debugging-jobs>`__
         -  `Debugging a Job Accepted by the Scheduler that Fails
            Immediately <#h_94310085932911650907024000>`__\ ` <#section-job-scheduling>`__

      -  `Common Reasons for Failed Jobs <#section-job-failures>`__

         -  `Not enough memory or time
            requested <#section-not-enough-memory-or-time>`__
         -  `Ran out of disk space in home folder or allocation
            folder <#section-ran-out-of-disk>`__

      Expand All Collapse All

   .. container::
      :name: content

      .. rubric:: The Job Submission Script
         :name: section-job-submission-script

      Slurm requires users to write a batch submission script to run a
      batch job. In this section, we present a basic submission script
      and how a user would then submit a batch job using this script. In
      the submission script, you specify the resources you need and what
      commands to run. You submit this script to the scheduler by
      running the ``sbatch`` command on the command line.

      .. rubric:: Example Submission Script
         :name: section-example-submission-script

      The following is an example batch job submission script. Please
      note any placeholders which are denoted with ``<>``. You will need
      to replace these with the appropriate value, and remove the
      ``<>``. This job submission script would be saved in a file such
      as ``jobscript.sh``.

      ::

         #!/bin/bash
         #SBATCH --account=<account> ## Required: your allocation/account name, i.e. eXXXX, pXXXX or bXXXX
         #SBATCH --partition=short ## Required: (buyin, short, normal, long, gengpu, genhimem, etc)
         #SBATCH --time=00:10:00 ## Required: How long will the job need to run (remember different partitions have restrictions on this parameter)
         #SBATCH --nodes=1 ## how many computers/nodes do you need (no default)
         #SBATCH --ntasks-per-node=1 ## how many cpus or processors do you need on per computer/node (default value 1)
         #SBATCH --mem=1G ## how much RAM do you need per computer/node (this affects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --job-name=sample_job ## When you run squeue -u  this is how you can identify the job
         #SBATCH --output=output.log ## standard out and standard error goes to this file

         # A regular comment in Bash
         module purge all
         module load python-miniconda3/4.12.0
         source activate /projects/intro/envs/slurm-py37-test

         python --version
         python slurm_test.py

      The first line of the script loads the bash shell. Only the lines
      that begin with ``#SBATCH`` are interpreted by Slurm at the time
      of job submission. Any words following ``##`` are treated as
      comments and ignored by Slurm interpreter. Once Slurm places the
      job on a compute node, the remainder of the script (everything
      after the last ``#SBATCH`` line) is run. Normally in Bash, ``#``
      is a comment character which means that anything written after a
      ``#`` is ignored by the Bash interpreter/language. When writing a
      submission script, however, the Slurm interpreter recognizes
      #SBATCH as a command.

      After the Slurm commands, the rest of the script works like a
      regular Bash script. You can modify environment variables, load
      modules, change directories, and execute program commands. Lines
      in the second half of the script that start with ``#`` are
      comments, such as ``# A regular comment in Bash`` in the the
      example above.

      Find a downloadable copy of this example script `on
      GitHub <https://github.com/nuitrcs/examplejobs>`__.

      .. rubric:: Submitting Your Batch Job
         :name: section-submitting-your-batch-job

      After you have written and saved your submission script, you can
      submit your job. At the command line type

      .. code:: code

         sbatch <name_of_script>

      where, in the example above ``<name_of_script>`` would be
      ``jobscript.sh``. Upon submission the scheduler will return your
      job number:

      .. code:: code

         Submitted batch job 549005

      If you have a workflow that accepts or needs the jobid as an input
      for `job
      monitoring <#all-slurm-commands-and-submission-options>`__ or `job
      dependencies <#section-dependent-jobs>`__, then you may prefer the
      return value of your job submission be just the job number. To do
      this, pass the ``--parsable`` argument:

      .. code:: code

         sbatch --parsable <name_of_script>
         549005

      If there is an error in your job submission script, the job will
      not be accepted by the scheduler and you will receive an error
      message right away, for example:

      .. code:: code

         sbatch: error: Batch job submission failed: Invalid account or account/partition combination specified

      If your job submission receives an error, you will need to address
      the issue and resubmit your job. If no error is received, your job
      has entered the queue and will run.

      .. rubric:: Slurm Configuration Settings
         :name: slurm-configuration-settings

      In this section, we go into details about a number of the Slurm
      configurations settings. In each subsection, we include;

      -  the possible range of values a user can set,
      -  how to think about what value to use for a given setting,
      -  whether the setting is required, and
      -  if a setting is required, what the default is for the setting.

      .. rubric:: Allocation/Account
         :name: section-account

      To specify the account, please include a ``-A/--account`` option
      in your job submission script:

      ``#SBATCH -A <allocation>``

      Slurm offers two methods, one short (such as ``-A``) and one
      verbose (``--account``) to write most of its setting flags. For
      verbose methods, an ``=`` sign is required after the flag (such as
      ``--account=``).

      To submit jobs to Quest, you must be part of an **active**
      classroom, research, or buy-in allocation. Additionally, for
      buy-in allocations, you must be part of a **buy-in allocation with
      access to compute resources**. To determine the names of the
      allocation(s)/account(s) that you are part of on Quest, you can
      run the following on the command line which will produce output
      similar to below.

      ::

         $ groups
         quest_demo b1011 p30157 e31572

      Once you determine the allocation(s)/account(s) that you are part
      of, you can check whether the allocation is active and has access
      to compute resources by running ``checkproject`` and reading the
      last two lines of the output. An example of running
      ``checkproject`` is below.

      ::

         $ checkproject <allocation> 

         ==================================== 
         Reporting for project pXXXXX
         ------------------------------------
         1 GB in 4623 files (0% of 1000 GB quota)
         Allocation Type: Allocation I
         Expiration Date: 2022-12-01
         Status: ACTIVE
         Compute and storage allocation - when status is ACTIVE, this allocation has compute node access and can submit jobs
         ------------------------------------
         ====================================

      For more information on allocations and allocation management,
      please see `Managing an Allocation on
      Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1486>`__.

      .. rubric:: Quest Partitions/Queues
         :name: section-partitions

      To specify the partition, please include a ``-p/--partition``
      option in your job submission script:

      ``#SBATCH -p <partition>``

      Quest offers several partitions or queues where you can run your
      job. Based on the duration of your job, number of cores, and type
      of access to Quest, you should select the most appropriate
      partition for your job. A partition must be specified when you
      submit your job otherwise the scheduler will return the error,
      ``"sbatch: error: Batch job submission failed: No partition specified or system default partition"``.

      .. rubric:: Partition Definitions: General Access (“p” and “e”
         accounts)
         :name: partition-definitions-general-access-p-and-e-accounts

      Standard compute node access.

      .. container:: table-responsive

         +-----------+-----------------+-----------------+-----------------+
         | Partition | Minimum         | Maximum         | Notes           |
         |           | Walltime        | Walltime        |                 |
         +===========+=================+=================+=================+
         | short     | 00:00:00        | 04:00:00        | The short       |
         |           |                 |                 | partition is    |
         |           |                 |                 | for jobs that   |
         |           |                 |                 | will run in 4   |
         |           |                 |                 | hours or less.  |
         |           |                 |                 | The short       |
         |           |                 |                 | partition has   |
         |           |                 |                 | access to more  |
         |           |                 |                 | compute nodes   |
         |           |                 |                 | than the normal |
         |           |                 |                 | or long         |
         |           |                 |                 | partitions.     |
         |           |                 |                 | This feature    |
         |           |                 |                 | plus the        |
         |           |                 |                 | shorter         |
         |           |                 |                 | duration        |
         |           |                 |                 | enables short   |
         |           |                 |                 | partition jobs  |
         |           |                 |                 | to be scheduled |
         |           |                 |                 | faster.         |
         +-----------+-----------------+-----------------+-----------------+
         | normal    | 04:00:00        | 48:00:00        | The normal      |
         |           |                 |                 | partition is    |
         |           |                 |                 | for jobs that   |
         |           |                 |                 | will run in     |
         |           |                 |                 | between 4 hours |
         |           |                 |                 | and 2 days. The |
         |           |                 |                 | normal          |
         |           |                 |                 | partition has   |
         |           |                 |                 | access to more  |
         |           |                 |                 | compute nodes   |
         |           |                 |                 | than the long   |
         |           |                 |                 | partition, but  |
         |           |                 |                 | less than the   |
         |           |                 |                 | short           |
         |           |                 |                 | partition.      |
         +-----------+-----------------+-----------------+-----------------+
         | long      | 48:00:00        | 168:00:00       | The long        |
         |           |                 |                 | partition is    |
         |           |                 |                 | for jobs that   |
         |           |                 |                 | will run in     |
         |           |                 |                 | between 2 days  |
         |           |                 |                 | and 7 days. The |
         |           |                 |                 | long partition  |
         |           |                 |                 | has access to a |
         |           |                 |                 | less compute    |
         |           |                 |                 | nodes than the  |
         |           |                 |                 | short or normal |
         |           |                 |                 | partition.      |
         +-----------+-----------------+-----------------+-----------------+

      Specialty compute node access

      .. container:: table-responsive

         +-----------+------------------+-------------------------------------+
         | Partition | Maximum Walltime | Notes                               |
         +===========+==================+=====================================+
         | gengpu    | 48:00:00         | This partition can be used only if  |
         |           |                  | your job requires GPUs. In addition |
         |           |                  | to entering *gengpu* as your        |
         |           |                  | partition in your submission        |
         |           |                  | script, you must specify            |
         |           |                  | ``#SBATCH --gres=gpu:a100:X``,      |
         |           |                  | where ``X`` is the number of GPUs   |
         |           |                  | you want to request. Please see     |
         |           |                  | `GPUs on                            |
         |           |                  | QUEST <https://                     |
         |           |                  | services.northwestern.edu/TDClient/ |
         |           |                  | 30/Portal/KB/ArticleDet?ID=1112>`__ |
         |           |                  | for more information about the      |
         |           |                  | GPUs.                               |
         +-----------+------------------+-------------------------------------+
         | genhimem  | 48:00:00         | This partition can be used only if  |
         |           |                  | your job requires more than 180 GB  |
         |           |                  | memory per node. This partition has |
         |           |                  | access to 4 nodes, three 28-core    |
         |           |                  | nodes with 500GB of schedulable     |
         |           |                  | memory and one 52-core node with    |
         |           |                  | 1.5TB of schedulable memory.        |
         +-----------+------------------+-------------------------------------+

      .. rubric:: Partition Definitions: Full Access (buy-ins, or “b”
         accounts)
         :name: partition-definitions-full-access-buy-ins-or-b-accounts

      .. container:: table-responsive

         +-----------------------+-----------------------+-----------------------+
         | Partition             | Maximum Walltime      | Notes                 |
         +=======================+=======================+=======================+
         | Allocation name (e.g. | Allocation-specific   | For those users part  |
         | “b1234”)              |                       | of an allocation      |
         | or                    |                       | which has             |
         | “buyin”               |                       | purchased\ `computing |
         |                       |                       | access <h             |
         |                       |                       | ttps://it.northwester |
         |                       |                       | n.edu/secure/purchasi |
         |                       |                       | ng-resources.html>`__ |
         |                       |                       | to Quest, the         |
         |                       |                       | resources available   |
         |                       |                       | and any limits on     |
         |                       |                       | jobs are governed by  |
         |                       |                       | the specific policies |
         |                       |                       | of the full-access    |
         |                       |                       | allocation. In        |
         |                       |                       | addition to setting   |
         |                       |                       | the ``-A/--account``  |
         |                       |                       | flag to your buy-in   |
         |                       |                       | allocation name, you  |
         |                       |                       | can set the partition |
         |                       |                       | in one of two ways:   |
         |                       |                       |                       |
         |                       |                       | | Example:            |
         |                       |                       |                       |
         |                       |                       | ``#SBATCH -p bXXXXX`` |
         |                       |                       | | Example:            |
         |                       |                       |   ``#SBATCH-p buyin`` |
         |                       |                       |                       |
         |                       |                       | If your allocation    |
         |                       |                       | has specific          |
         |                       |                       | partition names, such |
         |                       |                       | as ``genomics``,      |
         |                       |                       | ``ciera-std``,        |
         |                       |                       | ``grail-std`` etc.,   |
         |                       |                       | you should use those  |
         |                       |                       | partition names       |
         |                       |                       | instead of your       |
         |                       |                       | allocation name or    |
         |                       |                       | ``buyin``.            |
         +-----------------------+-----------------------+-----------------------+

      .. rubric:: Notes
         :name: notes

      Additional specialized partitions exist for specific allocations.
      You may be instructed to use a partition name that is not listed
      above.

      If you have a general access allocation and need to run jobs
      longer than one week, `contact Research
      Computing <mailto:quest-help@northwestern.edu?subject=Quest%20Long-running%20job>`__
      for a consultation. Some special accommodations can be made for
      jobs requiring the resources of up to a single node for a month or
      less.

      .. rubric:: Walltime/Length of the job
         :name: section-walltime

      To specify the walltime for your job, please include a
      ``-t/--walltime`` option in your job submission script:

      ``#SBATCH -t <timelimit>``

      There are two important considerations when selecting the
      walltime, the `partition <#section-partitions>`__ that you chose
      and how long your job is expected to run. Although the partition
      that you choose will control the maximum wall time that can be
      selected, we do *not* recommend to simply select the maximum
      allowable wall time for that partition unless it is truly needed.
      There are two problematic ways to set wall time that will create
      undesired outcomes:

      -  If your walltime is much longer than your job needs to run,
         then your job will take longer to start running.
      -  If your walltime is shorter than your job needs to run, then
         your job will fail as there is no way to extend the walltime of
         a running job on Quest.

      This is why we recommend submitting a single, representative job
      and seeing how long it takes to run before selecting a walltime
      and submitting a large number of jobs. Please note that Slurm
      considers *only the*\ **actual**\ *duration of your job (not the
      defined walltime) when calculating your resource utilization.*

      .. rubric:: Number of Nodes
         :name: section-number-of-nodes

      To specify the number of nodes, please include the ``-N/--nodes``
      option in your job submission script:

      ``#SBATCH --nodes=<number_of_nodes>``

      Although the number of nodes is an optional setting, we strongly
      recommend always setting this value. Specifically, we recommend
      setting this value to

      ``#SBATCH --nodes=1``

      as the vast majority of software can only run on a single computer
      and cannot run across multiple computers. When you forget to set
      this value, but you do set the `Number of
      Cores <#number-of-cores>`__, this can cause Slurm to match you
      with a set of computing resources which your application will be
      unable to use, but will still be penalized in your fair share for
      using. If you know that your application uses Message Passing
      Interface (MPI) to parallelize, then setting this value to
      something ``>1`` could make sense.

      .. rubric:: Number of Cores
         :name: section-number-of-cores

      There are two methods for specifying the number of cores, the
      ``-n/--ntasks`` option which indicates how many total cores you
      would like:

      ``#SBATCH --ntasks=<number_of_cores>``

      or the ``--ntasks-per-node=`` option whichindicates how many cores
      you would like *per node* and should always be used with the
      `Number of Nodes <#number-of-nodes>`__ option:

      .. code:: code

         #SBATCH --nodes=<number_of_nodes>
         #SBATCH --ntasks-per-node=<number_of_cores_per_node>

      Although the number of cores is an optional setting whose default
      is 1, we strongly recommend always setting this value.
      Specifically, we recommend setting this value (to start) to

      ``#SBATCH --ntasks=1``

      The only situation in which ``-n/--ntasks`` should be greater than
      1 is if the application you are using has the capability to be
      parallelized. Many applications do **not** have this capability
      and therefore it is best to start of setting this value to 1. If
      your application is capable of parallelization, you will next want
      to determine what type of parallelization it uses in order to set
      this value correctly. For instance, if your application utilizes
      shared memory parallelization (OpenMP, R’s doParallel, Python’s
      multiprocessing, MATLAB local parpool, etc) then you can consider
      setting this value to be greater than 1. However, shared memory
      parallelization can only utilize CPUs *within a single computer*
      and CPUs allocated *across* computers will go unused. Therefore,
      if your code is parallelized in this manner, you must also specify

      ``#SBATCH --nodes=1``

      If you know that your application uses Message Passing Interface
      (MPI) to parallelize, then it can utilize CPUs allocated\ *across*
      computers and therefore setting ``-n/--ntasks`` without also
      setting ``-N/--nodes`` would make sense.

      A final consideration when selecting the number of CPUs is how
      many CPUs are available on each of the different
      generations/families of compute nodes that make up Quest. Below is
      a table which summarizes the relevant information.

      .. container:: table-responsive

         +----------------+----------------+----------------+----------------+
         | Node Family    | Number of CPUs | Amount of      | Partitions     |
         | Name           |                | *              | with these     |
         |                |                | *Schedulable** | Nodes          |
         |                |                | Memory/RAM     |                |
         +================+================+================+================+
         | quest8         | 28             | 84GB           | short/nor      |
         | (general       |                |                | mal/long/buyin |
         | access)        |                |                |                |
         +----------------+----------------+----------------+----------------+
         | quest8 (buyin) | 28             | 180GB          | buyin/short    |
         +----------------+----------------+----------------+----------------+
         | quest9         | 40             | 180GB          | buyin/short    |
         +----------------+----------------+----------------+----------------+
         | quest10        | 52             | 180GB          | short/nor      |
         |                |                |                | mal/long/buyin |
         +----------------+----------------+----------------+----------------+
         | quest11        | 64             | 243GB          | short/buyin    |
         +----------------+----------------+----------------+----------------+

      To drive home this point, imagine you made the following request:

      ::

         #SBATCH --nodes=1
         #SBATCH --ntasks-per-node=30
         #SBATCH --partition=short

      This request would eliminate Slurm’s ability to match you with any
      of the computers from generation quest8 and would increase the
      amount of time it will take to schedule your job as only one type
      of compute node is able to match your request.

      .. rubric:: Required memory
         :name: section-required-memory

      There are two methods for specifying how much memory/RAM you need,
      the ``--mem`` option which indicates how much memory you
      want\ *per node*.

      ``#SBATCH --mem=<memory per node>G``

      or the ``--mem-per-cpu`` option which indicates how much
      memory/RAM you need *per CPU.*

      ``#SBATCH --mem-per-cpu=<memory per cpu>G``

      If your job submission script does not specify how much memory
      your job requires, then the default setting is 3.25 GB of memory
      per core.

      ``#SBATCH --mem-per-cpu=3256M``

      Therefore, you submitted a job to run on 10 cores and did not
      specify your memory request in your job submission script, Slurm
      will allocate 32.5 GB in total.

      The memory that is allocated to your job via this setting creates
      a **hard upper limit** and your application cannot access memory
      beyond what Slurm reserves for them; if your job tries to access
      more memory than has been reserved, it will be terminated.

      There is a special setting to request the entire memory of the
      computer.

      ``#SBATCH --mem=0``

      How much memory this ends up being will depend on what
      generation/family of computer Slurm matches you to. The following
      is a table which summarizes the relevant information.

      .. container:: table-responsive

         +----------------+----------------+----------------+----------------+
         | Node Family    | Number of CPUs | Amount of      | Partitions     |
         | Name           |                | *              | with these     |
         |                |                | *Schedulable** | Nodes          |
         |                |                | Memory/RAM     |                |
         +================+================+================+================+
         | quest8         | 28             | 84GB           | short/nor      |
         | (general       |                |                | mal/long/buyin |
         | access)        |                |                |                |
         +----------------+----------------+----------------+----------------+
         | quest8 (buyin) | 28             | 180GB          | buyin/short    |
         +----------------+----------------+----------------+----------------+
         | quest9         | 40             | 180GB          | buyin/short    |
         +----------------+----------------+----------------+----------------+
         | quest10        | 52             | 180GB          | short/nor      |
         |                |                |                | mal/long/buyin |
         +----------------+----------------+----------------+----------------+
         | quest11        | 64             | 243GB          | short/buyin    |
         +----------------+----------------+----------------+----------------+

      A final consideration when selecting the amount of memory/RAM is
      the available memory/RAM on each of the different
      generations/families of compute nodes that make up Quest. To drive
      home this point, imagine you made the following request:

      ::

         #SBATCH --nodes=1
         #SBATCH --mem=130G
         #SBATCH --partition=short

      This request would eliminate Slurm’s ability to match you with any
      of the computers from generation quest8 and would increase the
      amount of time it will take to schedule your job as you will have
      reduced the pool of available compute nodes.

      .. rubric:: How can I tell if my job needs more memory to run
         successfully?
         :name: how-can-i-tell-if-my-job-needs-more-memory-to-run-successfully

      Use the ``sacct -X`` command to see information about your recent
      jobs, for example:

      .. code:: code

         $ sacct -X
                    JobID    JobName  Partition    Account  AllocCPUS      State ExitCode 
             ------------ ---------- ---------- ---------- ---------- ---------- -------- 
             1273539      lammps-te+      short     p1234          40  COMPLETED      0:0 
             1273543      vasp-open+      short     p1234          40 OUT_OF_ME+    0:125

      | 
      | The “State” field is the status of your job when it finished.
        Jobs with a “COMPLETED” state have run without system errors.
        Jobs with an “OUT_OF_ME+” state have run out of memory and
        failed. “OUT_OF_ME+” jobs need to request more memory in their
        job submission scripts to complete successfully.
      | If the job you’re investigating is not recent enough to be
        listed by ``sacct -X``, add date fields to the command to see
        jobs between specific start and end dates. For example, to see
        all jobs between September 15, 2019 and September 16, 2019:

      .. code:: code

         $ sacct -X --starttime=091519 --endtime=091619

      | 
      | Specify the date using MMDDYY. More information on sacct is
        available `here <https://slurm.schedmd.com/sacct.html>`__.

      .. rubric:: My job ran out of memory and failed, now what?
         :name: my-job-ran-out-of-memory-and-failed-now-what

      First, determine how much memory your job needs following the
      steps outlined below. Once you know how much memory your job
      needs, edit your job submission script to reserve that amount of
      memory + 10% for your job.

      To find your job’s memory utilization on a compute node:

      #. create a test job by editing your job’s submission script to
         reserve all of the memory of the node it runs on
      #. run your test job
      #. confirm your test job has completed successfully
      #. use ``seff`` to see how much memory your job actually used.

      *Create a test job*

      To profile your job’s memory usage, create a test job by modifying
      your job’s submission script to include the lines:

      .. code:: code

         #SBATCH --mem=0
         #SBATCH --nodes=1

      | Setting ``--mem=0`` reserves all of the memory on the node for
        your job; if you already have a ``--mem=`` directive in your job
        submission script, comment it out. Now your job will not run out
        of memory unless your job needs more memory than is available on
        the node.
      | Setting ``--nodes=1`` reserves a single node for your job. For
        jobs that run on multiple nodes such as MPI-based programs,
        request the number of nodes that your job utilizes. Be sure to
        specify a value for ``#SBATCH --nodes=`` or the cores your job
        submission script reserves could end up on as many nodes as
        cores requested. Be aware that by setting ``--mem=0``, you will
        be reserving all the memory on each of those nodes that your
        cores are reserved on.

      | 2) *Run your test job
        *
      | Submit your test job to the cluster with the ``sbatch`` command.
        For interactive jobs, use ``srun`` or ``salloc``.
      | 3) *Did your test job complete successfully?*
      | When your job has stopped running, use the ``sacct -X`` command
        to confirm your job finished with state “COMPLETED”. If your
        test job finishes with an “OUT_OF_ME+” state, confirm that you
        are submitting the modified job submission script that requests
        all of the memory on the node. If the “OUT_OF_ME+” errors
        persist, your job may require more memory than is available on
        the compute node it ran on. In this case, please email
        quest-help@northwestern.edu for assistance.
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
             

      Check the job State reported in the 4th line. If it is “COMPLETED
      (exit code 0)”, look at the last two lines. “Memory Utilized” is
      the amount of memory your job used, in this case 60Gb.

      If the job State is FAILED or CANCELLED, the Memory Efficiency
      percentage reported by ``seff``\ will be extremely inaccurate. The
      ``seff`` command only works on jobs that have COMPLETED
      successfully.

      .. rubric:: How much memory should I reserve in my job script?
         :name: how-much-memory-should-i-reserve-in-my-job-script

      It’s a good idea to reserve slightly more memory than your job
      utilized since the same job may require slightly different amounts
      of memory depending on variations in data it processes in each run
      of the job. To correctly reserve memory for this job, edit your
      test job submission script to modify the ``#SBATCH --mem=``
      directive to reserve 10% more than 60Gb in the job submission
      script:

      .. code:: code

         #SBATCH --mem=66G

      For jobs that use MPI, remove the ``#SBATCH --mem=`` directive
      from your job submission script. Now specify the amount of memory
      you’d like to reserve per core instead. For example, if your job
      uses 100Gb of memory total and runs on 10 cores, reserve 10Gb plus
      a safety factor per cpu:

      .. code:: code

         #SBATCH --mem-per-cpu=11G

      If it doesn’t matter how many nodes your cores are distributed on,
      you may remove the ``#SBATCH --nodes=`` directive as well.

      Be careful not to reserve significant amounts of memory beyond
      what your job requires as your job’s wait time will increase.
      Reserving excessive memory also wastes shared resources that could
      be used by other researchers.

      .. rubric:: Standard Output/Error
         :name: section-output-error

      To specify a file into which *both* the standard output *and*
      standard error from your job will be written, please include
      *only* the ``-o/--output`` option in your job submission script:

      ``#SBATCH --output=<name of file>.out``

      This will cause a file to be created in the submission directory
      with the name you defined. You may also specify filename which
      includes the absolute or full path to the file **but you cannot
      just include a path to a directory**. Please make sure that all
      directories in the file path name exist on Quest.

      To separate out the standard output and standard error into two
      separate files, please include *both* the ``-o/--output`` option
      *and* the ``-e/--error`` option in your job submission script:

      .. code:: code

         #SBATCH --output=<name of file>.out
         #SBATCH --error=<name of file>.err

      If you do not include either option, the default setting will be
      to write both the standard output and standard error from your job
      in a file called

      ``slurm-<slurm jobid>.out``
      where ``<slurm jobid>`` is the ID given to your job by SLURM. You
      can replicate this default naming scheme yourself by providing the
      following option:

      ``#SBATCH --output=slurm-%j.out``

      In addition to ``%j`` which will add the job id to the name of
      your output file, there is also ``%x`` which will add the job name
      to the name of your output file.

      .. rubric:: Job Name
         :name: section-job-name

      To specify a name for your job, please include a ``-J/--job-name``
      option in your job submission script:

      ``#SBATCH --job-name=<job name>``

      The default is to set this value to the name of the submission
      file, so we recommend that you set this to a memorable string
      because it can be useful when trying to identify a specific job
      among several running or completed jobs.

      .. rubric:: Sending e-mail alerts about your job
         :name: section-email

      To receive e-mails regarding the status of your Slurm jobs, please
      include *both* the ``--mail-type`` option *and* the
      ``--mail-user`` option in your job submission script:

      .. code:: code

         #SBATCH --mail-type=<job state that triggers email> ## BEGIN, END, FAIL or ALL
         #SBATCH --mail-user=<email address>

      If you do not include both of these options, then you will not
      receive emails from Slurm. Also, you can include any combination
      of BEGIN, END, FAIL as an argument for this option.

      .. rubric:: Constraints
         :name: section-constraints

      To specify an architecture constraint for your job, please include
      a ``-C/--constraint`` option in your job submission script:

      ``#SBATCH --constraint=<name of compute node architecture>``

      Not all Quest compute nodes are the same. We currently have four
      different generations or architectures of compute nodes which we
      refer to as quest8, quest9 and quest10 and a summary table of
      these architectures is provided below.

      .. container:: table-responsive

         +----------------+----------------+----------------+----------------+
         | Node Family    | Number of CPUs | Amount of      | Partitions     |
         | Name           |                | *              | with these     |
         |                |                | *Schedulable** | Nodes          |
         |                |                | Memory/RAM     |                |
         +================+================+================+================+
         | quest8         | 28             | 84GB           | short/nor      |
         | (general       |                |                | mal/long/buyin |
         | access)        |                |                |                |
         +----------------+----------------+----------------+----------------+
         | quest8 (buyin) | 28             | 180GB          | buyin/short    |
         +----------------+----------------+----------------+----------------+
         | quest9         | 40             | 180GB          | buyin/short    |
         +----------------+----------------+----------------+----------------+
         | quest10        | 52             | 180GB          | short/nor      |
         |                |                |                | mal/long/buyin |
         +----------------+----------------+----------------+----------------+
         | quest11        | 64             | 243GB          | short/buyin    |
         +----------------+----------------+----------------+----------------+

      | You can find detailed information on each of these architectures
        `here <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/specs.html>`__.
        If you need to restrict your job to a particular architecture,
        you can do so through the constraint directive. For example,
        ``--constraint=quest10`` will cause the scheduler to only match
        you to compute nodes of the quest10 generation.
      | Moreover, if you would like to match to any generation of
        compute nodes, but would like all the compute nodes to be either
        of generation quest8 or quest9 or quest10 or quest11 and not a
        combination of generations, then you can set the following for
        constraint.
      | ``#SBATCH --constraint="[quest8|quest9|quest10|quest11]"``
      | This is recommended for jobs that are parallelized using MPI.

      .. rubric:: All Slurm Configuration Options
         :name: section-all-slurm-options

      .. container:: table-responsive

         +-----------------------+-----------------------+-----------------------+
         | Option                | Slurm (sbatch)        | Default/Required      |
         +=======================+=======================+=======================+
         | Account               | –account=<account>    | Required: Not set by  |
         |                       | -A <account>          | default               |
         +-----------------------+-----------------------+-----------------------+
         | Partition/queue       | –                     | Required: Not set by  |
         |                       | partition=<partition> | default               |
         +-----------------------+-----------------------+-----------------------+
         | Wall time limit       | –time=<hh:mm:ss>      | Required: Not set by  |
         |                       | -t<hh:mm:ss>          | default               |
         +-----------------------+-----------------------+-----------------------+
         | Job name              | –job-name=<name>      | Optional: Default is  |
         |                       | -J <name>             | to set this value to  |
         |                       |                       | the name of the       |
         |                       |                       | submission file.      |
         +-----------------------+-----------------------+-----------------------+
         | Node count            | –nodes=<count>        | Optional: Not set by  |
         |                       | -N <count>            | default               |
         +-----------------------+-----------------------+-----------------------+
         | Core count            | -n <count>            | Optional: Default is  |
         |                       |                       | 1                     |
         |                       | –ntasks=<count>       |                       |
         +-----------------------+-----------------------+-----------------------+
         | Process count per     | –nt                   | Optional: Not set by  |
         | node                  | asks-per-node=<count> | default               |
         +-----------------------+-----------------------+-----------------------+
         | Core count (per       | –                     | Optional: Default is  |
         | process)              | cpus-per-task=<cores> | 1                     |
         +-----------------------+-----------------------+-----------------------+
         | Memory per node       | –mem=<memory>         | Optional: Not set by  |
         |                       | (default unit MB)     | default               |
         +-----------------------+-----------------------+-----------------------+
         | Memory per processor  | –mem-per-cpu=<memory> | Optional: Default is  |
         |                       |                       | 3.25GB                |
         +-----------------------+-----------------------+-----------------------+
         | Request GPUs          | –gr                   | Optional: Not set by  |
         |                       | es=gpu:<type>:<count> | default               |
         +-----------------------+-----------------------+-----------------------+
         | Instead of specifying | -w,                   | Optional: Not set by  |
         | how many nodes you    | –nodelis              | default               |
         | want,                 | t=<node>[,node2[,…]]> |                       |
         | you could request a   | -F, –nodefile=<node   |                       |
         | specific set of       | file>                 |                       |
         | compute nodes.        |                       |                       |
         | This cannot be used   |                       |                       |
         | in combination with   |                       |                       |
         | the ``--nodes=``      |                       |                       |
         | setting.              |                       |                       |
         +-----------------------+-----------------------+-----------------------+
         | Job array             | -a <array indices>    | Optional: Not set by  |
         |                       |                       | default               |
         +-----------------------+-----------------------+-----------------------+
         | Standard output file  | –output=<file path>   | Optional: Not set by  |
         |                       | (path must exist)     | default               |
         +-----------------------+-----------------------+-----------------------+
         | Standard error file   | –error=<file path>    | Optional: Not set by  |
         |                       | (path must exist)     | default               |
         +-----------------------+-----------------------+-----------------------+
         | Combine stdout/stderr | –output=<combined out | Optional: Set to      |
         | to stdout             | and err file path>    | ``slurm-<jobid>.out`` |
         |                       |                       | by default.           |
         +-----------------------+-----------------------+-----------------------+
         | Architecture          | –cons                 | Optional: Not set by  |
         | constraint            | traint=<architecture> | default               |
         |                       | -C <architecture>     |                       |
         +-----------------------+-----------------------+-----------------------+
         | Copy environment      | –export=ALL (default) | Optional: Default is  |
         |                       | –export=NONE ## to    | to export ALL         |
         |                       | not export            | environmental         |
         |                       | environment           | settings from the     |
         |                       |                       | submission            |
         |                       |                       | environment to the    |
         |                       |                       | runtime environment.  |
         +-----------------------+-----------------------+-----------------------+
         | Copy environment      | –export               | Optional: Not set by  |
         | variable              | =<variable[=value][,v | default               |
         |                       | ariable2=value2[,…]]> |                       |
         +-----------------------+-----------------------+-----------------------+
         | Job dependency        | –dependency           | Optional: Not set by  |
         |                       | =after:jobID[:jobID…] | default               |
         |                       | –dependency=a         |                       |
         |                       | fterok:jobID[:jobID…] |                       |
         |                       | –dependency=afte      |                       |
         |                       | rnotok:jobID[:jobID…] |                       |
         |                       | –dependency=af        |                       |
         |                       | terany:jobID[:jobID…] |                       |
         +-----------------------+-----------------------+-----------------------+
         | Request event         | –mail-type=<events>   | Optional: Not set by  |
         | notification          | Note: multiple        | default               |
         |                       | mail-type requests    |                       |
         |                       | may be specified in a |                       |
         |                       | comma separated list: |                       |
         |                       | –mai                  |                       |
         |                       | l-type=BEGIN,END,FAIL |                       |
         +-----------------------+-----------------------+-----------------------+
         | Email address         | –mail-user=<email     | Optional: Not set by  |
         |                       | address>              | default               |
         +-----------------------+-----------------------+-----------------------+
         | Defer job until the   | –begin=<date/time>    | Optional: Not set by  |
         | specified time        |                       | default               |
         +-----------------------+-----------------------+-----------------------+
         | Node exclusive job    | –exclusive            | Optional: Not set by  |
         |                       |                       | default               |
         +-----------------------+-----------------------+-----------------------+

      .. rubric:: Environmental Variables Set by Slurm
         :name: section-slurm-environmental-variables

      The variables in the following table are set by Slurm and they are
      accessbile in the environment of your job after the job has
      started running

      .. container:: table-responsive

         +-----------------------------------+-----------------------------------+
         | Info                              | Slurm                             |
         +===================================+===================================+
         | Job name                          | $SLURM_JOB_NAME                   |
         +-----------------------------------+-----------------------------------+
         | Job ID                            | $SLURM_JOB_ID                     |
         +-----------------------------------+-----------------------------------+
         | Submit directory                  | $SLURM_SUBMIT_DIR                 |
         +-----------------------------------+-----------------------------------+
         | Node list                         | | $SLURM_JOB_NODELIST             |
         |                                   | | $SLURM_NODELIST                 |
         +-----------------------------------+-----------------------------------+
         | Job array index                   | $SLURM_ARRAY_TASK_ID              |
         +-----------------------------------+-----------------------------------+
         | Queue name                        | $SLURM_JOB_PARTITION              |
         +-----------------------------------+-----------------------------------+
         | Number of nodes allocated         | $SLURM_JOB_NUM_NODES              |
         |                                   | $SLURM_NNODES                     |
         +-----------------------------------+-----------------------------------+
         | Number of processes               | $SLURM_NTASKS                     |
         +-----------------------------------+-----------------------------------+
         | Number of processes per node      | $SLURM_TASKS_PER_NODE             |
         +-----------------------------------+-----------------------------------+
         | Requested tasks per node          | $SLURM_NTASKS_PER_NODE            |
         +-----------------------------------+-----------------------------------+
         | Requested CPUs per task           | $SLURM_CPUS_PER_TASK              |
         +-----------------------------------+-----------------------------------+
         | Scheduling priority               | $SLURM_PRIO_PROCESS               |
         +-----------------------------------+-----------------------------------+
         | Job user                          | $SLURM_JOB_USER                   |
         +-----------------------------------+-----------------------------------+
         | Log In Node from which this job   | $SLURM_SUBMIT_HOST                |
         | was submitted.                    |                                   |
         +-----------------------------------+-----------------------------------+

      .. rubric:: SLURM Commands and Job Management
         :name: all-slurm-commands-and-submission-options

      In this section, we discuss how to manage batch jobs after they
      have been submitted on Quest. This includes how to monitor jobs
      currently pending or running, how to cancel jobs, and how to check
      on the status of past jobs.

      .. rubric:: Table of Common Slurm Commands
         :name: section-common-slurm-commands

      .. container:: table-responsive

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
         | Show expected start time          | squeue -j <job ID> –start         |
         +-----------------------------------+-----------------------------------+
         | Queue list / info                 | scontrol show partition [queue]   |
         +-----------------------------------+-----------------------------------+
         | Hold a job                        | scontrol hold <job ID>            |
         +-----------------------------------+-----------------------------------+
         | Release a job                     | scontrol release <job ID>         |
         +-----------------------------------+-----------------------------------+
         | Start an interactive job          | salloc <args>                     |
         |                                   | srun –pty <args>                  |
         +-----------------------------------+-----------------------------------+
         | X forwarding                      | srun –pty <args> –x11             |
         +-----------------------------------+-----------------------------------+
         | Monitor or review a job’s         | sacct -j <job_num> –format        |
         | resource usage                    | JobID,jobname,NTask               |
         |                                   | s,nodelist,CPUTime,ReqMem,Elapsed |
         |                                   | (see sacct for all format         |
         |                                   | options)                          |
         +-----------------------------------+-----------------------------------+
         | View job batch script             | sacct -j <job_num>                |
         |                                   | -B/–batch-script                  |
         +-----------------------------------+-----------------------------------+

      .. rubric:: The ``squeue`` Command
         :name: squeue

      The ``squeue`` command can be used display information about your
      current jobs on Quest.

      .. container:: table-responsive

         ======================== =============================================
         Command                  Description
         ======================== =============================================
         squeue -u <NetID>        Show only jobs belonging to user specified
         squeue -A <AllocationID> Show only jobs belonging to account specified
         squeue -j <JobID>        Display the status of the specified job
         squeue -t R              Show running jobs
         squeue -t PD             Show pending jobs
         squeue –help             See documentation and additional options
         ======================== =============================================

      .. rubric:: The ``sacct`` Command
         :name: sacct

      The ``sacct`` command can be used display information about your
      past and current jobs on Quest.

      *Starting with Slurm 22.05, there is a new feature of ``sacct``
      which allows users to query the job submission script for a given
      Slurm job for up to 1 year.* The syntax is as follows:

      ::

         sacct -j <job_num> -B

      Please note that the ``-j`` option is required to query the
      submission script. Please find an example of this use of ``sacct``
      below.

      ::

         $ sacct -j 1454461 -B
         Batch Script for 1454461
         --------------------------------------------------------------------------------
         #!/bin/bash
         #SBATCH -A a9009
         #SBATCH -p all
         #SBATCH --gres=gpu:p100:1
         #SBATCH --output=anaconda_pytorch.txt
         #SBATCH --job=anaconda_pytorch
         #SBATCH -N 1
         #SBATCH -n 1
         #SBATCH -t 0:10:00
         #SBATCH --mem=20G
         #SBATCH --nodelist=qgpu6038

         module purge all
         module load python-miniconda3/4.12.0
         source activate /projects/intro/envs/pytorch-1.10-py38
         python training_pytorch.py
         source deactivate
         source activate /projects/intro/envs/pytorch-1.11-py38
         python training_pytorch.py

      Additionally, ``sacct`` can be used to provide specific
      information about your Slurm jobs. Below, we demonstrate the
      default/basic behavior of ``sacct``. By default, ``sacct`` will
      only display information about jobs from today and only a subset
      of information: the jobid, jobname, partition, account, AllocCPUS,
      state, and exitcode.

      ::

         $ sacct -X -u <netid>
         JobID      JobName Partition  Account AllocCPUS   State ExitCode
         ------------ ---------- ---------- ---------- ---------- ---------- --------
         1453894      bash    all   a9009     1 COMPLETED   0:0
         1454434   sample_job    all   a9009     52   FAILED   6:0 

      The standard output of ``sacct`` may not provide the information
      we want. To remedy this, we can use the ``--format`` flag to
      choose what we want in our output. The format flag is handled by a
      list of comma separated variables which specify output data:

      ::

         $ sacct --user=<netid> --format=var_1,var_2, ... ,var_N

      A chart of some variables is provided below:

      .. container:: table-responsive

         ======== ============================================================
         Variable Description
         ======== ============================================================
         account  Account the job ran under.
         cputime  Formatted (Elapsed time \* CPU) count used by a job or step.
         elapsed  Jobs elapsed time formated as DD-HH:MM:SS.
         exitcode The exit code returned by the job script or salloc.
         jobid    The id of the Job.
         jobname  The name of the Job.
         ncpus    Amount of allocated CPUs.
         nnodes   The number of nodes used in a job.
         ntasks   Number of tasks in a job.
         priority Slurm priority.
         qos      Quality of service.
         user     Username of the person who ran the job.
         ======== ============================================================

      In addition, only jobs from today will be displayed. To change
      this, you can supply a start time and an end time via the
      ``--starttime`` and ``--endtime`` flags, respectively. For
      example, suppose you want to find information about jobs that were
      run on September 12, 2022 and you want to show information
      regarding the job name, the number of nodes used in the job, the
      number of cpus, and the elapsed time. Your command would look like
      this:

      ::

         $ sacct -X --starttime=09/12/22 --format=jobname,nnodes,ncpus,elapsed

      A full list of variables that specify data handled by ``sacct``
      can be found with the ``--helpformat`` flag or by `visiting the
      slurm page on sacct <https://slurm.schedmd.com/sacct.html>`__.

      .. rubric:: The ``seff`` Command
         :name: seff

      To see the maximum amount of memory used/needed by your job run
      the command: ``seff <job_id>``. This returns output similar to:

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

      Check the job State reported in the 4th line. If it is “COMPLETED
      (exit code 0)”, look at the last two lines. “Memory Utilized” is
      the amount of memory your job used, in this case 60Gb.

      If the job State is FAILED or CANCELLED, the Memory Efficiency
      percentage reported by ``seff``\ will be extremely inaccurate.
      *The ``seff`` command only works on jobs that have COMPLETED
      successfully.*

      .. rubric:: The ``checkjob`` Command
         :name: checkjob

      The ``checkjob`` command displays detailed information about a
      submitted job’s status and diagnostic information that can be
      useful for troubleshooting submission issues. It can also be used
      to obtain useful information about completed jobs such as the
      allocated nodes, resources used, and exit codes.

      Example usage:

      .. code:: code

         checkjob <JobID>

      where you can get your <JobID> using the squeue commands above.

      .. rubric:: Example for a Successfully Running Job
         :name: example-for-a-successfully-running-job

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
      -  The time passed after job start and the total walltime request
         are given with RunTime and TimeLimit.
      -  The node name(s) are listed after NodeList.
      -  The paths to job’s working directory (WorkDir), standard error
         (StdErr) and output (StdOut) files are given.
      -  If a batch job script is used for submission, the script is
         presented at the end.

      .. rubric:: Cancelling Jobs
         :name: scancel

      You can cancel one or all of your jobs with ``scancel``. Proceed
      with caution, as this cannot be undone, and you will not be
      prompted for confirmation after issuing the command.

      .. container:: table-responsive

         ================== ===============================
         Command            Description
         ================== ===============================
         scancel <JobID>    Cancel the job with given ID
         scancel -u <NetID> Cancel all the jobs of the user
         ================== ===============================

      .. rubric:: Holding, Releasing, or Modifying Jobs
         :name: holding-releasing-or-modifying-jobs

      Users can place their jobs in a “JobHeldUser” state while
      submitting the job or after the job has been queued. Running jobs
      cannot be placed on hold.

      .. container:: table-responsive

         ===================== =============================================
         Command               Description
         ===================== =============================================
         #SBATCH -H            Place hold within job script
         sbatch -H <jobscript> Place hold while submitting from command line
         scontrol hold <jobID> Place hold on a queued job from command line
         ===================== =============================================

      The job status will be shown in the output of monitoring commands
      such as ``squeue`` or ``checkjob``.

      To release a job from user hold state:

      .. code:: code

         scontrol release <JobID>

      The job control command (``scontrol``) can also be used for
      changing the parameters of a submitted job **before** it starts
      running. The following parameters can be modified safely:

      -  Job dependency (change to “none”)
      -  Partition (queue)
      -  Job name
      -  Wall clock limit
      -  Allocation

      The table below contains some useful examples of using
      ``scontrol`` to change a job’s parameters.

      .. container:: table-responsive

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

      For a complete listing of scontrol options, see the official
      `scontrol
      documentation <https://slurm.schedmd.com/scontrol.html>`__.

      .. rubric:: Probing Priority
         :name: probing-priority

      Slurm implements a multi-factor priority scheme for ordering the
      queue of jobs waiting to be run. ``sprio`` command is used to see
      the contribution of different factors to a pending job’s
      scheduling priority.

      .. container:: table-responsive

         +------------------+--------------------------------------------------+
         | Command          | Description                                      |
         +==================+==================================================+
         | sprio            | Show scheduling priority for all pending jobs    |
         |                  | for the user                                     |
         +------------------+--------------------------------------------------+
         | sprio -j <jobID> | Show scheduling priority of the defined job      |
         +------------------+--------------------------------------------------+

      For running jobs, you can see the starting priority using
      ``checkjob <jobID>`` command.

      .. rubric:: Special Types of Job Submissions
         :name: section-special-job-types

      In this section, we provide details and examples of how to use
      Slurm to run interactive jobs, job arrays, and jobs that depend on
      each other.

      .. rubric:: Interactive Job Examples
         :name: section-interactive-jobs

      .. rubric:: Submitting an Interactive Job (to run an application
         *without* Graphical User Interface - GUI)
         :name: section-section-interactive-jobs-non-gui

      To launch an interactive job from the Quest log-in node in order
      to run an application *without* a GUI, use either the
      `srun <https://slurm.schedmd.com/srun.html>`__ or
      `salloc <https://slurm.schedmd.com/salloc.html>`__ command. If you
      use ``srun`` to run an interactive job, then SLURM will
      automatically launch a terminal session on the compute node after
      it schedules the job and you simply need to wait for this to
      happen. *Due to the behavior of* ``srun``\ *, if you lose
      connection to your interactive session, the interactive job will
      terminate.*

      .. code:: code

         [quser23 ~]$srun -N 1 -n 1 --account=<account> --mem=<memory>G --partition=<partition> --time=<hh:mm:ss> --pty bash -l
         srun: job 3201233 queued and waiting for resources
         srun: job 3201233 has been allocated resources
         ----------------------------------------
         srun job start: Mon Mar 14 13:25:41 CDT 2022
         Job ID: 3201233
         Username: <netid>
         Queue: <partition>
         Account: <account>
         ----------------------------------------
         The following variables are not
         guaranteed to be the same in
         prologue and the job run script
         ----------------------------------------
         PATH (in prologue) : /hpc/usertools:/usr/lib64/qt-3.3/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/lpp/mmfs/bin:/opt/ibutils/bin
         WORKDIR is: /home/<netid>
         ----------------------------------------
         [qnode0114 ~]$

      If you use ``salloc`` instead, it will *not* automatically launch
      a terminal session on the compute node. Instead, after it
      schedules your job/request, it will tell you the name of the
      compute node at which point you can run ``ssh qnodeXXXX`` to
      directly connect to the compute node. *Due to the behavior of*
      ``salloc``\ *, if you lose connection to your interactive session,
      the interactive job will*\ **not**\ *terminate.
      *

      ::

         [quser21 ~]$ salloc -N 1 -n 1 --account=<account> --mem=&ltmemory>G --partition=<partition> --time=<hh:mm:ss>
         salloc: Pending job allocation 276305
         salloc: job 276305 queued and waiting for resources
         salloc: job 276305 has been allocated resources
         salloc: Granted job allocation 276305
         salloc: Waiting for resource configuration
         salloc: Nodes qnode8029 are ready for job
         [quser21 ~]$ ssh qnode8029
         Warning: Permanently added 'qnode8029,172.20.134.29' (ECDSA) to the list of known hosts.
         [qnode8029 ~]$

      .. rubric:: Submitting an Interactive Job (to run an application
         *with* Graphical User Interface)
         :name: section-section-interactive-jobs-gui

      To launch an interactive job from the Quest log-in node in order
      to run an application *with* a GUI, first you need to connect to
      Quest using an application with X11 forwarding support. We
      recommend `using the FastX3
      client <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1511>`__.
      Once you have connected to Quest with X11 forwarding enabled, you
      can then use either the
      `srun <https://slurm.schedmd.com/srun.html>`__ or
      `salloc <https://slurm.schedmd.com/salloc.html>`__ command. If you
      use ``srun`` to run an interactive job, then SLURM will
      automatically launch a terminal session on the compute node after
      it schedules the job and you simply need to wait for this to
      happen. *Due to the behavior of* ``srun``\ *, if you lose
      connection to your interactive session, the interactive job will
      terminate.*

      .. code:: code

         [quser23 ~]$srun --x11 -N 1 -n 1 --account=<account> --mem=<memory>G --partition=<partition> --time=<hh:mm:ss> --pty bash -l
         srun: job 3201233 queued and waiting for resources
         srun: job 3201233 has been allocated resources
         ----------------------------------------
         srun job start: Mon Mar 14 13:25:41 CDT 2022
         Job ID: 3201233
         Username: <netid>
         Queue: <partition>
         Account: <account>
         ----------------------------------------
         The following variables are not
         guaranteed to be the same in
         prologue and the job run script
         ----------------------------------------
         PATH (in prologue) : /hpc/usertools:/usr/lib64/qt-3.3/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/lpp/mmfs/bin:/opt/ibutils/bin
         WORKDIR is: /home/<netid>
         ----------------------------------------
         [qnode0114 ~]$

      If you use ``salloc`` instead, it will *not* automatically launch
      a terminal session on the compute node. Instead, after it
      schedules your job/request, it will tell you the name of the
      compute node at which point you can run ``ssh qnodeXXXX`` to
      directly connect to the compute node. *Due to the behavior of*
      ``salloc``\ *, if you lose connection to your interactive session,
      the interactive job will*\ **not**\ *terminate.*

      ::

         [quser21 ~]$ salloc --x11 -N 1 -n 1 --account=<account> --mem=<XXG> --partition=<partition> --time=<hh:mm:ss>
         salloc: Pending job allocation 276305
         salloc: job 276305 queued and waiting for resources
         salloc: job 276305 has been allocated resources
         salloc: Granted job allocation 276305
         salloc: Waiting for resource configuration
         salloc: Nodes qnode8029 are ready for job
         [quser21 ~]$ ssh -X qnode8029
         Warning: Permanently added 'qnode8029,172.20.134.29' (ECDSA) to the list of known hosts.
         [qnode8029 ~]$

      .. rubric:: Job Array
         :name: section-job-array

      Job arrays can be used to submit multiple jobs at once that use
      the same application script. This can be useful if you want to run
      the same script multiple times with different input parameters.

      In the example below, the –array option defines the job array,
      with a specification of the index numbers you want to use (in this
      case, 0 through 9). The $SLURM_ARRAY_TASK_ID bash environmental
      variable takes on the value of the job array index for each job
      (so here, integer values 0 through 9, one value for each job). In
      this example, the value of $SLURM_ARRAY_TASK_ID is used to select
      the correct index from the input_args bash array which was
      constructed by reading in *input_args.txt*, each row of which is
      then passed on to a script as command line arguments.

      .. code:: filenameheader

         jobsubmission.sh

      .. code:: code

         #!/bin/bash
         #SBATCH --account=w10001  ## YOUR ACCOUNT pXXXX or bXXXX
         #SBATCH --partition=w10001  ### PARTITION (buyin, short, normal, w10001, etc)
         #SBATCH --array=0-9 ## number of jobs to run "in parallel" 
         #SBATCH --nodes=1 ## how many computers do you need
         #SBATCH --ntasks-per-node=1 ## how many cpus or processors do you need on each computer
         #SBATCH --time=00:10:00 ## how long does this need to run (remember different partitions have restrictions on this param)
         #SBATCH --mem-per-cpu=1G ## how much RAM do you need per CPU (this affects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --job-name="sample_job_\${SLURM_ARRAY_TASK_ID}" ## use the task id in the name of the job
         #SBATCH --output=sample_job.%A_%a.out ## use the jobid (A) and the specific job index (a) to name your log file
         #SBATCH --mail-type=ALL ## you can receive e-mail alerts from SLURM when your job begins and when your job finishes (completed, failed, etc)
         #SBATCH --mail-user=email@u.northwestern.edu  ## your email

         module purge all
         module load python-anaconda3
         source activate /projects/intro/envs/slurm-py37-test

         IFS=$'\n' read -d '' -r -a input_args < input_args.txt

         python slurm_test.py --filename ${input_args[$SLURM_ARRAY_TASK_ID]}

      where *input_args.txt* contains the following:

      .. code:: filenameheader

         input_args.txt

      .. code:: code

         filename1.txt
         filename2.txt
         filename3.txt
         filename4.txt
         filename5.txt
         filename6.txt
         filename7.txt
         filename8.txt
         filename9.txt
         filename10.txt

      and *myscript.py* contains the following code:

      .. code:: filenameheader

         myscript.py

      .. code:: code

         import argparse
         import time

         def parse_commandline():
             """Parse the arguments given on the command-line.
             """
             parser = argparse.ArgumentParser(description=__doc__)
             parser.add_argument("--filename",
                                help="Name of file",
                                default=None)


             args = parser.parse_args()

             return args


         ###############################################################################
         # BEGIN MAIN FUNCTION
         ###############################################################################
         if __name__ == '__main__':
             args = parse_commandline()
             #time.sleep(10) # Sleep for 3 seconds
             print(args.filename)

      In this example, *myscript.py* will receive the values in
      *input.csv* as arguments: the first field will be sys.argv[1], the
      second field will be sys.argv[2], etc.

      **Note: make sure the number you specify for the –array parameter
      matches the number of lines in your input file!**

      Also, note that in this example standard output and error files
      are printed separately for each element of the job array with the
      –output and –error options. To avoid each element overwriting
      these files, tag them with jobID (%A) and elementID (%a) variables
      (which are automatically assigned by the scheduler) so elements
      have their own distinct output and error files. An example of this
      is shown in the *jobsubmission.sh* presented above.

      Submit this script with:

      .. code:: code

         sbatch jobsubmission.sh

      The job array will then be submitted to the scheduler with each
      array element requesting the same resources (such as number of
      cores, time, memory etc.) as defined in the job submission script.

      .. rubric:: Dependent Jobs
         :name: section-dependent-jobs

      Dependent jobs are a series of jobs which run or wait to run
      conditional on the state of another job. For instance, you may
      submit two jobs and you want the first job to complete
      successfully before the second job runs. In order to submit this
      type of workflow, you pass **sbatch** the jobid of the job that
      needs to finish before this job starts via the command line
      argument:
      ::

         --dependency=afterok:<jobid>

      To accomplish this, it is helpful to write all of your **sbatch**
      commands in bash script. You will notice that anything you can
      tell slurm via #SBATCH in the submission script itself, you can
      also pass to **sbatch** via the command line. The key here is that
      the bash variable *jid0, jid1, jid2* will contain the jobid that
      SLURM assigns after you run the **sbatch** command.

      .. code:: filenameheader

         wrapper_script.sh

      .. code:: code

         #!/bin/bash
         jid0=($(sbatch --time=00:10:00 --account=w10001 --partition=w10001 --nodes=1 --ntasks-per-node=1 --mem=8G --job-name=example --output=job_%A.out example_submit.sh))

         echo "jid0 ${jid0[-1]}" >> slurm_ids

         jid1=($(sbatch --dependency=afterok:${jid0[-1]} --time=00:10:00 --account=w10001 --partition=w10001 --nodes=1 --ntasks-per-node=1 --mem=8G --job-name=example --output=job_%A.out --export=DEPENDENTJOB=${jid0[-1]} example_submit.sh))

         echo "jid1 ${jid1[-1]}" >> slurm_ids

         jid2=($(sbatch --dependency=afterok:${jid1[-1]} --time=00:10:00 --account=w10001 --partition=w10001 --nodes=1 --ntasks-per-node=1 --mem=8G --job-name=example --output=job_%A.out --export=DEPENDENTJOB=${jid1[-1]} example_submit.sh))

         echo "jid2 ${jid2[-1]}" >> slurm_ids

      In the above, the second job will not start until the first job is
      finished and the third job will not start until the second one is
      finished. The actual submission script that is being run is below.

      .. code:: filenameheader

         example_submit.sh

      .. code:: code

         #!/bin/bash
         #SBATCH --mail-type=ALL ## you can receive e-mail alerts from SLURM when your job begins and when your job finishes (completed, failed, etc)
         #SBATCH --mail-user=email@u.northwestern.edu ## your email

         if [[ -z "${DEPENDENTJOB}" ]]; then
             echo "First job in workflow"
         else
             echo "Job started after " $DEPENDENTJOB
         fi

         module purge all
         module load python-anaconda3
         source activate /projects/intro/envs/slurm-py37-test

         python --version
         python myscript.py --job-id $DEPENDENTJOB

      where *myscript.py* contains the following code:

      .. code:: filenameheader

         myscript.py

      .. code:: code

         import argparse
         import time


         def parse_commandline():
             """Parse the arguments given on the command-line.
             """
             parser = argparse.ArgumentParser(description=__doc__)
             parser.add_argument("--job-id",
                                help="Job number",
                                default=0)

             args = parser.parse_args()

             return args


         ###############################################################################
         # BEGIN MAIN FUNCTION
         ###############################################################################
         if __name__ == '__main__':
             args = parse_commandline()
             time.sleep(3) # Sleep for 3 seconds
             print(args.job_id)

      In this example, we print the job id that had to finish in order
      for the dependent job to begin. Therefore, the very first job
      should print 0 because it did not rely on any job to finish in
      order to run but the second job should print the jobid of the
      first job and so on.

      .. code:: code

         bash wrapper_script.sh

      This will submit the three jobs in sequence and you should see
      jobs 2 and 3 pending for reason DEPENDENCY.

      .. rubric:: Factors Affecting Job Scheduling on Quest
         :name: section-job-scheduling

      If your job is waiting on the queue, the reason is most probably
      one of the following:

      -  Your job’s score is lower compared to others
      -  Unavailable/occupied compute resources at that moment.

      .. rubric:: Priority
         :name: priority

      Total priority score is combination of several factors. These
      factors are the following:

      **i. Fair Share** Quest’s job scheduler uses a fair share
      mechanism to dynamically determine a score. The calculation is
      based on the comparison between your share of the resources and
      your actual usage of these resources. If you or other members of
      your allocation used large amounts of resources in the recent
      past, the priority of current jobs will be lower. Accordingly,
      your jobs will wait longer before they start if the scheduler
      queue is busy.

      On the other hand, if the job queue is empty and the compute
      resources are idling, regardless of the priority, your jobs will
      run. You will never run out of compute hours in this model.

      Fairshare also includes a recovery mechanism for job priority. The
      contribution of past resource usage to priority calculations
      decays over time. Without new usage, the job scores will be
      restored significantly within a month.

      If you are using a general access allocation, the fair share
      scores of your jobs will be affected from the overall resource
      usage by all members of the allocation. If you are using a buy-in
      allocation that has its own compute nodes, your own usage of the
      those nodes will be the determining factor for your job’s fair
      share score.

      **ii. Allocation Type** There are two types of allocations for
      research projects on Quest. Research I allocations are ideal for
      small to moderate computational needs whereas Research II
      allocations require considerably more resources. Due to this
      difference in computational needs, a Research II allocation has a
      higher share of the system resources (or initial fairshare score)
      compared to a Research I allocation. This is equivalent to
      receiving more compute hours with a Research II than a Research I.

      **iii. Job Age** Age is length of time an eligible job has been
      waiting in the queue. Jobs will accumulate priority proportional
      to their age. This can help overcome starting priority differences
      between jobs coming from other actors.

      **iv. Partition** A priority score is associated with partition
      that the job uses. This is only applicable for certain buy-in
      allocations which have multiple partitions.

      More detailed information about Slurm’s multi-factor priority
      system, please see this
      `page <https://slurm.schedmd.com/priority_multifactor.html>`__.

      .. rubric:: Backfill Scheduling
         :name: backfill-scheduling

      There is a secondary mechanism that starts lower priority jobs on
      slots reserved by higher priority jobs while these jobs are
      acquiring their full set of resources. This is called “backfill”
      scheduling which helps to increase the utilization of the compute
      nodes and guarantees no delay in starting the higher priority
      jobs. To benefit from this mechanism, it is important to
      accurately request resources (wall time, core, memory) so that the
      scheduler can find appropriate space on the resource map. Please
      review `resource utilization
      page <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1695>`__
      for different methods you can use to identify your job’s needs.

      From time to time, compute resources cannot be backfilled
      effectively and nodes/cores may appear idle while jobs are waiting
      on the queue.

      .. rubric:: Diagnosing Issues with Your Job Submission Script
         and/or Your Job Itself
         :name: section-diagnosing-jobs

      .. rubric:: Debugging a Job Submission Script Rejected By The
         Scheduler
         :name: section-debugging-jobs

      If your job submission script generates an error when you submit
      it with the **sbatch** command, the problem in your script is in
      one or more of the lines that begin with #SBATCH. To debug job
      scripts that generate errors, look up the error message in the
      section below to identify the most likely reason your script
      received that error message. Once you have identified the mistake
      in your script, edit your script to correct it and re-submit your
      job. If you receive the same error message again, examine the
      error message and the mistake in your script more closely.
      Sometimes the same error message can be generated by two different
      mistakes in the same script, meaning it is possible that you may
      resolve the first mistake but need to correct a second mistake to
      clear that particular error message. Mistakes can be difficult to
      identify, and often require careful reading of your #SBATCH lines.

      When you re-submit your job you may receive a new error message.
      This means the mistake that generated the first error message has
      been resolved, and now you need to fix another mistake. Slurm
      returns up to two distinct error messages at a time. If your
      submission script has more than two mistakes, you will need to
      re-submit your job multiple times to identify and fix all of them.

      When Slurm encounters a mistake in your job submission script, it
      does not read the rest of your script that comes after the
      mistake. If the mistake generates an error, you can fix it and
      resubmit your job, however not all mistakes generate errors. If
      your script’s required elements (account, partition, nodes, cores,
      and wall time) have been read successfully before Slurm encounters
      your mistake, your job will still be accepted by the scheduler and
      run, just not the way you expect it to. Scripts with mistakes that
      don’t generate errors still need to be debugged since the
      scheduler has ignored some of your #SBATCH lines. You can identify
      a script with mistakes if the output from your job is unexpected
      or incorrect.

      To use the references below, search for the exact error message
      generated by your job. Some error messages appear to be similar
      but are generated by different mistakes.

      Note that the errors listed in this document may also be generated
      by interactive job submissions using ``srun`` or ``salloc``. In
      those cases, the error messages will begin with ``srun`` error or
      ``salloc`` error. The procedure to resolve these error messages is
      the same.

      With certain combinations of GUI editors and character sets on
      your personal computer, copying and pasting into Quest job
      submission scripts may bring in specific hidden characters that
      interfere with the scheduler’s ability to interpret the script. In
      these cases, #SBATCH lines will have no mistakes but still
      generate errors when submitted to the scheduler. To see all of the
      hidden characters in your job submission script, use the command
      cat -A <script_name>. To resolve this, you may need to type your
      submission script into a native unix editor like vi and not use
      copy and paste.

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: –account option required or sbatch: error:
            Unable to allocate resources: Invalid account or
            account/partition combination specified

         .. container::
         panel panel-body js-panelnormalswitches0 collapse

            | Location of mistake:
            | ``#SBATCH --account=<allocation>``
            | or
            | ``#SBATCH -A <allocation>``
            | Example of correct account syntax:
            | ``#SBATCH --account=p12345``
            | or
            | #SBATCH -A p12345

            | Possible mistake: your script doesn’t have an ``#SBATCH``
              line specifying account
            | Fix: confirm that ``#SBATCH --account=<allocation>`` is in
              your script.

            | Possible mistake: a typo in the “–account=” or “-A” part
              of this ``#SBATCH`` line
            | Fix: examine this line closely to make sure the syntax is
              correct

            | Possible mistake: you are not a member of the allocation
              specified in your job submission script
            | Fix: confirm you are a member of the allocation by typing
              ``groups`` at the command line on Quest. If the allocation
              you have specified in your job submission script is not
              listed, you are not a member of this allocation. Use an
              allocation that you are a member of in your job submission
              script.

            | Possible mistake: the mistake is on a line earlier in your
              job submission script which causes Slurm to stop reading
              your script before it reaches the
              ``#SBATCH --account=<allocation>`` line
            | Fix: Move the ``#SBATCH --account=<allocation>`` line to
              be immediately after the line ``#!/bin/bash`` and submit
              your job again. If this generates a new error referencing
              a different line of your script, the account line is
              correct and the mistake is elsewhere in your submission
              script. To resolve the new error, follow the debugging
              suggestions for the new error message.

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: Your allocation has expired or sbatch: error:
            Unable to allocate resources: Invalid qos specification

         .. container::
         panel panel-body js-panelnormalswitches1 collapse

            | Location of mistake:
            | ``#SBATCH --account=<allocation>``
            | or
            | ``#SBATCH -A <allocation>``

            The allocation specified in your job submission script is no
            longer active.

            If you are a member of more than one allocation, you may
            wish to submit your job to an alternate allocation. To see a
            list your allocations, type ``groups`` at the command line
            on Quest.

            To renew your allocation or request a new one, please see
            `Managing an Allocation on
            Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1486>`__.

      .. container:: panel panel-default

         .. container:: panel-heading

            srun: error: –partition option required or srun: error:
            Unable to allocate resources: Access/permission denied

         .. container::
         panel panel-body js-panelnormalswitches2 collapse

            | Location of mistake:
            | ``#SBATCH --partition=<partition/queue>``
            | or
            | ``#SBATCH -p <partition/queue>``

            | Example of correct syntax for general access allocations
              (“p” account):
            | ``#SBATCH --partition=short``
            | or
            | ``#SBATCH -p short``

            | Example of correct syntax for buy-in allocations (“b”
              account):
            | ``#SBATCH --partition=buyin``
            | or
            | ``#SBATCH -p buyin``

            Note that Slurm refers to queues as partitions.

            | Possible mistake: your script doesn’t have an ``#SBATCH``
              line specifying partition
            | Fix: confirm that
              ``#SBATCH --partition=<partition/queue>`` or
              ``#SBATCH -p <partition/queue>`` is in your script.

            | Possible mistake: a typo in the “–partition=” or “-p” part
              of this ``#SBATCH`` line
            | Fix: examine this line closely to make sure the syntax is
              correct

            | Possible mistake: the mistake is on a line earlier in your
              job submission script which causes Slurm to stop reading
              your script before it reaches the
              ``#SBATCH --account=<allocation>`` line
            | Fix: Move the ``#SBATCH --account=<allocation>`` line to
              be immediately after the line ``#!/bin/bash`` and submit
              your job again. If this generates a new error referencing
              a different line of your script, the account line is
              correct and the mistake is elsewhere in your submission
              script. To resolve the new error, follow the debugging
              suggestions for the new error message.

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: Unable to allocate resources: Invalid qos
            specification

         .. container::
         panel panel-body js-panelnormalswitches3 collapse

            | Location of mistake:
            | ``#SBATCH --partition=<partition/queue>``
            | or
            | ``#SBATCH -p <partition/queue>``

            The partition/queue name specified is not associated with
            the allocation in the line
            ``#SBATCH --account=<allocation>``.

            Possible mistake: Your script specifies a buy-in allocation,
            and you’ve specified “short”, “normal” or “long” as your
            partition/queue.

            | Possible mistake: Your script specifies an allocation and
              partition combination which do not belong together.
            | Fix: Specify the correct partition/queue for your
              allocation. To see the allocations and partitions you have
              access to, use this version of the ``sinfo`` command:

            .. code:: code

               sinfo -o "%g %.10R %.20l"
               GROUPS      PARTITION         TIMELIMIT
               b1234       buyin             168:00:00

            Note that “GROUPS” are allocations/accounts on Quest.
            In this example, valid lines in your job submission script
            that relate to account, partition and time would be:

            .. code:: code

               #SBATCH --account=b1234
               #SBATCH --partition=buyin
               #SBATCH --time=168:00:00

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: invalid partition specified: <partition_name>
            or sbatch: error: Unable to allocate resources: Invalid
            partition name specified

         .. container::
         panel panel-body js-panelnormalswitches4 collapse

            | Location of mistake:
            | ``#SBATCH --partition=<partition/queue>``
            | or
            | ``#SBATCH -p <partition/queue>``

            | Example of correct syntax for general access allocations
              (“p” account):
            | ``#SBATCH --partition=short``
            | or
            | ``#SBATCH -p short``

            | Example of correct syntax for buy-in allocations (“b”
              account):
            | ``#SBATCH --partition=buyin``
            | or
            | ``#SBATCH -p buyin``

            | Possible mistake: a typo in the “–partition=” or “-p” part
              of this ``#SBATCH`` line
            | Fix: examine this line closely to make sure the syntax is
              correct

            | Possible mistake: Your script specifies a general access
              allocation (“p” account) with a queue that isn’t “short”,
              “normal” or “long”.
            | Fix: change your partition to be “short”, “normal” or
              “long”

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: Unable to allocate resources: Invalid account
            or account/partition combination specified or sbatch: error:
            Unable to allocate resources: User’s group not permitted to
            use this partition

         .. container::
         panel panel-body js-panelnormalswitches5 collapse

            This message can refer to mistakes on the ``#SBATCH`` lines
            specifying account or partition.

            | Possible location of mistake specifying account:
            | ``#SBATCH --account=<allocation>``
            | or
            | ``#SBATCH -A <allocation>``

            | Possible location of mistake specifying partition
            | ``#SBATCH --partition=<partition/queue>``
            | or
            | ``#SBATCH -p <partition/queue>``

            | Possible mistake: the syntax in the #SBATCH line
              specifying account is incorrect
            | Fix: examine the account line closely to confirm the
              syntax is exactly correct. Example of correct account
              syntax:
            | ``#SBATCH --account=p12345``
            | or
            | ``#SBATCH -A p12345``

            | Possible mistake: you are trying to run in a
              partition/queue that belongs to one account, while
              specifying a different account.
            | Fix: Specify the correct partition/queue for your
              allocation. To see the allocations and partitions you have
              access to, use this version of the ``sinfo`` command:

            .. code:: code

               sinfo -o "%g %.10R %.20l"
               GROUPS      PARTITION         TIMELIMIT
               b1234       buyin             168:00:00

            Note that “GROUPS” are allocations/accounts on Quest.
            In this example, valid lines in your job submission script
            that relate to account, partition and time would be:

            .. code:: code

               #SBATCH --account=b1234
               #SBATCH --partition=buyin
               #SBATCH --time=168:00:00

            | Possible mistake: the mistake is on a line earlier in your
              job submission script which causes Slurm to stop reading
              your script before it reaches the
              ``#SBATCH --account=<allocation>`` line
            | Fix: Move the ``#SBATCH --account=<allocation>`` line to
              be immediately after the line ``#!/bin/bash`` and submit
              your job again. If this generates a new error referencing
              a different line of your script, the account line is
              correct and the mistake is elsewhere in your submission
              script. To resolve the new error, follow the debugging
              suggestions for the new error message.

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: –time limit option required or sbatch: error:
            Unable to allocate resources: Requested time limit is
            invalid (missing or exceeds some limit)

         .. container::
         panel panel-body js-panelnormalswitches6 collapse

            | Location of mistake:
            | ``#SBATCH --time=<hours:minutes:seconds>``
            | or
            | ``#SBATCH -t <hours:minutes:seconds>``

            | Example of correct syntax:
            | ``#SBATCH --time=10:00:00``
            | or
            | ``#SBATCH -t 10:00:00``

            | Possible mistake: your script doesn’t have an ``#SBATCH``
              line specifying time
            | Fix: confirm that ``#SBATCH --time=<hh:mm:ss>`` is in your
              script.

            | Possible mistake: a typo in the “–time=” or “-t” part of
              this ``#SBATCH`` line
            | Fix: examine this line closely to make sure the syntax is
              correct.

            | Possible mistake: the time request is too long for the
              partition (queue)
            | Fix: review the wall time limits of your partition and
              adjust the amount of time requested by your script. For
              general access users with allocations that begin with a
              “p”, please use this reference:

            .. container:: table-responsive

               ========= ==================
               Partition Walltime limit
               ========= ==================
               short     4 hours
               normal    48 hours
               long      7 days / 168 hours
               genhimem  48 hours
               gengpu    48 hours
               ========= ==================

            Buy-in accounts that begin with a “b” have their own wall
            time limits. For information on the wall time of your
            partition, use the ``sinfo`` command:

            .. code:: code

               sinfo -o "%g %.10R %.20l"
               GROUPS      PARTITION         TIMELIMIT
               b1234       buyin             168:00:00

            To fix this error, set your wall time to be less than the
            time limit of your partition and re-submit your job.

            | Possible mistake: the mistake is on a line earlier in your
              job submission script which causes Slurm to stop reading
              your script before it reaches the
              ``#SBATCH --account=<allocation>`` line
            | Fix: Move the ``#SBATCH --time=<hh:mm::ss>`` line to be
              immediately after the line #!/bin/bash and submit your job
              again. If this generates a new error referencing a
              different line of your script, the account line is correct
              and the mistake is elsewhere in your submission script. To
              resolve the new error, follow the debugging suggestions
              for the new error message.

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: unrecognized option <option>

         .. container::
         panel panel-body js-panelnormalswitches7 collapse

            | Example:
            | Line in script: ``#SBATCH --n-tasks-per-node=1``

            ::

               Error generated sbatch: unrecognized option ‘--n-tasks-per-node=1'

            With an “unrecognized option” error, Slurm correctly read
            the first part of the ``#SBATCH`` line but the option that
            follows it has generated the error. In this example, the
            option has a dash between “n” and “tasks” that should not be
            there. The correct option does not have a dash in that
            location. This line should be corrected to:

            ::

               #SBATCH --ntasks-per-node=1

            To fix this error, locate the option specified in the error
            message and examine it carefully for errors. To see correct
            syntax for all ``#SBATCH`` directives, see `Converting
            Moab/Torque scripts to
            Slurm <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1795>`__.

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: CPU count per node can not be satisfied or
            sbatch: error: Batch job submission failed: Requested node
            configuration is not available

         .. container::
         panel panel-body js-panelnormalswitches8 collapse

            | Location of mistake:
            | ``#SBATCH --ntasks-per-node=<CPU count>``
            | Example of mistake:
            | ``#SBATCH --ntasks-per-node=10000``

            This error is generated if your job requests more CPUs/cores
            than are available on the nodes in the partition your job
            submission script specified. CPU count is the number of
            cores requested by your job submission script. Cores are
            also called processors or CPUs.

            To fix this mistake, use the ``sinfo`` command to get the
            maximum number of cores available in the partitions you have
            access to:

            .. code:: code

               sinfo -o "%g %.10R %.20l %.10c"
               GROUPS      PARTITION       TIMELIMIT       CPUS
               b1234       buyin           2-00:00:00      20+

            In this example, your job submission script can request up
            to 20 CPUs/cores per node like this:

            ::

               #SBATCH --ntasks-per-node=20

      .. container:: panel panel-default

         .. container:: panel-heading

            sbatch: error: Batch script contains DOS line breaks (\r\n)
            or sbatch: error: instead of expected UNIX line breaks (\n).

         .. container::
         panel panel-body js-panelnormalswitches9 collapse

            Location of mistake:

            Hidden characters in your job submission script

            Mistake: your job submission script was created on a Windows
            machine and copied onto Quest without converting it into
            UNIX encoded characters.

            Fix: from the command line on Quest run the command
            ``dos2unix <submission_script``> to correct your job
            submission script and re-submit your job to the scheduler.

      .. rubric:: Debugging a Job Accepted by the Scheduler
         :name: h_94310085932911650907024000

      Once your job has been accepted, the Slurm scheduler will return a
      job id number. After waiting in the queue, your job will run. To
      see the status of your job, use the command ``sacct -X``.

      For jobs with mistakes that do not give error messages, you will
      need to investigate if you notice something is wrong with how the
      job runs. If you notice a problem on the list below, click on it
      for debugging suggestions.

      .. container:: panel panel-default

         .. container:: panel-heading

            Job runs very slowly or dies after starting

         .. container::
         panel panel-body js-panelnormalswitches10 collapse

            | Problem: job runs very slowly, or dies after starting
            | Possible cause: job script is not reading the directive
              ``#SBATCH --mem=<amount>``.

            All Slurm job scripts should specify the amount of memory
            your job needs to run. If your job runs very slowly or dies,
            investigate if it requests enough memory with the Slurm
            utility ``seff``. For more information, see `Checking
            Processor and Memory Utilization for Jobs on
            Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1695>`__.

      .. container:: panel panel-default

         .. container:: panel-heading

            Job name is name of job submission script instead of name in
            submission script

         .. container::
         panel panel-body js-panelnormalswitches11 collapse

            | Problem: job name is name of job submission script instead
              of name in submission script
            | Possible cause: job script is not reading the
              ``#SBATCH --job-name=<job name>`` directive.

            | Slurm is not reading the ``#SBATCH`` directive:
            | ``#SBATCH -J <Job_Name>``
            | or
            | ``#SBATCH --job-name=<Job_Name>``

            To see the name of your job, run ``sacct -X``. If JOB NAME
            is the first eight characters of the name of your submission
            script, SLURM has not read the ``#SBATCH`` lines for job
            name.

            | Possible Mistake: a typo in the “–job-name=” or “-J” part
              of this ``#SBATCH`` line
            | Fix: examine this line closely to make sure the syntax is
              correct

            | Possible mistake: the mistake is on a line earlier in your
              job submission script which causes Slurm to stop reading
              your script before it reaches the
              ``#SBATCH --job-name=<job name>`` line
            | Fix: Move the ``#SBATCH --job-name=<job name>`` line to be
              immediately after the line ``#!/bin/bash`` and submit your
              job again. If this generates a new error referencing a
              different line of your script, the account line is correct
              and the mistake is elsewhere in your submission script. To
              resolve the new error, follow the debugging suggestions
              for the new error message.

      .. container:: panel panel-default

         .. container:: panel-heading

            Modules or environment variables are inherited from the
            login session by a running job

         .. container::
         panel panel-body js-panelnormalswitches12 collapse

            | Problem: modules or environmental variables are inherited
              from the login session by a running job
            | Possible cause: job script is not purging modules before
              beginning compute node session

            Fix: after the ``#SBATCH`` directives in your job submission
            script, add the line

            .. code:: code

               module purge all

            This will clear any modules inherited from your login
            session, and begin your job in a clean environment. You will
            need to load any necessary modules into your job submission
            script after this line.

            .. rubric:: Job immediately fails and generates no output or
               error file
               :name: job-immediately-fails-and-generates-no-output-or-error-file

            | Problem: job can’t write into output and/or error files so
              job immediately dies
            | Possible cause: job script specifies directory path for
              output and/or error files but does not provide a file name
            | Possible cause: job script specifies a directory that does
              not exist

            Slurm is not getting a file name that it can write into in
            the SBATCH directive:

            .. code:: code

               #SBATCH –-output=/path/to/file/file_name

            or

            .. code:: code

               #SBATCH --error=/path/to/file/file_name

            | Possible Mistake: a typo in the “–output=” or “–error”
              part of this #SBATCH line
            | Fix: examine this line closely to make sure the syntax is
              correct

            | Possible Mistake: providing a directory but not a file
              name for output and/or error files
            | Fix: add a file name at the end of the specified path. For
              a file name in the format ``<job_name>.o<job_id>``, use

            .. code:: code

               #SBATCH –-output=/path/to/file/"%x.o%j"

            Note if a separate error file is not specified, errors and
            output will both be written into the output file. To
            generate a separate error file, include the line:

            .. code:: code

               #SBATCH –-error=/path/to/file/"%x.e%j"

      .. rubric:: Common reasons for Failed Jobs
         :name: section-job-failures

      This section provides some common reasons for why your job may
      fail and how to go about fixing it.
      .. rubric:: Job Exceeded Request Time or Memory
         :name: #section-ran-out-of-disk

      Besides errors in your script or hardware failure, your job may be
      aborted by the system if it is still running when the walltime
      limit you requested (or the upper walltime limit for the
      partition) is reached. You will see ``TIMEOUT`` state for these
      jobs.

      If you use more cores than you requested, the system will again
      stop the job. This can happen with programs that are
      multi-threaded. Similarly, if the job exceeds the requested
      memory, the job will be terminated. Due to this, it is important
      to profile your code for the memory requirement.

      If you do not set the number of nodes/cores, memory or time in
      your job submission script, the default values will be assigned by
      the scheduler.

      .. rubric:: Out of Disk Space
         :name: #section-ran-out-of-disk

      Your job could also fail if you exceed your storage quote in your
      home or projects directory.

      Check how much space you are using in your home directory with

      .. code:: code

         homedu

      or

      .. code:: code

         du -h --max-depth=0 ~

      Check how much space is used in your projects directory with

      .. code:: code

         checkproject <allocationID>
