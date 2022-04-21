Slurm Configuration Settings
----------------------------

In this section, we go into details about a number of the Slurm
configurations settings. In each subsection, we include the possible
range of values a user can set, how to think about what value to use for
a given setting, and whether or not the setting is required and if not
required, what the default is for the setting.

.. _section-account:

Allocation/Account
~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   To specify the account, please include a ``-A/--account`` option in
   your job submission script:

   ``#SBATCH -A <allocation>``

   To submit jobs to Quest, you must be part of an **active** classroom,
   research, or buy-in allocation. Additionally, for buy-in allocations
   you must be part of a **buy-in allocation with access to compute
   resources**. To determine the names of the allocation(s)/account(s)
   that you are part of on Quest, you can run the following on the
   command line which will produce output similar to below.

   ::

      $ groups
      quest_demo b1011 p30157 e31572

   Once you determine the allocation(s)/account(s) that you are part of,
   you can check whether or not the allocation is active and has access
   to compute resources by running ``checkproject`` and reading the last
   two lines of the output. An example of running ``checkproject`` is
   below.

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

   For more information on allocations and allocation management, please
   see `Managing an Allocation on
   Quest <https://kb.northwestern.edu/65175>`__.

.. _section-partitions:

Quest Partitions/Queues
~~~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   To specify the partition, please include a ``-p/--partition`` option
   in your job submission script:

   ``#SBATCH -p <partition>``

   Quest offers several partitions or queues where you can run your job.
   Based on the duration of your job, number of cores, and type of
   access to Quest, you should select the most appropriate partition for
   your job. A partition must be specified when you submit your job or
   the scheduler will return the error,
   ``"sbatch: error: Batch job submission failed: No partition specified or system default partition"``.

   .. rubric:: Partition Definitions: General Access ("p" and "e"
      accounts)
      :name: partition-definitions-general-access-p-and-e-accounts

   Standard compute node access.

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

   +-----------+------------------+-------------------------------------+
   | Partition | Maximum Walltime | Notes                               |
   +===========+==================+=====================================+
   | gengpu    | 48:00:00         | This partition can be used only if  |
   |           |                  | your job requires GPUs. In addition |
   |           |                  | to specifying *gengpu* as your      |
   |           |                  | partition, you must additionally    |
   |           |                  | specify in your submission script   |
   |           |                  | ``#SBATCH --gres=gpu:a100:X``,      |
   |           |                  | where ``X`` is the number of GPUs   |
   |           |                  | you want to request. Please see     |
   |           |                  | `GPUs on                            |
   |           |                  | QUEST <htt                          |
   |           |                  | ps://kb.northwestern.edu/108515>`__ |
   |           |                  | for more information about the      |
   |           |                  | GPUs.                               |
   +-----------+------------------+-------------------------------------+
   | genhimem  | 48:00:00         | This partition can be used only if  |
   |           |                  | your job requires more than 180 GB  |
   |           |                  | memory per node. This partition has |
   |           |                  | access to 4 nodes, three 28 core    |
   |           |                  | nodes with 500GB of schedulable     |
   |           |                  | memory and one 52 core node with    |
   |           |                  | 1TB of schedulable memory.          |
   +-----------+------------------+-------------------------------------+

   .. rubric:: Partition Definitions: Full Access (buy-ins, or "b"
      accounts)
      :name: partition-definitions-full-access-buy-ins-or-b-accounts

   +-----------------------+-----------------------+-----------------------+
   | Partition             | Maximum Walltime      | Notes                 |
   +=======================+=======================+=======================+
   | Allocation name (e.g. | Allocation-specific   | Using the allocation  |
   | "b1234")              |                       | name as the partition |
   | or                    |                       | name is only          |
   | "buyin"               |                       | available to users    |
   |                       |                       | with `full            |
   |                       |                       | acce                  |
   |                       |                       | ss <http://www.it.nor |
   |                       |                       | thwestern.edu/researc |
   |                       |                       | h/user-services/quest |
   |                       |                       | /full-access.html>`__ |
   |                       |                       | to Quest. The         |
   |                       |                       | resources available   |
   |                       |                       | and any limits on     |
   |                       |                       | jobs are governed by  |
   |                       |                       | the specific policies |
   |                       |                       | of the full-access    |
   |                       |                       | allocation.           |
   |                       |                       | Example:              |
   |                       |                       | ``#SLURM -p b1234``   |
   |                       |                       | When using the buyin  |
   |                       |                       | partition, you must   |
   |                       |                       | also specify the      |
   |                       |                       | appropriate buyin     |
   |                       |                       | allocation ID in your |
   |                       |                       | job submission        |
   |                       |                       | script, using the     |
   |                       |                       | ``-A`` flag. Using    |
   |                       |                       | the buyin partition   |
   |                       |                       | is the same as using  |
   |                       |                       | your allocation name  |
   |                       |                       | as the partition      |
   |                       |                       | name.                 |
   |                       |                       | Example:              |
   |                       |                       | ``#SLURM -p buyin``   |
   |                       |                       | If your allocation    |
   |                       |                       | has specific          |
   |                       |                       | partition names, such |
   |                       |                       | as genomics,          |
   |                       |                       | ciera-std, grail-std  |
   |                       |                       | etc., you should use  |
   |                       |                       | those partition names |
   |                       |                       | instead of your       |
   |                       |                       | allocation name or    |
   |                       |                       | buyin partition.      |
   +-----------------------+-----------------------+-----------------------+

   .. rubric:: Notes
      :name: notes

   Additional specialized partitions exist for specific allocations. You
   may be instructed to use a partition name that isn't listed above.

   If you need to run jobs longer than one week, `contact Research
   Computing <mailto:quest-help@northwestern.edu?subject=Quest%20Long-running%20job>`__
   for a consultation. Some special accommodations can be made for jobs
   requiring the resources of up to a single node for a month or less.

