Quest Partitions/Queues
=======================

Definitions of Quest’s partitions/queues.

Quest offers several partitions or queues where you can run your job.
Based on the duration of your job, number of cores, and type of access
to Quest, you should select the most appropriate partition for your job.
A partition must be specified when you submit your job or the scheduler
will return the error, “sbatch: error: Batch job submission failed: No
partition specified or system default partition”. Users with `full
access <https://it.northwestern.edu/secure/purchasing-resources.html>`__
to Quest or the `Genomics Compute
Cluster <http://www.it.northwestern.edu/research/user-services/quest/genomics.html>`__
must specify the appropriate partition for those resources. To specify
the partition, please include a ``-p`` option in your job submission
script:

#SBATCH -p <PartitionName>

Partition Definitions: General Access (“p” and “e” accounts)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: table-responsive

   +-----------+------------------+-------------------------------------+
   | Partition | Maximum Walltime | Notes                               |
   +===========+==================+=====================================+
   | short     | 04:00:00         | Short jobs have access to more      |
   |           |                  | cores on Quest than longer jobs do  |
   |           |                  | and are usually scheduled sooner.   |
   +-----------+------------------+-------------------------------------+
   | normal    | 48:00:00         | Normal jobs may run up to 2 days.   |
   +-----------+------------------+-------------------------------------+
   | long      | 168:00:00        | Long jobs may run for up to 7 days. |
   +-----------+------------------+-------------------------------------+
   | gengpu    | 48:00:00         | This partition can be used only if  |
   |           |                  | your job requires GPUs.             |
   +-----------+------------------+-------------------------------------+
   | genhimem  | 48:00:00         | This partition can be used only if  |
   |           |                  | your job requires more than 180 GB  |
   |           |                  | memory per node.                    |
   +-----------+------------------+-------------------------------------+

Partition Definitions: Full Access (buy-ins, or “b” accounts)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: table-responsive

   +-----------------------+-----------------------+-----------------------+
   | Partition             | Maximum Walltime      | Notes                 |
   +=======================+=======================+=======================+
   | Allocation name (e.g. | Allocation-specific   | Using the allocation  |
   | “b1234”)              |                       | name as the partition |
   | or                    |                       | name is only          |
   | “buyin”               |                       | available to users    |
   |                       |                       | with `full            |
   |                       |                       | access <h             |
   |                       |                       | ttps://it.northwester |
   |                       |                       | n.edu/secure/purchasi |
   |                       |                       | ng-resources.html>`__ |
   |                       |                       | to Quest. The         |
   |                       |                       | resources available   |
   |                       |                       | and any limits on     |
   |                       |                       | jobs are governed by  |
   |                       |                       | the specific policies |
   |                       |                       | of the full-access    |
   |                       |                       | allocation.           |
   |                       |                       | Example:              |
   |                       |                       | ``#SLURM -p b1234``   |
   |                       |                       | When using the        |
   |                       |                       | ``buyin`` partition,  |
   |                       |                       | you must also specify |
   |                       |                       | the appropriate buyin |
   |                       |                       | allocation ID in your |
   |                       |                       | job submission        |
   |                       |                       | script, using the -A  |
   |                       |                       | flag. Using the buyin |
   |                       |                       | partition is the same |
   |                       |                       | as using your         |
   |                       |                       | allocation name as    |
   |                       |                       | the partition name.   |
   |                       |                       | Example:              |
   |                       |                       | ``#SLURM -p buyin``   |
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
   |                       |                       | ``buyin`` partition.  |
   +-----------------------+-----------------------+-----------------------+

Notes
~~~~~

Additional specialized partitions exist for specific allocations. You
may be instructed to use a partition name that isn’t listed above.

Note that jobs that have not finished on their own by the end of the
requested time are terminated by the scheduler.

When resources for a job are not immediately available, the job will be
assigned a pending (PD) status while the scheduler waits for resources
to become available. There is a hard limit of 5,000 total submitted jobs
per user at one time.

General access allocations have access to `compute nodes with
GPUs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1112>`__
under the ``gengpu`` partition. To request one A100 GPU, you should set
``gengpu`` as the partition and add the following line to your job
submission script: ``#SBATCH --gres=gpu:a100:1``. Please see `GPUs on
QUEST <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1112>`__
for more information about the GPUs.

If you need to run jobs longer than one week, `contact Research
Computing <mailto:quest-help@northwestern.edu?subject=Quest%20Long-running%20job>`__
for a consultation. Some special accommodations can be made for jobs
requiring the resources of up to a single node for a month or less.
