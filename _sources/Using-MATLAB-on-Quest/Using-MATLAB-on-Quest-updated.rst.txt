Using MATLAB on Quest
=====================

Setup and reference instructions for running MATLAB on Quest.

We provide a summary of the different ways to run MATLAB on Quest and
which option is right for you will depend on the type of MATLAB code you
are running and if parallelization (either explicit through the use of
``parfor`` or implicit through MATLAB’s internal multithreading) will
provide impactful speeds up of your software.

-  `Submitting single core MATLAB jobs <#section-matlab-single-core>`__

   -  Simple SLURM Submission Script

-  `Submitting many single core MATLAB jobs, changing only the input
   arguments <#section-matlab-job-array>`__

   -  SLURM Job Array (embarrassingly parallel)

-  `Submitting jobs which make use of MATLAB’s internal
   multithreading <#section-matlab-multithreading>`__

   -  Cannot scale past 1 node and not all code/functions can be
      parallelized in this manner

-  `Submitting Scalable Parallel MATLAB
   Jobs <#section-matlab-scalable-jobs>`__

   -  Scales from 1 computer to multiple computers seamlessly

Expand All Collapse All

.. container:: panel panel-default

   .. container:: panel-heading

      Submitting single core MATLAB jobs

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      In many situations, it will be most beneficial to run a single
      core MATLAB job. Even when you are not explicitly parallelizing
      your code, you should be aware of `MATLAB’s use of
      multithreading <#section-matlab-multithreading>`__ which will need
      to be disabled by passing the command line argument
      ``-singleCompThread`` in order to run a single core MATLAB job.
      Below we provide an example submission script called
      ``example_submit.sh`` and an example MATLAB script called
      ``simple.m``.

      **example_submit.sh**

      ::

         #!/bin/bash
         #SBATCH --account=w10001 ## YOUR ACCOUNT pXXXX or bXXXX
         #SBATCH --partition=w10001 ### PARTITION (buyin, short, normal, w10001, etc)
         #SBATCH --nodes=1 ## Never need to change this
         #SBATCH --ntasks-per-node=1 ## Never need to change this
         #SBATCH --time=00:10:00 ## how long does this need to run (remember different partitions have restrictions on this param)
         #SBATCH --mem=4G ## how much RAM you need per computer (this effects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --output=matlab_job.out ## standard out and standard error goes to this file


         ## job commands; simple is the MATLAB .m file, specified without the .m extension
         module load matlab/r2021b
         matlab -singleCompThread -batch "input_arg1='arg1';input_arg2='arg2';input_arg3='arg3';simple"

      **simple.m**

      ::

         %input_arg1 is set in the submission script that calls this file
         %input_arg2 is set in the submission script that calls this file
         %input_arg3 is set in the submission script that calls this file


         disp(input_arg1)
         disp(input_arg2)
         disp(input_arg3)


         disp('Start Sim')
         t0 = tic;
         for i = 1:100000
         A{i} = rand(1);
         end
         t = toc(t0);

         X = sprintf('Simulation took %f seconds',t);
         disp(X)

      If both of these files are in the same folder, you can run this
      MATLAB job through SLURM by running

      ``sbatch example_submit.sh``

