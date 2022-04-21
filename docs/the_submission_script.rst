.. _h_8666220334701650307877674:

The Job Submission Script
-------------------------

In this section, we present a basic submission script and how a user
would then submit a batch job using this submission script. To submit a
batch job, you first write a submission script specifying the resources
you need and what commands to run, then you submit this script to the
scheduler by running the ``sbatch`` command on the command line.

.. _h_6598678335431650314984849:

Example Submission Script
~~~~~~~~~~~~~~~~~~~~~~~~~

A submission script for a batch job could look like the following. When
substituting values, replace ``<>`` too. These commands would be saved
in a file such as ``jobscript.sh``.

::

   #!/bin/bash
   #SBATCH --account=<account> ## Required: your allocation/account name, i.e. eXXXXX, pXXXX or bXXXX
   #SBATCH --partition=short ## Required: (buyin, short, normal, long, gengpu, genhimem, etc)
   #SBATCH --time=00:10:00 ## Required: How long will the job need to run (remember different partitions have restrictions on this parameter)
   #SBATCH --nodes=1 ## how many computers/nodes do you need (no default)
   #SBATCH --ntasks-per-node=1 ## how many cpus or processors do you need on per computer/node (default value 1)
   #SBATCH --mem=1G ## how much RAM do you need per computer/node (this effects your FairShare score so be careful to not ask for more than you need))
   #SBATCH --job-name=sample_job ## When you run squeue -u NETID this is how you can identify the job
   #SBATCH --output=output.log ## standard out and standard error goes to this file


   module purge all
   module load python-anaconda3
   source activate /projects/intro/envs/slurm-py37-test
    
   python --version
   python slurm_test.py

The first line of the script loads the bash shell. Lines that begin with
``#SBATCH`` are interpreted by Slurm. Until Slurm places the job on a
compute node, no other line in this script is executed. In these lines,
``#`` is needed; it is not a comment character when used with
``#SBATCH``.

After the Slurm commands, the rest of the script works like a regular
Bash script. You can modify environment variables, load modules, change
directories, and execute program commands. Lines in the second half of
the script that start with ``#`` are comments.

Find a downloadable copy of this example script `on
GitHub <https://github.com/nuitrcs/examplejobs>`__.

.. _h_943032369801650314991617:

Submitting Your Batch Job
~~~~~~~~~~~~~~~~~~~~~~~~~

After you've written and saved your submission script, you can submit
your job. At the command line type

.. code:: code

   sbatch <name_of_script>

where, in the example above ``<name_of_script>`` would be
``jobscript.sh``. Upon submission the scheduler will return your job
number:

.. code:: code

   Submitted batch job 549005

If you would prefer the return value of your job submission to be just
the job number, pass the ``--parsable`` argument:

.. code:: code

   sbatch --parsable <name_of_script>
   549005

This may be desirable if you have a workflow that accepts the return
value as a variable for job monitoring or dependencies.

If there is an error in your job submission script, the job will not be
accepted by the scheduler and you will receive an error message right
away, for example:

.. code:: code

   sbatch: error: Batch job submission failed: Invalid account or account/partition combination specified

If your job submission receives an error, you will need to resubmit your
job. If no error is received, your job has entered the queue and will
run.
