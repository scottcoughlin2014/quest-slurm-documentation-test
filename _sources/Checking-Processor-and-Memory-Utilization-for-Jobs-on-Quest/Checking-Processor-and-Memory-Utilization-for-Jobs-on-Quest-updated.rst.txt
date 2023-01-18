Checking Processor and Memory Utilization for Jobs on Quest
===========================================================

This page provides commands for checking processor (core) and RAM memory
utilization for jobs that run on Quest.

Understanding the memory and CPU requirements of your jobs will help you
to utilize Quest resources more efficiently. Below are some methods you
can apply to measure a job’s CPU and RAM usage.

How to check resource utilization for completed jobs?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Slurm provides a tool called\ ``seff``\ to check the memory utilization
and CPU efficiency for completed jobs. Note that for running and failed
jobs, the efficiency numbers reported by\ ``seff``\ are not reliable so
please use this tool only for successfully completed jobs.

We are going to look at a finished job that was submitted using the
script below:

.. code:: code

   #!/bin/bash
   #SBATCH --account=p12345
   #SBATCH --job-name=lmp-cpu
   #SBATCH --ntasks=10
   #SBATCH --ntasks-per-node=10
   #SBATCH --mem-per-cpu=100M
   #SBATCH --time=01:02:00

   module purge
   module load lammps/lammps-22Aug18

   mpirun -n 10 lmp -in in.fcc

This job submission script requests 10 tasks in a node. The scheduler
assigns one core to one task so 10 cores were assigned for this job. The
script also requests 100 megabytes per core. As a result, 1000 megabytes
were reserved for this job.

After the job is completed, we can examine the utilization report
produced by the\ ``seff <jobid>``\ command.

.. code:: code

   [abc123@quser24 ~]$ seff 549437
   Job ID: 549437
   Cluster: quest
   User/Group: abc123/abc123
   State: COMPLETED (exit code 0)
   Nodes: 1
   Cores per node: 10
   CPU Utilized: 01:06:22
   CPU Efficiency: 103.16% of 01:04:20 core-walltime
   Job Wall-clock time: 00:06:26
   Memory Utilized: 287.50 MB
   Memory Efficiency: 28.75% of 1000.00 MB

``CPU Efficiency``\ is calculated as the ratio of the actual core time
from all cores divided by the number of cores requested divided by the
run time. Here, we see that the\ ``CPU Efficiency``\ is 103% which means
that the job utilized all 10 cores fully during the run time.

``Memory Efficiency``\ is calculated as the ratio of the high-water mark
of memory used by all tasks divided by the memory requested for the job.
The total memory request for this job was 1000 megabytes and only 287.5
megabytes were used. Thus the memory efficiency is calculated as 28.75%.

Profiling your processes for memory and CPU usage before production
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``time`` command is provided with Quest’s operating system. You can
launch a program with ``/usr/bin/time`` in front of it so that the
system will watch your program and provide statistics about the CPU and
RAM usage.

To test your code, it is recommended that you start an interactive
session reserving a compute node. Here is an example test for an MPI
parallelized code (namely ``lmp``):

.. code:: code

   [abc123@qnode4233 ~]$ /usr/bin/time -v mpirun -n 10 lmp -in in.fcc > lmp.out

The output of this command is as follows:

.. code:: code

   Command being timed: "mpirun -n 10 lmp -in in.fcc"
   User time (seconds): 3682.19
   System time (seconds): 1.41
   Percent of CPU this job got: 999%
   Elapsed (wall clock) time (h:mm:ss or m:ss): 6:25.64
   Average shared text size (kbytes): 0
   Average unshared data size (kbytes): 0
   Average stack size (kbytes): 0
   Average total size (kbytes): 0
   Maximum resident set size (kbytes): 31050
   Average resident set size (kbytes): 0
   Major (requiring I/O) page faults: 51
   Minor (reclaiming a frame) page faults: 408429
   Voluntary context switches: 11770
   Involuntary context switches: 2159
   Swaps: 0
   File system inputs: 0
   File system outputs: 181592
   Socket messages sent: 0
   Socket messages received: 0
   Signals delivered: 0
   Page size (bytes): 4096
   Exit status: 0