.. _section-walltime:

Walltime/Length of the job
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   To specify the walltime for your job, please include a
   ``-t/--walltime`` option in your job submission script:

   ``#SBATCH -t <timelimit>``

   There are two important considerations when selecting the walltime,
   the `partition <#section-partitions>`__ that you chose and how long
   your job is expected to run. Although the partition that you choose
   will control the maximum wall time that can be selected, we recommend
   not to simply select the maximum allowable wall time for that
   partition unless it is truly needed. The scheduler will take your
   walltime request at face value, and so this can lead to your job
   taking longer than necessary to be scheduled. An incorrect walltime
   specification does not hurt you when Slurm assesses your utilization
   of the cluster as *only the*\ **actual**\ *duration of your job is
   used in computing your utilization.* On the flip side of being too
   conservative with your walltime selection, if you are too aggressive
   and your job will need more time to complete then requested, there is
   no way to extend the walltime of a running job on Quest. This is why
   we recommend submitting a single, representative job and seeing how
   long it takes to run before selecting a walltime and submitting a
   large number of jobs.

.. _section-number-of-nodes:

Number of Nodes
~~~~~~~~~~~~~~~

.. container:: panel-content

   To specify the number of nodes, please include the ``-N/--nodes``
   option in your job submission script:

   ``#SBATCH --nodes=<number_of_nodes>``

   Although the number of nodes is an optional setting, we strongly
   recommend always setting this value. Specifically, we recommend
   setting this value to

   ``#SBATCH --nodes=1``

   as the vast majority of software can only run on a single computer
   and cannot run across multiple computers. When you forget to set this
   value, but you do set the `Number of Cores <#number-of-cores>`__,
   this can cause Slurm to match you with a set of computing resources
   which your application will be unable to use, but will still be
   penalized in your fair share for using. If you know that your
   application uses Message Passing Interface (MPI) to parallelize, then
   setting this value to something ``>1`` could make sense.

.. _section-number-of-cores:

Number of Cores
~~~~~~~~~~~~~~~

