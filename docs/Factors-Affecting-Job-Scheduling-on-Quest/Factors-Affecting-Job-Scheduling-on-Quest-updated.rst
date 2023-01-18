Factors Affecting Job Scheduling on Quest
=========================================

Jobs submitted to the Slurm scheduler on Quest are given a “priority”
score that affects when they run.

If your job is waiting on the queue, the reason is most probably one of
the following:

-  Your job’s score is lower compared to others
-  Unavailable/occupied compute resources at that moment.

Priority
--------

Total priority score is combination of several factors. These factors
are the following:

**i. Fair Share** Quest’s job scheduler uses a fair share mechanism to
dynamically determine a score. The calculation is based on the
comparison between your share of the resources and your actual usage of
these resources. If you or other members of your allocation used large
amounts of resources in the recent past, the priority of current jobs
will be lower. Accordingly, your jobs will wait longer before they start
if the scheduler queue is busy.

On the other hand, if the job queue is empty and the compute resources
are idling, regardless of the priority, your jobs will run. You will
never run out of compute hours in this model.

Fairshare also includes a recovery mechanism for job priority. The
contribution of past resource usage to priority calculations decays over
time. Without new usage, the job scores will be restored significantly
within a month.

If you are using a general access allocation, the fair share scores of
your jobs will be affected from the overall resource usage by all
members of the allocation. If you are using a buy-in allocation that has
its own compute nodes, your own usage of the those nodes will be the
determining factor for your job’s fair share score.

**ii. Allocation Type** There are two types of allocations for research
projects on Quest. Research I allocations are ideal for small to
moderate computational needs whereas Research II allocations require
considerably more resources. Due to this difference in computational
needs, a Research II allocation has a higher share of the system
resources (or initial fairshare score) compared to a Research I
allocation. This is equivalent to receiving more compute hours with a
Research II than a Research I.

**iii. Job Age** Age is length of time an eligible job has been waiting
in the queue. Jobs will accumulate priority proportional to their age.
This can help overcome starting priority differences between jobs coming
from other actors.

**iv. Partition** A priority score is associated with partition that the
job uses. This is only applicable for certain buy-in allocations which
have multiple partitions.

More detailed information about Slurm’s multi-factor priority system,
please see this
`page <https://slurm.schedmd.com/priority_multifactor.html>`__.

Backfill Scheduling
-------------------

There is a secondary mechanism that starts lower priority jobs on slots
reserved by higher priority jobs while these jobs are acquiring their
full set of resources. This is called “backfill” scheduling which helps
to increase the utilization of the compute nodes and guarantees no delay
in starting the higher priority jobs. To benefit from this mechanism, it
is important to accurately request resources (wall time, core, memory)
so that the scheduler can find appropriate space on the resource map.
Please review `resource utilization
page <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1695>`__
for different methods you can use to identify your job’s needs.

From time to time, compute resources cannot be backfilled effectively
and nodes/cores may appear idle while jobs are waiting on the queue.
