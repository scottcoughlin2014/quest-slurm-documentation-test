Examples of Jobs on Quest
=========================

Additional details and examples of Quest job submission scripts and
commands.

`Interactive Jobs <#interactive-jobs>`__

`Batch Jobs <#batch-jobs>`__

`Job Array <#job-array>`__

`Dependent Jobs <#dependent-jobs>`__

See Research Computing’s `GitHub repository of example
jobs <https://github.com/nuitrcs/examplejobs>`__ for language/software
specific examples and scripts you can copy and modify.

Interactive Jobs
----------------

Example: Interactive Job with More than One Node
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you submit an interactive job requiring more than one node, you will
land on the Head Node for your job once the scheduler finds the
resources. For more information see `Submitting an Interactive
Job <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__.

.. code:: code

   [abc123@quser22 ~]$ srun -A p12345 -p normal -N 2 --ntasks-per-node=10 --mem-per-cpu=1G --time=01:00:00 --pty bash -l
   ----------------------------------------
   srun job start: Thu Feb 28 16:31:27 CST 2019
   Job ID: 557781
   Username: abc123
   Queue: normal
   Account: p12345
   ----------------------------------------
   The following variables are not
   guaranteed to be the same in
   prologue and the job run script
   ----------------------------------------
   PATH (in prologue) : /hpc/slurm/usertools:/usr/lib64/qt-3.3/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/lpp/mmfs/bin:/opt/ibutils/bin:/home/abc123/bin
   WORKDIR is: /home/abc123
   ----------------------------------------
   [abc123@qnode4217 ~]$

This interactive job requests 2 nodes and 10 cores in each of those
nodes. In total you will have access to 20 cores. Since the job reserved
1 GB per core, 20 GB of RAM is allocated in total (i.e. 10 GB in each
node).

To find the name of the other compute node(s) that you have been
assigned as part of your job, you can use the ``squeue`` command in a
from the same terminal session. Look under NODELIST in the ``squeue``
output:

.. code:: code

   [abc123@qnode4217 ~]$ squeue
   JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
   557781    normal     bash  abc123   R       5:51      2 qnode[4217-4218]

You can then ssh to the other node(s) if needed as long as your job is
active.

.. code:: code

   ssh qnode4218

Batch Jobs
----------

Job Array
~~~~~~~~~

Job arrays can be used to submit multiple jobs at once that use the same
application script. This can be useful if you want to run the same
script multiple times with different input parameters.

In the example below, the ``--array`` option defines the job array, with
a specification of the index numbers you want to use (in this case, 0
through 9). The ``$SLURM_ARRAY_TASK_ID`` bash environmental variable
takes on the value of the job array index for each job (so here, integer
values 0 through 9, one value for each job). In this example, the value
of ``$SLURM_ARRAY_TASK_ID`` is used to select the correct index from the
input_args bash array which was constructed by reading in
*input_args.txt*, each row of which is then passed on to a script as
command line arguments.

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
   #SBATCH --mem-per-cpu=1G ## how much RAM do you need per CPU (this effects your FairShare score so be careful to not ask for more than you need))
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

In this example, ``myscript.py`` will receive the values in
``input.csv`` as arguments: the first field will be ``sys.argv[1]``, the
second field will be ``sys.argv[2]``, etc.

**Note: make sure the number you specify for the ``--array`` parameter
matches the number of lines in your input file!**

Also, note that in this example standard output and error files are
printed separately for each element of the job array with the
``--output`` and ``--error`` options. To avoid each element overwriting
these files, tag them with jobID (``%A``) and elementID (``%a``)
variables (which are automatically assigned by the scheduler) so
elements have their own distinct output and error files.

Submit this script with:

.. code:: code

   sbatch jobsubmission.sh

The job array will then be submitted to the scheduler.

Dependent Jobs
~~~~~~~~~~~~~~

Dependent jobs are a series of jobs which run or wait to run conditional
on the state of another job. For instance, you may submit two jobs and
you want the first job to complete successfully before the second job
runs. In order to submit this type of workflow, you pass *sbatch* the
jobid of the job that needs to finish before this job starts via the
command line argument:

::

   --dependency=afterok:<jobid>

To accomplish this, it is helpful to write all of your *sbatch* commands
in bash script. You will notice that anything you can tell slurm via
#SBATCH in the submission script itself, you can also pass to *sbatch*
via the command line. The key here is that the bash variable *jid0,
jid1, jid2* will contain the jobid that SLURM assigns after you run the
*sbatch*\ command.

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

In this example, we print the job id that had to finish in order for the
dependent job to begin. Therefore, the very first job should print 0
because it did not rely on any job to finish in order to run but the
second job should print the jobid of the first job and so on.

.. code:: code

   bash wrapper_script.sh

This will submit the three jobs in sequence and you should see jobs 2
and 3 pending for reason DEPENDENCY.