The ``Percent of CPU this job got`` line reports core utilization. A
value around 100% means that the job run on 1 core. In this case, 999%
means that the job used about 10 cores. The number reported for
``Maximum resident set size`` will inform you about how much RAM your
task has used at most. Since the example program is using MPI
parallelization with 10 tasks and one task of the program used maximum
31050 kilobytes of memory, we can estimate that the whole job needs a
bit more than 310 megabytes.

The information gathered from this test will be helpful estimating the
memory/cpu needs of similar jobs in the future. When submitting a job, a
good practice is to request 10-15% more memory than the finding in your
memory profiling as a safety factor.

Here is another profiling example for a program (``pi_red``) that is
parallelized using OpenMP threads:

.. code:: code

   [abc123@qnode4233 ~]$ /usr/bin/time -v ./pi_red
   Command being timed: "./pi_red"
   User time (seconds): 22547.96
   System time (seconds): 0.79
   Percent of CPU this job got: 996%
   Elapsed (wall clock) time (h:mm:ss or m:ss): 37:42.85
   Average shared text size (kbytes): 0
   Average unshared data size (kbytes): 0
   Average stack size (kbytes): 0
   Average total size (kbytes): 0
   Maximum resident set size (kbytes): 832
   Average resident set size (kbytes): 0
   Major (requiring I/O) page faults: 0
   Minor (reclaiming a frame) page faults: 305
   Voluntary context switches: 19
   Involuntary context switches: 67224
   Swaps: 0
   File system inputs: 0
   File system outputs: 0
   Socket messages sent: 0
   Socket messages received: 0
   Signals delivered: 0
   Page size (bytes): 4096
   Exit status: 0

The program used 832 kilobytes of memory and about 10 CPU cores with its
threads. Unlike MPI tasks, threads share a common memory space, thus the
whole job used 832 kilobytes in this case.

How to check resource utilization for running jobs?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

While your job is running, you can examine the instantaneous memory and
CPU utilization from the compute node(s) directly. You can find out
which node(s) is (are) working on your job by using
``squeue -j <jobID>`` command. Let’s look at the MPI parallelized lmp
program which is submitted as a batch job:

.. code:: code

   [abc123@quser21 ~]$ squeue -j 549437
      JOBID PARTITION     NAME     USER  ST       TIME  NODES NODELIST(REASON)
     549437     short  lmp-cpu   abc123   R       2:34      1 qnode4233

Once you have identified the name of the node that your job is running
on, you can connect to this node directly:

.. code:: code

   [abc123@quser21 ~]$ ssh qnode4233
   [abc123@qnode4233 ~]$

You will see your command prompt name changes from the login node
(quser21 in this example) to the compute node (qnode4233 in this case).
There are two useful commands that will inform you about the resource
utilization of your program. The first one is ``ps`` which gives a
snapshot of the running processes on the node.

.. code:: code

   [abc123@qnode4233 ~]$ ps -u$USER -o %cpu,rss,args

In the command above, we request ``ps`` to report %cpu and resident set
size (rss) utilized for the running processes (args). The output is
obtained as follows:

.. code:: code

   %CPU   RSS COMMAND
    0.0  1576 /bin/bash /var/spool/slurmd/job549437/slurm_script
    0.4  5504 mpirun -n 10 lmp -in in.fcc
    100 30248 lmp -in in.fcc
    100 28384 lmp -in in.fcc
    100 28524 lmp -in in.fcc
    100 28632 lmp -in in.fcc
    100 28512 lmp -in in.fcc
    100 28264 lmp -in in.fcc
    100 28064 lmp -in in.fcc
    100 28084 lmp -in in.fcc
    100 28140 lmp -in in.fcc
    100 28668 lmp -in in.fcc
    0.0  2124 sshd: abc123@pts/10
    0.1  2140 -bash
    0.0  1572 ps -uabc123 -o %cpu,rss,argsK

