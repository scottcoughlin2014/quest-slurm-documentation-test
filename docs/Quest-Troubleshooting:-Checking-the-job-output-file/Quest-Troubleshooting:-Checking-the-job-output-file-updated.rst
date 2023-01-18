Quest Troubleshooting: Checking the job output file
===================================================

This page demonstrates an example of looking problems in a job output
files when a job fails.

When a job fails, the first place to look is the output/error file for
your job. Unless you explicitly directed it elsewhere, it will be in the
directory from which you submitted your job. Even if you directed output
from your script/program to another location, there is still an output
file with information about the job itself. By default, the output file
is named slurm-\<jobID>.out and it contains both the standard output and
error. You can use the cat command to print the contents to the
terminal, or open the file in your preferred text editor.

If the job exit value, ExitCode (listed in ``checkjob <JobID>`` report)
is anything other than 0:0, the scheduler thinks something went wrong
with the job.

Here is an example of the error/output file for a job where the job
submission script referenced a command that wasn’t found. The relevant
notice is given as: line 10: lmpx: command not found. The line number
refers to lines in the job submission script. A command not found can
happen if you have not loaded the necessary module to add the necessary
executable command to your path. It is also possible that there is a
typo in your the name of the executable.

.. code:: code

   [akh9585@quser21 fail_example]$ cat slurm-549001.out
   /var/spool/slurmd/job549001/slurm_script: line 10: lmpx: command not found

For this case, ``checkjob 549001`` report includes the following lines:

.. code:: code

   JobState=FAILED Reason=NonZeroExitCode Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=127:0

Slurm reports that the job is FAILED in JobState and the ExitCode is
given as 127:0. The scheduler obtains the exit code from bash return
code. Bash returns 127 when the command doesn’t exist.