.. container:: panel panel-default

   .. container:: panel-heading

      Submitting many single core MATLAB jobs, changing only the input
      arguments

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      Let’s say that we want to submit the exact same single core MATLAB
      job many times over while only changing the arguments that are
      passed to ``simple.m``. We can accomplish this through a using a
      `SLURM Job
      Array <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1551>`__.
      In the example below, the ``--array`` option defines the job
      array, with a specification of the index numbers you want to use
      (in this case, 0 through 9). The ``$SLURM_ARRAY_TASK_ID`` bash
      environmental variable takes on the value of the job array index
      for each job (so here, integer values 0 through 9, one value for
      each job). In this example, the value of ``$SLURM_ARRAY_TASK_ID``
      is used to select the correct index from the ``input_args`` bash
      array which was constructed by reading in *input_arguments.txt*,
      each row of which is then passed on to a script as command line
      arguments. Essentially, each row of *input_arguments.txt*
      represents a job in your job array and each column represents an
      input argument (in this example input_arguments[1-3]) to your
      script.

      **example_submit.sh**

      ::

         #!/bin/bash
         #SBATCH --account=w10001 ## YOUR ACCOUNT pXXXX or bXXXX
         #SBATCH --partition=w10001 ### PARTITION (buyin, short, normal, w10001, etc)
         #SBATCH --array=0-9 ## number of jobs to run "in parallel"
         #SBATCH --nodes=1 ## Never need to change this
         #SBATCH --ntasks-per-node=1 ## Never need to change this
         #SBATCH --time=15:00:00 ## how long does this need to run (remember different partitions have restrictions on this param)
         #SBATCH --mem=8G ## how much RAM do you need per computer (this effects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --job-name="sample_job_\${SLURM_ARRAY_TASK_ID}" ## use the task id in the name of the job
         #SBATCH --output=sample_job.%A_%a.out ## use the jobid (A) and the specific job index (a) to name your log file


         # read in each row of the input_arguments text file into an array called input_args
         IFS=$'\n' read -d '' -r -a input_arguments < input_arguments.txt

         # Use the SLURM_ARRAY_TASK_ID varaible to select the correct index from input_arguments and then split the string by whatever delimiter you chose (in our case each input argument is split by a space)
         IFS=' ' read -r -a input_args <<< "${input_arguments[$SLURM_ARRAY_TASK_ID]}"

         # Pass the input arguments associated with this SLURM_ARRAY_TASK_ID to your function.
         ## job commands; job_array is the MATLAB .m file, specified without the .m extension
         module load matlab/r2021b
         matlab -singleCompThread -batch "input_arg1='${input_args[0]}';input_arg2='${input_args[1]}';input_arg3='${input_args[2]}';simple"

      **input_arguments.txt**

      ::

         job1_arg1 job1_arg2 job1_arg3
         job2_arg1 job2_arg2 job2_arg3
         job3_arg1 job3_arg2 job3_arg3
         job4_arg1 job4_arg2 job4_arg3
         job5_arg1 job5_arg2 job5_arg3
         job6_arg1 job6_arg2 job6_arg3
         job7_arg1 job7_arg2 job7_arg3
         job8_arg1 job8_arg2 job8_arg3
         job9_arg1 job9_arg2 job9_arg3
         job10_arg1 job10_arg2 job10_arg3

      If both of these files are in the same folder, you can run this
      MATLAB job array through SLURM by running

      ``sbatch example_submit.sh``