.. container:: panel-content

   There are two methods for specifying the number of cores, the
   ``-n/--ntasks`` option which indicates how many total cores you would
   like:

   ``#SBATCH --ntasks=<number_of_cores>``

   or the ``--ntasks-per-node=n`` option which indicates how many cores
   you would like *per node* and should always be used with the `Number
   of Nodes <#number-of-nodes>`__ option:

   .. code:: code

      #SBATCH --nodes=<number_of_nodes>
      #SBATCH --ntasks-per-node=<number_of_cores_per_node>

   Although the number of cores is an optional setting whose default is
   1, we strongly recommend always setting this value. Specifically, we
   recommend setting this value (to start) to

   ``#SBATCH --ntasks=1``

   The only situation in which ``-n/--ntasks`` should be greater than 1
   is if the application you are using has the capability to be
   parallelized. Many applications do **not** have this capability and
   therefore it is best to start of setting this value to 1. If your
   application is capable of parallelization, you will next want to
   determine what type of parallelization it uses in order to set this
   value correctly. For instance, if your application utilizes shared
   memory parallelization (OpenMP, R's doParallel, Python's
   multiprocessing, MATLAB local parpool, etc) then you can consider
   setting this value to be greater than 1. However, shared memory
   parallelization can only utilize CPUs *within a single computer* and
   CPUs allocated *across* computers will go unused. Therefore, if your
   code is parallelized in this manner, you must also specify
   ``#SBATCH --nodes=1``
   Finally, if you know that your application uses Message Passing
   Interface (MPI) to parallelize, then it can utilize CPUs
   allocated *across* computers and therefore setting ``-n/--ntasks``
   without also setting ``-N/--nodes`` would make sense.

   A final consideration when selecting how many CPUs you want is how
   many CPUs are available on each of the different generations/families
   of compute nodes that make up Quest. Below is a table which
   summarizes the relevant information.

   +----------------+----------------+----------------+----------------+
   | Node Family    | Number of CPUs | Amount of      | Partitions     |
   | Name           |                | *              | with these     |
   |                |                | *Schedulable** | Nodes          |
   |                |                | Memory/RAM     |                |
   +----------------+----------------+----------------+----------------+
   | quest7         | 28             | 116GB          | short/norm     |
   |                |                |                | al/long/buying |
   +----------------+----------------+----------------+----------------+
   | quest8         | 28             | 84GB           | short/norm     |
   | (general       |                |                | al/long/buying |
   | access)        |                |                |                |
   +----------------+----------------+----------------+----------------+
   | quest8 (buyin) | 28             | 180GB          | buyin/short    |
   +----------------+----------------+----------------+----------------+
   | quest9         | 40             | 180GB          | buyin/short    |
   +----------------+----------------+----------------+----------------+
   | quest10        | 52             | 180GB          | short/norm     |
   |                |                |                | al/long/buying |
   +----------------+----------------+----------------+----------------+

   To drive home this point, imagine you made the following request:

   ::

      #SBATCH --nodes=1
      #SBATCH --ntasks-per-node=30
      #SBATCH --partition=short

   This request would eliminate the Slurm's ability to match you with
   any of the computers from generation quest7/8/9 and would increase
   the amount of time it will take to schedule your job as only one type
   of compute node is able to match your request.\ ````

.. _section-required-memory:

Required memory
~~~~~~~~~~~~~~~