The output shows 10 lmp tasks each using 100% of a CPU core and about 30
megabytes (default units for rss in ``ps`` command is kilobytes). In
total, this job has been using 10 CPU cores and around 300 megabytes of
memory when ``ps`` command was issued.

A similar result can be obtained from ``top`` (i.e. short for table of
processes) command which shows live data instead of a snapshot.

.. code:: code

   [abc123@qnode4233 ~]$ top -u$USER

The command will start the live task manager. Memory and CPU usages can
be tracked from RES and %CPU columns respectively. We see 10 lmp tasks,
each consuming around 30000 kilobytes of memory and 99.9% of one CPU.
Once you have gathered the necessary information, press ``q`` to quit
the task manager.

.. code:: code

   top - 01:07:31 up 168 days, 18:08,  1 user,  load average: 7.15, 2.42, 1.84
   Threads: 1332 total,  11 running, 1321 sleeping,   0 stopped,   0 zombie
   %Cpu(s): 50.3 us,  0.2 sy,  0.0 ni, 49.4 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
   KiB Mem : 13183529+total, 11912448+free,  6916132 used,  5794680 buff/cache
   KiB Swap:        0 total,        0 free,        0 used. 12324249+avail Mem

     PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
   16234 abc123    20   0  613580  28668  11948 R 99.9  0.0   1:15.64 lmp
   16223 abc123    20   0  629792  30700  12108 R 99.9  0.0   1:15.64 lmp
   16225 abc123    20   0  613220  28796  12276 R 99.9  0.0   1:15.67 lmp
   16229 abc123    20   0  612824  28560  12260 R 99.9  0.0   1:15.65 lmp
   16230 abc123    20   0  612824  28332  12232 R 99.9  0.0   1:15.66 lmp
   16232 abc123    20   0  612812  28116  11872 R 99.9  0.0   1:15.66 lmp
   16233 abc123    20   0  612804  28152  11920 R 99.9  0.0   1:15.66 lmp
   16226 abc123    20   0  612836  28536  12312 R 99.7  0.0   1:15.66 lmp
   16228 abc123    20   0  613184  28636  11988 R 99.7  0.0   1:15.66 lmp
   16231 abc123    20   0  612416  28088  11968 R 99.7  0.0   1:15.63 lmp

Now, let’s examine what we see for another job running the ``pi_red``
program. Note that this program is using threading for parallelization.
The ``ps`` reports 999% CPU (translates to utilizing 10 cores) and 620
kilobytes of memory (RSS).

.. code:: code

   [abc123@qnode4233 ~]$ ps -u$USER -o %cpu,rss,args
   %CPU   RSS COMMAND
    0.0  1468 /bin/bash /var/spool/slurmd/job549441/slurm_script
    999   620 ./pi_red
    0.0  2120 sshd: abc123@pts/0
    0.0  2192 -bash
    0.0  1572 ps -uabc123 -o %cpu,rss,args

The same information could be obtained from RES and %CPU columns of
``top`` command for ``pi_red`` program.

.. code:: code

   [abc123@qnode4233 ~]$ top -u$USER
   top - 15:49:20 up 123 days,  8:50,  1 user,  load average: 9.30, 4.14, 1.61
   Tasks: 279 total,   2 running, 277 sleeping,   0 stopped,   0 zombie
   %Cpu(s): 50.0 us,  0.0 sy,  0.0 ni, 49.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
   KiB Mem : 13183529+total, 10491147+free,  6206644 used, 20717172 buff/cache
   KiB Swap:        0 total,        0 free,        0 used. 12455683+avail Mem

     PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    2068 abc123    20   0   29220    620    516 R  1000  0.0  27:01.04 pi_red
    2357 abc123    20   0  157836   2376   1560 R   0.7  0.0   0:00.07 top
    1876 abc123    20   0  113120   1468   1212 S   0.0  0.0   0:00.00 slurm_script
    2082 abc123    20   0  130612   2120    936 S   0.0  0.0   0:00.00 sshd
    2083 abc123    20   0  115516   2192   1672 S   0.0  0.0   0:00.02 bash