.. container:: panel panel-default

   .. container:: panel-heading

      Multithreading (MATLAB Computational Threads)

   .. container:: panel panel-body js-panelnormalswitches2 collapse

      MATLAB has built-in multithreading for some linear algebra and
      numerical functions. By default, MATLAB will try to use *all of
      the cores* on a machine to perform these computations. However, if
      a job you’ve submitted to Quest uses more cores than were
      requested, the job will be cancelled. If you want MATLAB to be
      able to use multiple cores for these calculations, then you can
      **omit** the ``-singleCompThread`` option when starting MATLAB and
      add the following line to the **beginning** of your main *.m*
      script.

      .. code:: code

         maxNumCompThreads(str2num(getenv('SLURM_NPROCS')));

      Adding this line to your *.m* script is to be used in combination
      with the following options below in your submission script or
      ``sbatch`` command:

      .. code:: code

         #SBATCH --nodes=1
         #SBATCH --ntasks-per-node=<numberofcores>

      **Example**

      .. code:: code

         #SBATCH --nodes=1
         #SBATCH --ntasks-per-node=8

      This will allow MATLAB to spawn 8 parallel threads which is equal
      to the 8 CPU resources that you will have requested and received
      from the scheduler.

      Note: Setting the maximum number of computational threads using
      ``maxNumCompThreads`` does not propagate to your next MATLAB
      session and so if you are typing commands in the command window,
      then you must do this everytime you start a new MATLAB session.

      Below, we show both an example of a MATLAB program that **would
      benefit** from utilizing MATLAB’s internal multithreading and by
      exactly how much it would benefit and an example of a MATLAB
      program that **would not benefit** from utilizing MATLAB’s
      internal multithreading. In the following program(s), the MATLAB
      function that benefits from MATLAB’s auto-multithreading is
      ``svd``. For both of the MATLAB programs below ``threads_help.m``
      and ``threads_do_not_help.m``, we make two different computing
      resource requests to highlight the impact of multithreading, 1 CPU
      on 1 computer and 8 CPUs on 1 computer. The former request will
      restrict ``maxNumCompThreads`` to be 1 which is equivalent to
      running MATLAB with the ``-singleCompThread`` option.

      **threads_help.m**

      ::

         disp('Start Sim')
         maxNumCompThreads(str2num(getenv('SLURM_NPROCS')));

         t0 = tic;

         for i = 5000:5100
           z = randn(i);
           max(svd(z));
         end
         t = toc(t0);

         X = sprintf('Simulation took %f seconds',t);
         disp(X)

      Due to the size of the matrices being passed to ``svd`` (somewhere
      between 5000x5000 and 5100x5100), we **do find** a substantive
      difference (~5 time faster) when ``svd`` is able to parallelize
      over 8 CPUs. *Note that the scaling is not 1 to 1 in terms of how
      much faster the simulations goes and how many CPUs you give MATLAB
      access to. In this case, access to 8 CPUs leads only to a 5X speed
      up, not 8.*

      8 CPUs: Simulation took 1233.587702 seconds

      1 CPU: Simulation took 6446.807831 seconds

      **threads_do_not_help.m**

      ::

         disp('Start Sim')
         maxNumCompThreads(str2num(getenv('SLURM_NPROCS')));

         t0 = tic;

         for i = 500:510
           z = randn(i);
           max(svd(z));
         end
         t = toc(t0);

         X = sprintf('Simulation took %f seconds',t);
         disp(X)

      Due to the size of the matrices being passed to ``svd`` (somewhere
      between 500x500 and 510x510), we **do not find** a substantive
      difference (~1.5 time faster) when ``svd`` is able to parallelize
      over 8 CPUs.

      1 CPU: Simulation took 1.044554 seconds

      8 CPUs: Simulation took 0.683330 seconds

      A final note, it is very likely that if you run the same program
      on Quest as you do on your local labtop or workstation `in a
      single core configuration <#section-matlab-single-core>`__ you
      will find that it will take longer to run on Quest than on your
      laptop due to this automated multithreading behavior from MATLAB.
      That being said, enabling the multithreading behavior in MATLAB
      comes at a cost of needing to request more CPUs which may or may
      not being used efficiently by MATLAB depending on the exact
      functions you call and the size of the data/matrices being
      operated on as can be seen from the above example.