.. container:: panel-content

   There are two methods for specifying how much memory/RAM you need,
   the ``--mem`` option which indicates how much memory you want *per
   node*.

   ``#SBATCH --mem=<memory per node>G``

   or the ``--mem-per-cpu`` option which indicates how much memory/RAM
   you need *per CPU.*

   ``#SBATCH --mem-per-cpu=<memory per cpu>G``

   If your job submission script does not specify how much memory your
   job requires, then the default setting is 3.25 GB of memory per core.

   ``#SBATCH --mem-per-cpu=3256M``

   Therefore, you submitted a job to run on 10 cores and did not specify
   your memory request in your job submission script, Slurm will
   allocate 32.5 GB in total.

   The memory that is allocated to your job via this setting creates a
   **hard upper limit** and your application cannot access memory beyond
   what Slurm reserves for them; if your job tries to access more memory
   than has been reserved, it will be terminated.

   There is a special setting to request the entire memory of the
   computer.

   ``#SBATCH --mem=0``

   How much memory this ends up being will depend on what
   generation/family of computer Slurm matches you to. The following is
   a table which summarizes the relevant information.

   +----------------+----------------+----------------+----------------+
   | Node Family    | Number of CPUs | Amount of      | Partitions     |
   | Name           |                | *              | with these     |
   |                |                | *Schedulable** | Nodes          |
   |                |                | Memory/RAM     |                |
   +----------------+----------------+----------------+----------------+
   | quest7         | 28             | 116GB          | short/norm     |
   |                |                |                | al/long/buying |
   +----------------+----------------+----------------+----------------+
   | quest8         | 28             | 84GB           | short/norm     |
   | (general       |                |                | al/long/buying |
   | access)        |                |                |                |
   +----------------+----------------+----------------+----------------+
   | quest8 (buyin) | 28             | 180GB          | buyin/short    |
   +----------------+----------------+----------------+----------------+
   | quest9         | 40             | 180GB          | buyin/short    |
   +----------------+----------------+----------------+----------------+
   | quest10        | 52             | 180GB          | short/norm     |
   |                |                |                | al/long/buying |
   +----------------+----------------+----------------+----------------+

   A final consideration when selecting how much memory/RAM you want
   is how much memory/RAM is available on each of the different
   generations/families of compute nodes that make up Quest. To drive
   home this point, imagine you made the following request:

   ::

      #SBATCH --nodes=1
      #SBATCH --mem=130G
      #SBATCH --partition=short

   This request would eliminate the Slurm's ability to match you with
   any of the computers from generation quest7/8 and would increase the
   amount of time it will take to schedule your job as you will have
   reduced the pool of available compute nodes.

   **How can I tell if my job needs more memory to run successfully?**

   Use the ``sacct -X`` command to see information about your recent
   jobs, for example:

   .. code:: code

      $ sacct -X
                 JobID    JobName  Partition    Account  AllocCPUS      State ExitCode 
          ------------ ---------- ---------- ---------- ---------- ---------- -------- 
          1273539      lammps-te+      short     p1234          40  COMPLETED      0:0 
          1273543      vasp-open+      short     p1234          40 OUT_OF_ME+    0:125

   | 
   | The "State" field is the status of your job when it finished. Jobs
     with a "COMPLETED" state have run without system errors. Jobs with
     an "OUT_OF_ME+" state have run out of memory and failed.
     "OUT_OF_ME+" jobs need to request more memory in their job
     submission scripts to complete successfully.
   | If the job you're investigating is not recent enough to be listed
     by ``sacct -X``, add date fields to the command to see jobs between
     specific start and end dates. For example, to see all jobs between
     September 15, 2019 and September 16, 2019:

   .. code:: code

      $ sacct -X --starttime=091519 --endtime=091619

   | 
   | Specify the date using MMDDYY. More information on sacct is
     available `here <https://slurm.schedmd.com/sacct.html>`__.

   **My job ran out of memory and failed, now what?**

   First, determine how much memory your job needs following the steps
   outlined below. Once you know how much memory your job needs, edit
   your job submission script to reserve that amount of memory + 10% for
   your job.

   **How much memory does my job need?**

   To determine out how much memory your job uses on a compute node:

   #. create a test job by editing your job's submission script to
      reserve all of the memory of the node it runs on
   #. run your test job
   #. confirm your test job has completed successfully
   #. use ``seff`` to see how much memory your job actually used.

   *Create a test job*

   To profile your job's memory usage, create a test job by modifying
   your job's submission script to include the lines:

   .. code:: code

      #SBATCH --mem=0
      #SBATCH --nodes=1

   | Setting ``--mem=0`` reserves all of the memory on the node for your
     job; if you already have a ``--mem=`` directive in your job
     submission script, comment it out. Now your job will not run out of
     memory unless your job needs more memory than is on the node.
   | Setting ``--nodes=1`` reserves a single node for your job. For jobs
     that run on multiple nodes such as MPI-based programs, request the
     number of nodes that your job runs on. Be sure to specify a value
     for ``#SBATCH --nodes=`` or the cores your job submission script
     reserves could end up on as many nodes as cores requested. Be aware
     that by setting ``--mem=0``, you will be reserving all the memory
     on each of those nodes that your cores are reserved on.

   | 2) *Run your test job
     *
   | Submit your test job to the cluster with the ``sbatch`` command.
     For interactive jobs, use ``srun`` or ``salloc``.
   | 3) *Did your test job complete successfully?*
   | When your job has stopped running use the sacct -X command to
     confirm your job finished with state "COMPLETED". If your test job
     finishes with an "OUT_OF_ME+" state, confirm that you are
     submitting the modified job submission script that requests all of
     the memory on the node. If the "OUT_OF_ME+" errors persist, your
     job may require more memory than is available on the compute node
     it ran on. In this case, please email quest-help@northwestern.edu
     for assistance.
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
          

   Check the job State reported in the 4th line. If it is "COMPLETED
   (exit code 0)", look at the last two lines. "Memory Utilized" is the
   amount of memory your job used, in this case 60Gb.

   | If the job State is FAILED or CANCELLED, the Memory Efficiency
     percentage reported by seff will be extremely inaccurate. The seff
     command only works on jobs that have COMPLETED successfully.
   | **How much memory should I reserve in my job script?**

   It's a good idea to reserve slightly more memory than your job
   utilized since the same job may require slightly different amounts of
   memory depending on variations in data it processes in each run of
   the job. To correctly reserve memory for this job, edit your test job
   submission script to modify the ``#SBATCH --mem=`` directive to
   reserve 10% more than 60Gb in the job submission script:

   .. code:: code

      #SBATCH --mem=66G

   For jobs that use MPI, remove the ``#SBATCH --mem=`` directive from
   your job submission script. Now specify the amount of memory you'd
   like to reserve per core instead. For example, if your job uses 100Gb
   of memory total and runs on 10 cores, reserve 10Gb plus a safety
   factor per cpu:

   .. code:: code

      #SBATCH --mem-per-cpu=11G

   If it doesn't matter how many nodes your cores are distributed on you
   may remove the ``#SBATCH --nodes=`` directive as well.

   Be careful not to reserve significant amounts of memory beyond what
   your job requires as your job's wait time will increase and reserving
   excessive memory also wastes shared resources that could be used by
   other researchers.

