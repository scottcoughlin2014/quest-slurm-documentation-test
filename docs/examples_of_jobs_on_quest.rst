.. _h_90022083356871649361533121:

Example of Jobs on Quest
------------------------

In this section, we provide details and examples of how to use Slurm to
run interactive jobs, job arrays, and jobs that depend on each other.

.. _section-interactive-jobs:

Interactive Job Examples
~~~~~~~~~~~~~~~~~~~~~~~~

.. container:: panel-content

   .. rubric:: Submitting an Interactive Job (to run an application
      *without* Graphical User Interface)
      :name: section-section-interactive-jobs-non-gui
      :class: panel-head

   .. container:: panel-content

      To launch an interactive job from the Quest log-in node in order
      to run an application *without* a GUI use either the
      `srun <https://slurm.schedmd.com/srun.html>`__ or
      `salloc <https://slurm.schedmd.com/salloc.html>`__ command. If you
      use ``srun`` to run an interactive job, then SLURM will
      automatically launch a terminal session on the compute node after
      it schedules the job and you simply need to wait for this to
      happen. *Due to the behavior of ``srun``, if you lose connection
      to your interactive session, the interactive job will terminate.*

      .. code:: code

         [quser23 ~]$ srun -N 1 -n 1 --account=<account> --mem=XXG --partition=<partition> --time=<hh:mm:ss> --pty bash -l
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
      directly connect to the compute node. *Due to the behavior of
      ``salloc``, if you lose connection to your interactive session,
      the interactive job will*\ **not**\ *terminate.
      *

      ::

         [quser21 ~]$ salloc -N 1 -n 1 --account=<account> --mem=<XXG> --partition=<partition> --time=<hh:mm:ss>
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
      :class: panel-head

   .. container:: panel-content

      To launch an interactive job from the Quest log-in node in order
      to run an application *with* a GUI, first you need to connect to
      Quest using an application with X11 forwarding support. We
      recommend `using the FastX3
      client <https://kb.northwestern.edu/page.php?id=69237>`__. Once
      you have connected to Quest with X11 forwarding enabled, you can
      then use either the `srun <https://slurm.schedmd.com/srun.html>`__
      or `salloc <https://slurm.schedmd.com/salloc.html>`__ command. If
      you use ``srun`` to run an interactive job, then SLURM will
      automatically launch a terminal session on the compute node after
      it schedules the job and you simply need to wait for this to
      happen. *Due to the behavior of ``srun``, if you lose connection
      to your interactive session, the interactive job will terminate.*

      .. code:: code

         [quser23 ~]$ srun --x11 -N 1 -n 1 --account=<account> --mem=XXG --partition=<partition> --time=<hh:mm:ss> --pty bash -l
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
      directly connect to the compute node. *Due to the behavior of
      ``salloc``, if you lose connection to your interactive session,
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

.. _section-job-array:

Job Array
~~~~~~~~~

.. container:: panel-content

   Job arrays can be used to submit multiple jobs at once that use the
   same application script. This can be useful if you want to run the
   same script multiple times with different input parameters.
   In the example below, the --array option defines the job array, with
   a specification of the index numbers you want to use (in this case, 0
   through 9). The $SLURM_ARRAY_TASK_ID bash environmental variable
   takes on the value of the job array index for each job (so here,
   integer values 0 through 9, one value for each job). In this example,
   the value of $SLURM_ARRAY_TASK_ID is used to select the correct index
   from the input_args bash array which was constructed by reading in
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

   In this example, myscript.py will receive the values in input.csv as
   arguments: the first field will be sys.argv[1], the second field will
   be sys.argv[2], etc.

   **Note: make sure the number you specify for the --array parameter
   matches the number of lines in your input file!**

   Also, note that in this example standard output and error files are
   printed separately for each element of the job array with the
   --output and --error options. To avoid each element overwriting these
   files, tag them with jobID (%A) and elementID (%a) variables (which
   are automatically assigned by the scheduler) so elements have their
   own distinct output and error files.

   Submit this script with:

   .. code:: code

      sbatch jobsubmission.sh

   The job array will then be submitted to the scheduler.

.. _section-dependent-jobs:

Dependent Jobs
~~~~~~~~~~~~~~

.. container:: panel-content

   Dependent jobs are a series of jobs which run or wait to run
   conditional on the state of another job. For instance, you may submit
   two jobs and you want the first job to complete successfully before
   the second job runs. In order to submit this type of workflow, you
   pass *sbatch* the jobid of the job that needs to finish before this
   job starts via the command line argument:
   ::

      --dependency=afterok:<jobid>

   To accomplish this, it is helpful to write all of your *sbatch*
   commands in bash script. You will notice that anything you can tell
   slurm via #SBATCH in the submission script itself, you can also pass
   to *sbatch* via the command line. The key here is that the bash
   variable *jid0, jid1, jid2* will contain the jobid that SLURM assigns
   after you run the *sbatch*\ command.

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

   In this example, we print the job id that had to finish in order for
   the dependent job to begin. Therefore, the very first job should
   print 0 because it did not rely on any job to finish in order to run
   but the second job should print the jobid of the first job and so on.

   .. code:: code

      bash wrapper_script.sh

   This will submit the three jobs in sequence and you should see jobs 2
   and 3 pending for reason DEPENDENCY.