.. container:: panel panel-default

   .. container:: panel-heading

      Submitting Scalable (Single or Multi node) Parallel MATLAB Jobs

   .. container:: panel panel-body js-panelnormalswitches3 collapse

      *Note that any version before 2017a (i.e. 2016a, 2015a, 2014b,
      2014a and 2013a) is not supported by Slurm scheduler for scalable
      parallelization.*

      To run parallel jobs on Quest that use cores across multiple
      nodes, you need to create a parallel profile.

      Log in to Quest with X-forwarding enabled (use the -X option when
      connecting via ssh or connect with FastX to have X-forwarding
      enabled by default).

      Launch MATLAB on the login node you land on from your home
      directory:

      ::

         module load matlab/r2021b
         matlab

      .. rubric:: CONFIGURATION OF QUEST CLUSTER
         :name: configuration-of-quest-cluster

      We will configure MATLAB to run parallel jobs on Quest by calling
      ``configCluster``. ``configCluster`` only needs to be called once
      per version of MATLAB. Please be aware that running
      ``configCluster`` more than once per version will reset your
      cluster profile back to default settings and erase any saved
      modifications to the profile.

      ::

         >> rehash toolboxcache
         >> configCluster

      .. rubric:: Jobs will now default to the cluster rather than
         submit to the local machine.
         :name: jobs-will-now-default-to-the-cluster-rather-than-submit-to-the-local-machine.

      *Although the Cluster Profile created by this script will work out
      of the box, we recommend slightly modifying the Cluster Profile.*

      If you are using the MATLAB GUI on Quest, you should be able to
      see and modify new cluster profile by doing the following:
      Parallel > Create and Manage Clusters). You should see that the
      default Cluster Profile is now quest R20XXa/b depending on what
      version of MATLAB > r2017a you are using.

      |find-cluster-profile-manager| |clusterprofilemanager|

      Click *Edit* and change the *JobStorageLocation* entry and then
      click *Done*. We recommend updating the *JobStorageLocation* to be
      the default value of current working directory (which can be
      achieved by leaving the PATH blank).

      .. image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Change-JobStorageLocation-to-default.png
         :alt: changeJobStorageLocation
         :width: 619px
         :height: 401px

      .. rubric:: CONFIGURING JOBS
         :name: configuring-jobs

      Prior to submitting the job, we can specify various parameters to
      pass to our jobs, such as partition, allocation, walltime, etc.

      ::

         >> % Get a handle to the cluster
         >> c = parcluster;

      The following options are *required* in order to submit a MATLAB
      job to the cluster.

      ::

         >> % Specify the walltime (e.g. 4 hours)
         >> c.AdditionalProperties.WallTime = '04:00:00';

         >> % Specify an account to use for MATLAB jobs (e.g. pXXXX, bXXXX, etc)
         >> c.AdditionalProperties.AccountName = 'account-name';

         >> % Specify a queue to use for MATLAB jobs (e.g. short, normal, long)
         >> c.AdditionalProperties.QueueName = 'queue-name';

      The following arguments are *optional* but are worth considering
      when running MATLAB jobs on the cluster

      ::

         >> % Specify memory to use for MATLAB jobs, per core (default: 4gb)
         >> c.AdditionalProperties.MemUsage = '6gb';

         >> % Specify number of nodes to use 
         >> c.AdditionalProperties.Nodes = 1; 

         >> % Require exclusive node
         >> c.AdditionalProperties.RequireExclusiveNode = false;

         >> % Specify e-mail address to receive notifications about your job
         >> c.AdditionalProperties.EmailAddress = 'user-id@northwestern.edu';

      The following arguments are apply when running GPU accelerated
      MATLAB jobs

      ::

         >> % Specify number of GPUs
         >> c.AdditionalProperties.GpusPerNode = 1;

         >> % Specify type of GPU card to use (e.g. a100)
         >> c.AdditionalProperties.GpuCard = '';

      Save changes after modifying AdditionalProperties for the above
      changes to persist between MATLAB sessions.

      ::

         >> c.saveProfile

      To see the values of the current configuration options, display
      AdditionalProperties.

      ::

         >> % To view current properties
         >> c.AdditionalProperties

      Unset a value when no longer needed.

      ::

         >> % Turn off email notifications
         >> c.AdditionalProperties.EmailAddress = '';
         >> c.saveProfile

      .. rubric:: PARALLEL BATCH JOB
         :name: parallel-batch-job

      Users can submit parallel workflows with the ``batch`` command
      either with or without the MATLAB GUI. Let’s use the following
      example for a parallel job, which is saved
      as\ ``quest_parallel_example.m``.

      ::

         disp('Start Sim')

         iter = 100000;
         t0 = tic;
         parfor idx = 1:iter
           A(idx) = idx;
         end
         t = toc(t0);

         X = sprintf('Simulation took %f seconds',t);
         disp(X)

         save RESULTS A t

      .. rubric:: SUBMITTING A PARALLEL BATCH JOB THROUGH THE MATLAB GUI
         :name: submitting-a-parallel-batch-job-through-the-matlab-gui

      First, we will make a MATLAB script called ``submit_matlab_job.m``
      which we will place in the same directory as
      ``quest_parallel_example.m`` and will look a lot like a SLURM
      submission script.

      ::

         % Get a handle to the cluster
         c = parcluster;

         %% Required arguments in order to submit MATLAB job

         % Specify the walltime (e.g. 4 hours)
         c.AdditionalProperties.WallTime = '01:00:00';

         % Specify an account to use for MATLAB jobs (e.g. pXXXX, bXXXX, etc)
         c.AdditionalProperties.AccountName = 'w10001';

         % Specify a queue/partition to use for MATLAB jobs (e.g. short, normal, long)
         c.AdditionalProperties.QueueName = 'w10001';

         %% optional arguments but worth considering

         % Specify memory to use for MATLAB jobs, per core (default: 4gb)
         c.AdditionalProperties.MemUsage = '5gb';

         % Specify number of nodes to use
         c.AdditionalProperties.Nodes = 1;

         % Specify e-mail address to receive notifications about your job
         c.AdditionalProperties.EmailAddress = 'quest_demo@northwestern.edu';

         % The script that you want to run through SLURM needs to be in the MATLAB PATH
         % Here we assume that quest_parallel_example.m lives in the same folder as submit_matlab_job.m
         addpath(pwd)

         % Finally we will submit the MATLAB script quest_parallel_example to SLURM such that MATLAB
         % will request enough resources to run a parallel pool of size 4 (i.e. parallelize over 4 CPUs).,
         job = c.batch('quest_parallel_example', 'Pool', 16, 'CurrentFolder', '.');

      After you have written your SLURM submission script like MATLAB
      program, run ``submit_matlab_job.m`` using the *Run* button in the
      MATLAB GUI.

      .. image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Run-Submit-Matlab-Job.png
         :alt: run-matlab-job-through-gui
         :width: 704px
         :height: 332px

      MATLAB should output the arguments that it passes to the
      ``sbatch`` command used to submit a job to SLURM and which are
      based on the configuration settings in ``submit_matlab_job.m.``
      *Note that the number of tasks submitted is the size of the
      parallel pool plus one*. This extra CPU is for the root or main
      MATLAB worker. You should also see a folder called JobX appear in
      your current working directory as can be seen in the screen shot
      below. This is the folder where the MATLAB job is running and
      where any print statements in your code will show up.

      .. image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Result-Of-Running-submit_matlab_job.png
         :alt: submit-matlab-job-result
         :width: 707px
         :height: 506px

      .. container::

         If we enter into the *Job1* folder, we will see that for every
         MATLAB task and/or worker there has a set of files associated
         with it. The most important file to keep in mind is
         ``Task1.diary.txt`` which is where any print or display
         statements that you have in your script will go. We demonstrate
         what ``Task1.diary.txt`` would contain based on our example job
         ``quest_parallel_example.m``.

      .. container::

      .. container::

         |Job1-Folder| |task1-diary-txt|

      .. rubric:: MONITORING YOUR JOB THROUGH THE JOB MONITOR
         :name: monitoring-your-job-through-the-job-monitor

      MATLAB provides an application called the Job Monitor which can
      also be a handy way of checking on the status of your MATLAB jobs.
      To launch Job Monitor from the MATLAB GUI, go to Parallel > Job
      Monitor .

      |job-monitor-find| |job-monitor|

      Job Monitor allows you to…

      -  Check on the state of your MATLAB job (pending, running,
         finished).
      -  If the job failed, **Show Errors** will display any MATLAB
         error messages.
      -  If you had print statements in your MATLAB job (i.e. disp,
         fprintf, etc), then **Show Diary** will show all the standard
         output from your job.
      -  Finally, **Load Variables** will allow you to load all of the
         variables defined in your program to your current WorkSpace.

      .. image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Job-Monitor-Options-Show-Diary-Load-Vairables.png
         :alt: job-monitor-options
         :width: 598px
         :height: 341px

      We demonstrate running *Show Diary* and *Load Variables* after
      successfully running the program ``quest_parallel_example.m``.
      Note the variables added to our workspace after running *Load
      Variables*.

      .. image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Result-of-Show-Diary-And-Load-Variables.png
         :alt: show-diary-and-load-variables
         :width: 744px
         :height: 487px

      .. rubric:: SUBMITTING A PARALLEL BATCH JOB WITHOUT THE MATLAB GUI
         :name: submitting-a-parallel-batch-job-without-the-matlab-gui

      If you do not plan to use the MATLAB GUI to run
      ``submit_matlab_job.m`` then you will want to create a bash script
      which will load MATLAB and run MATLAB in command line only mode.

      Create a file called ``submit_matlab_job_wrapper.sh`` which
      contains these two lines

      ::

         module load matlab/r2021b
         matlab -singleCompThread -batch submit_matlab_job

      All that is left to do is to submit the job by running

      ``$ bash submit_matlab_job_wrapper.sh``

      on the command line. This will take a little while to run but you
      will know when MATLAB has submitted a job to SLURM when it outputs
      the ``sbatch`` command that it ran based on the configuration
      settings in ``submit_matlab_job.m.`` For example, the above MATLAB
      submission script would produce this output:

      ::

         $ bash submit_matlab_job_wrapper.sh

         additionalSubmitArgs =

            '--ntasks=17 --cpus-per-task=1 --ntasks-per-core=1 -A w10001 -t 01:00:00 -p w10001 -N 1 --mem-per-cpu=5gb --mail-type=ALL --mail-user=quest_demo@northwestern.edu'

      Note that the number of tasks submitting is the size of the
      parallel pool plus one. This extra CPU is for the root or main
      MATLAB worker.

      .. rubric:: GPU BATCH JOB
         :name: gpu-batch-job

      Users can submit GPU workflows with the ``batch`` command either
      with or without the MATLAB GUI. Let’s use the following example
      for a GPU job, which is saved as\ ``quest_gpu_example.m``.

      ::

         display(gpuDevice)
         A = gpuArray([1 0 1; -1 -2 0; 0 1 -1]);
         e = eig(A);

      .. rubric:: SUBMITTING A GPU BATCH JOB WITHOUT THE MATLAB GUI
         :name: submitting-a-gpu-batch-job-without-the-matlab-gui

      In order to submit the above MATLAB job without the GUI, we will
      create two additional scripts, one BASH script and one MATLAB
      script.

      First, we will make a MATLAB script called ``submit_matlab_job.m``
      which will look a lot like a GPU SLURM submission script.

      ::

         % Get a handle to the cluster
         c = parcluster;

         %% Required arguments in order to submit a MATLAB GPU job
         % Specify the walltime (e.g. 4 hours)
         c.AdditionalProperties.WallTime = '01:00:00';
         % Specify an account to use for MATLAB jobs (e.g. pXXXX, bXXXX, etc)
         c.AdditionalProperties.AccountName = 'w10001';
         % Specify a queue/partition to use for MATLAB jobs (e.g. short, normal, long)
         c.AdditionalProperties.QueueName = 'w10001';
         % Specify number of GPUs
         c.AdditionalProperties.GpusPerNode = 1;
         % Specify type of GPU card to use (e.g. a100)
         c.AdditionalProperties.GpuCard = 'a100';

         %% optional arguments but worth considering
         % Specify memory to use for MATLAB jobs, per core (default: 4gb)
         c.AdditionalProperties.MemUsage = '5gb';
         % Specify number of nodes to use
         c.AdditionalProperties.Nodes = 1;
         % Specify e-mail address to receive notifications about your job
         c.AdditionalProperties.EmailAddress = 'quest_demo@northwestern.edu';

         % The script that you want to run through SLURM needs to be in the MATLAB PATH
         % Here we assume that quest_gpu_example.m lives in the same folder as submit_matlab_job.m
         addpath(pwd)

         % Finally we will submit the MATLAB script quest_gpu_example to SLURM such that MATLAB
         job = c.batch('quest_gpu_example', 'CurrentFolder', '.');

      After you have written your SLURM submission script like MATLAB
      program, we create a bash script which will simply run this MATLAB
      script which will submit a job to SLURM.

      Create a file called ``submit_matlab_job_wrapper.sh`` which
      contains these two lines

      ::

         module load matlab/r2021b
         matlab -singleCompThread -batch submit_matlab_job

      All that is left to do is to submit the job by running

      ``$ bash submit_matlab_job_wrapper.sh``

      on the command line. This will take a little while to run but you
      will know when MATLAB has submitted a job to SLURM when it outputs
      the ``sbatch`` command that it ran based on the configuration
      settings in ``submit_matlab_job.m.`` For example, the above MATLAB
      submission script would produce this output:

      ::

         $ bash submit_matlab_job_wrapper.sh

         additionalSubmitArgs =

            '--ntasks=1 --cpus-per-task=1 --ntasks-per-core=1 -A w10001 -t 01:00:00 -p w10001 -N 1 --gres=gpu:a100:1 --mem-per-cpu=5gb --mail-type=ALL --mail-user=quest_demo@northwestern.edu'

      Note the line ``--gres=gpu:a100:1`` which let’s you know that your
      have correctly requested for this job to run on a GPU resource.

      ::

      .. rubric:: INTERACTIVE JOBS
         :name: interactive-jobs

      To run an interactive pool job on the cluster, continue to use
      ``parpool`` as you’ve done before.

      ::

         >> % Get a handle to the cluster
         >> c = parcluster;

         >> % Open a pool of 12 workers on the cluster
         >> pool = c.parpool(12);

      In the screenshot below, when you create the pool object MATLAB
      will show you the arugments that it is passing to SLURM in order
      to make running the pool on the cluster.

      Rather than running local on the local machine, the pool can now
      run across multiple nodes on the cluster.

      ::

         >> % Run a parfor over 1000 iterations
         >> parfor idx = 1:1000
         a(idx) = …
         end

      Once we’re done with the pool, delete it.

      ::

         >> % Delete the pool
         >> pool.delete

      .. rubric:: DEBUGGING
         :name: debugging

      If a serial job produces an error, call the getDebugLog method to
      view the error log file. When submitting independent jobs, with
      multiple tasks, specify the task number.

      ``>> c.getDebugLog(job.Tasks(3))``

      For Pool jobs, only specify the job object.

      ``>> c.getDebugLog(job)``

      When troubleshooting a job, the cluster admin may request the
      scheduler ID of the job. This can be derived by calling schedID

      ::

         >> schedID(job)

         ans =

         25539

      .. rubric:: TO LEARN MORE
         :name: to-learn-more

      To learn more about the MATLAB Parallel Computing Toolbox, check
      out these resources:

      -  `Parallel Computing Coding
         Examples <https://www.mathworks.com/help/parallel-computing/examples.html>`__
      -  `Parallel Computing
         Documentation <https://www.mathworks.com/help/parallel-computing/index.html>`__
      -  `Parallel Computing
         Overview <https://www.mathworks.com/products/parallel-computing.html>`__
      -  `Parallel Computing
         Tutorials <https://www.mathworks.com/videos/series/parallel-and-gpu-computing-tutorials-97719.html>`__
      -  `Parallel Computing
         Videos <https://www.mathworks.com/videos/search.html?q=&fq=product:DM&page=1>`__
      -  `Parallel Computing
         Webinars <https://www.mathworks.com/videos/search.html?q=&fq%5B%5D=product:DM&fq%5B%5D=video-external-category:recwebinar&page=1>`__

.. |find-cluster-profile-manager| image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/FindClusterProfileManangerWindow.png
   :width: 599px
   :height: 394px
.. |clusterprofilemanager| image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/ClusterProfileManager.png
   :width: 609px
   :height: 398px
.. |Job1-Folder| image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Job1-Folder-Results.png
   :width: 547px
   :height: 389px
.. |task1-diary-txt| image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Task1_dairy_txt.png
   :width: 543px
   :height: 389px
.. |job-monitor-find| image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Another-Way-To-Monitor-Job.png
   :width: 628px
   :height: 411px
.. |job-monitor| image:: https://kb.northwestern.edu/images/group293/70716/matlab-cluster-2021b/Job-Monitor.png
   :width: 706px
   :height: 400px