.. _section-output-error:

Standard Output/Error
~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   To specify a file into which *both* the standard output *and*
   standard error from your job will be written, please include *only*
   the ``-o/--output`` option in your job submission script:

   ``#SBATCH -output=<name of file>.out``

   This will cause a file to be created in the submission directory with
   this name. You may also specify filename which includes the absolute
   or full path to the file **but you cannot just include a path to a
   directory**. Please make sure that all directories in the file path
   name exist on Quest.

   To separate out the standard output and standard error into two
   separate files, please include *both* the ``-o/--output`` option
   *and* the ``-e/--error`` option in your job submission script:

   .. code:: code

      #SBATCH -output=<name of file>.out
      #SBATCH -error=<name of file>.err

   If you do not include either option, the default setting with be to
   write both the standard output and standard error from your job in a
   file called
   ``slurm-<slurm jobid>.out``

   where ``<slurm job id>`` is the ID given to your job by SLURM. You
   can replicate this default naming scheme yourself by providing the
   following option:

   ``#SBATCH --output=slurm-%j.out``

   In addition to ``%j`` which will add the job id to the name of your
   output file, there is also ``%x`` will add the job name to the name
   of your output file.

.. _section-job-name:

Job Name
~~~~~~~~

.. container:: panel-content

   To specify a name for your job, please include a ``-J/--job-name``
   option in your job submission script:

   ``#SBATCH --job-name=<job name>``

.. _section-email:

Sending e-mail alerts about your job
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   To receive e-mails regarding the status of your Slurm jobs, please
   include *both* the ``--mail-type`` option *and* the ``--mail-user``
   option in your job submission script:

   .. code:: code

      #SBATCH --mail-type=<job state that triggers email> ## BEGIN, END, FAIL or ALL
      #SBATCH --mail-user=<email address>

   If you do not include both of these options, then you will not
   receive emails from Slurm. Also, you can include any combination of
   BEGIN, END, FAIL as an argument for this option.

.. _section-constraints:

Constraints
~~~~~~~~~~~

.. container:: panel-content

   To specify an architecture constraint for your job, please include a
   ``-C/--constraint`` option in your job submission script:

   ``#SBATCH --constraint=<name of compute node architecture>``

   Not all Quest compute nodes are the same. We currently have four
   different generations or architectures of compute nodes which we
   refer to as quest7, quest8, quest9 and quest10 and detailed
   information on each of these architectures can be found
   `here <https://www.it.northwestern.edu/research/user-services/quest/specs.html>`__.
   If you need to restrict your job to a particular architecture, you
   can do so through the constraint directive. For example,
   ``--constraint=quest10`` will cause the scheduler to only match you
   to compute nodes of the quest10 generation.
   Moreover, if you would like to match to any generation of compute
   nodes, but would like all the compute nodes to be either of
   generation quest7 or quest8 or quest9 or quest10 and not a
   combination of generations, then you can set the following for
   constraint.
   ``#SBATCH --constraint="[quest7|quest8|quest9|quest10]"``
   This can be a helpful setting for jobs that are parallelized using
   MPI.

