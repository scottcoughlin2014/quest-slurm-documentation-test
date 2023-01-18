Managing Full Access Allocations on Quest
=========================================

How to obtain information on full access/buy-in allocation resources on
Quest.

Users with `full
access <https://it.northwestern.edu/secure/purchasing-resources.html>`__,
also referred to as buy-in allocations, can view information about their
nodes, queues, and allocation members.

Full access allocation IDs usually start with the letter b; e.g. b1002.
Use your allocation ID in place of ``<allocationID>`` in all commands
below, without the surrounding ``<>``.

List Allocation Members
~~~~~~~~~~~~~~~~~~~~~~~

Display all current members of the allocation:

.. code:: code

   module load utilities
   grouplist <allocationID>

Status of Allocation Jobs in Queue
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Display the the status of all jobs submitted to an allocation

.. code:: code

   squeue –A <allocationID>

Example Output
^^^^^^^^^^^^^^

.. code:: code

   JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
   79872 b1002     hisat2-b   ghi678  PD      0:00      1 (None)
   79872 b1002         bash   def345  R      12:42      1 qgpu8014
   79851 b1002     rv2rg8z0   abc123  R   10:22:46      2 qnode[8010-8011]
   79846 b1002         vasp   abc123  R   11:16:41      5 qnode[6062-6066]

Status of Allocation Nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``sinfo`` command will give you information about state of a
particular account’s compute nodes. For full access allocations which
are using “buyin” partition (i.e. queue), nodes associated with the
allocation can be listed as below:

.. code:: code

   sinfo -N -p <allocationID>

For full access allocations which have multiple partitions (i.e. queues)
such as b1042, b0194 or b1095, nodes associated with the specific queue
can be listed as below:

.. code:: code

   sinfo -N -p <partitionID>

Individual Node Status
~~~~~~~~~~~~~~~~~~~~~~

After you identified your allocation’s and/or partition’s nodes using
``sinfo`` you can use the following command to get detailed information
about a specific node:

.. code:: code

   scontrol show node <nodeID>

**Example**

.. code:: code

   scontrol show node qnode5004

If the above commands don’t work for your allocation, contact
quest-help@northwestern.edu for assistance.

Why are my jobs running on nodes other than those reserved for my allocation?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have access to more than one allocation or queue (i.e. partition)
on Quest, you may have defined a different allocation or queue in your
job submission script. Please make sure that the intended allocation or
queue have been setup correctly in your job submission script.
