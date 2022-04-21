We breakdown the key configuration settings of your Slurm submission
script in detail, provide some details on additional settings you can
specify, and provide examples of types of jobs your can submit. The
program that schedules jobs and manages resources on Quest is called
Slurm. For video demonstrations on using Slurm on Quest please visit our
`Research Computing How-to
Videos <https://www.it.northwestern.edu/research/videos.html>`__ page.
We start off with an example Slurm job submission and follow with a
detailed discussion of each of the key configuration settings in the
script. These discussions will include the possible range of values you
can set, how to think about what value to use for the setting, and
whether or not the setting is required and if not required, what the
default is for the setting. After this, we will lay out and provide some
details on additional Slurm configuration settings that can be
specified. Finally, we will provide a number of job submission script
examples and motivate situations in which you would want to explore
using them.

.. toctree::
   :maxdepth: 3

   the_submission_script
   slurm_configuration
   slurm_commands_and_job_management
   examples_of_jobs_on_quest
   debugging_your_submission_script