.. _section-all-slurm-options:

All Slurm Configuration Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   +-----------------------------------+-----------------------------------+
   | Option                            | Slurm (sbatch)                    |
   +===================================+===================================+
   | Job name                          | --job-name=<name>                 |
   |                                   | -J <name>                         |
   +-----------------------------------+-----------------------------------+
   | Account                           | --account=<account>               |
   |                                   | -A <account>                      |
   +-----------------------------------+-----------------------------------+
   | Queue                             | --partition=<queue>               |
   +-----------------------------------+-----------------------------------+
   | Wall time limit                   | --time=<hh:mm:ss>                 |
   |                                   | -t<hh:mm:ss>                      |
   +-----------------------------------+-----------------------------------+
   | Node count                        | --nodes=<count>                   |
   |                                   | -N <count>                        |
   +-----------------------------------+-----------------------------------+
   | Core count                        | -n <count>                        |
   +-----------------------------------+-----------------------------------+
   | Process count per node            | --ntasks-per-node=<count>         |
   +-----------------------------------+-----------------------------------+
   | Core count (per process)          | --cpus-per-task=<cores>           |
   +-----------------------------------+-----------------------------------+
   | Memory limit                      | --mem=<limit> (Memory per node in |
   |                                   | MB)                               |
   +-----------------------------------+-----------------------------------+
   | Minimum memory per processor      | --mem-per-cpu=<memory>            |
   +-----------------------------------+-----------------------------------+
   | Request GPUs                      | --gres=gpu:<count>                |
   +-----------------------------------+-----------------------------------+
   | Instead of specifying how many    | -w,                               |
   | nodes you want,                   | --nodelist=<node>[,node2[,...]]>  |
   | you could request a specific set  | -F, --nodefile=<node file>        |
   | of compute nodes.                 |                                   |
   | This cannot be used in            |                                   |
   | combination with ``--nodes=``.    |                                   |
   +-----------------------------------+-----------------------------------+
   | Job array                         | -a <array indices>                |
   +-----------------------------------+-----------------------------------+
   | Standard output file              | --output=<file path> (path must   |
   |                                   | exist)                            |
   +-----------------------------------+-----------------------------------+
   | Standard error file               | --error=<file path> (path must    |
   |                                   | exist)                            |
   +-----------------------------------+-----------------------------------+
   | Combine stdout/stderr to stdout   | --output=<combined out and err    |
   |                                   | file path>                        |
   +-----------------------------------+-----------------------------------+
   | Architecture constraint           | --constraint=<architecture>       |
   |                                   | -C <architecture>                 |
   +-----------------------------------+-----------------------------------+
   | Copy environment                  | --export=ALL (default)            |
   |                                   | --export=NONE ## to not export    |
   |                                   | environment                       |
   +-----------------------------------+-----------------------------------+
   | Copy environment variable         | --export=<variable[               |
   |                                   | =value][,variable2=value2[,...]]> |
   +-----------------------------------+-----------------------------------+
   | Job dependency                    | --                                |
   |                                   | dependency=after:jobID[:jobID...] |
   |                                   | --de                              |
   |                                   | pendency=afterok:jobID[:jobID...] |
   |                                   | --depen                           |
   |                                   | dency=afternotok:jobID[:jobID...] |
   |                                   | --dep                             |
   |                                   | endency=afterany:jobID[:jobID...] |
   +-----------------------------------+-----------------------------------+
   | Request event notification        | --mail-type=<events>              |
   |                                   | Note: multiple mail-type requests |
   |                                   | may be specified in a comma       |
   |                                   | separated list:                   |
   |                                   | --mail-type=BEGIN,END,FAIL        |
   +-----------------------------------+-----------------------------------+
   | Email address                     | --mail-user=<email address>       |
   +-----------------------------------+-----------------------------------+
   | Defer job until the specified     | --begin=<date/time>               |
   | time                              |                                   |
   +-----------------------------------+-----------------------------------+
   | Node exclusive job                | --exclusive                       |
   +-----------------------------------+-----------------------------------+

.. _section-slurm-environmental-variables:

Environmental Variables Set by Slurm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

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
