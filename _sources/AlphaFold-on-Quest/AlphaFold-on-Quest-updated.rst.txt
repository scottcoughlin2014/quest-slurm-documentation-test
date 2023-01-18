AlphaFold on Quest
==================

How to run AlphaFold on Quest

Currently, we have AlphaFold version 2.0.0 and 2.1.1 (with multimers)
installed on Quest. For more details on both releases of AlphaFold,
please visit the `AlphaFold
website <https://github.com/deepmind/alphafold>`__.

-  `AlphaFold 2.1.1 with Separate CPU and GPU workloads
   (Preferred) <#section-one>`__
-  `AlphaFold 2.1.1 <#section-two>`__
-  `AlphaFold 2.0.0 <#section-three>`__
   ` <#section-two>`__\ ` <#section-two>`__

Expand All Collapse All

.. container:: panel panel-default

   .. container:: panel-heading

      Running AlphaFold 2.1.1 with Separate CPU and GPU workloads
      (Preferred)

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      .. rubric:: How is AlphaFold 2.1.1 with Separate CPU and GPU
         Workloads Installed On Quest?
         :name: how-is-alphafold-2.1.1-with-separate-cpu-and-gpu-workloads-installed-on-quest

      AlphaFold 2.1.1 is installed inside of a Singularity container
      following the `instructions from the DeepMind
      team <https://github.com/deepmind/alphafold/blob/c9ffb0bc187077602c1940269612fa8df1674fbf/docker/Dockerfile>`__.

      The container contains CUDA 11.1, Python 3.7.11, TensorFlow 2.5.0,
      jax 0.2.25, and jaxlib 0.1.69+cuda111. *In addition, please note
      that this install of AlphaFold contains a modification to
      AlphaFold in order to allow for the CPU and GPU parts of AlphaFold
      to be run separately.* We added the following flag

      ``+flags.DEFINE_boolean('only_msas', False, 'Whether to only build MSAs, and not do any prediction.')``
      Instead of calling singularity directly, we provide a module which
      wraps the call to the ``singularity run``.

      ``module load alphafold/2.1.1-only-msas-flag-addition``
      This creates a\ **two** shell functions, one for running Alphafold
      multimer (``alphafold-multimer``), and one for use with Alpha
      monomer (``alphafold-monomer``).

      .. code:: code

         alphafold-monomer --fasta_paths=/full/path/to/fasta \
           --output_dir=/full/path/to/outdir \
           --max_template_date= \
           --only_msas=[true|false] \
           --use_precomputed_msas=[true|false] \
           --model_preset=[monomer|monomer_casp14|monomer_ptm] \
           --db_preset=full_dbs \

      -  **model_preset**

         -  **monomer**: This is the original model used at CASP14 with
            no ensembling.
         -  **monomer_casp14**: This is the original model used at
            CASP14 with num_ensemble=8, matching our CASP14
            configuration. This is largely provided for reproducibility
            as it is 8x more computationally expensive for limited
            accuracy gain (+0.1 average GDT gain on CASP14 domains).
         -  **monomer_ptm**: This is the original CASP14 model fine
            tuned with the pTM head, providing a pairwise confidence
            measure. It is slightly less accurate than the normal
            monomer model.

      -  **use_precomputed_msas**

         -  Whether to read MSAs that have been written to disk.

      -  **only_msas**

         -  Whether to only build MSAs, and not do any prediction.

      .. code:: code

         alphafold-multimer --fasta_paths=/full/path/to/fasta \
           --output_dir=/full/path/to/outdir \
           --max_template_date= \
           --only_msas=[true|false] \
            --use_precomputed_msas=[true|false] \
           --model_preset=multimer \
           --db_preset=full_dbs \
           --is_prokaryote_list=[true|false] 

      -  **model_preset**

         -  **multimer**: This is the AlphaFold-Multimer model. To use
            this model, provide a multi-sequence FASTA file. In
            addition, the UniProt database should have been downloaded.

      -  **is_prokaryote_list**

         -  optionally set the –is_prokaryote_list flag with booleans
            that determine whether all input sequences in the given
            fasta file are prokaryotic. If that is not the case or the
            origin is unknown, set to false for that fasta.

      -  **use_precomputed_msas**

         -  Whether to read MSAs that have been written to disk.

      -  **only_msas**

         -  Whether to only build MSAs, and not do any prediction.

      If you would like to see the contents of the shell function
      ``alphafold-multimer`` or ``alphafold-monomer``, you can run
      ``type``\ ``alphafold-monome``\ r or ``type alphafold-multimer``
      on the command line.

      .. rubric:: How do you run AlphaFold with separate CPU and GPU
         workloads on Quest?
         :name: how-do-you-run-alphafold-with-separate-cpu-and-gpu-workloads-on-quest

      Below, we provide an example submission script for running
      AlphaFold with separate CPU and GPU workloads on Quest. First, you
      construct the submission script that will use only CPU resources,
      which we will call ``example_submit_cpu_part.sh``.

      ::

         #!/bin/bash
         #SBATCH --account=pXXXX ## YOUR ACCOUNT pXXXX or bXXXX
         #SBATCH --partition=short ### PARTITION (buyin, short, normal, etc)
         #SBATCH --nodes=1 ## how many computers do you need
         #SBATCH --ntasks-per-node=12 ## how many cpus or processors do you need on each computer
         #SBATCH --time=04:00:00 ## how long does this need to run (remember different partitions have restrictions on this param)
         #SBATCH --mem=85G ## how much RAM do you need per CPU (this effects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --job-name=run_AlphaFold ## When you run squeue -u NETID this is how you can identify the job
         #SBATCH --output=AlphaFold-CPU.log ## standard out and standard error goes to this file

         #########################################################################
         ### PLEASE NOTE:                           ###
         ### The above CPU and Memory resources have been selected based    ###
         ### on the computing resources that alphafold was tested on      ###
         ### which can be found here:                     ###
         ### https://github.com/deepmind/alphafold#running-alphafold)     ###
         ### It is likely that you do not have to change anything above    ###
         ### besides your allocation, and email (if you want to be emailed).  ###
         #########################################################################

         module purge
         module load alphafold/2.1.1-only-msas-flag-addition

         # To run alphafold more efficiently,
         # we split the CPU and GPU parts of the pipeline into two separate submissions.
         # Below we provide a way to run the CPU part of alpahfold-multimer and alphafold-monomer

         # real example monomer (takes about 3 hours and 15 minutes)
         alphafold-monomer --fasta_paths=/projects/intro/alphafold/T1050.fasta \
           --max_template_date=2020-05-14 \
           --model_preset=monomer \
           --db_preset=full_dbs \
           --only_msas=true \
           --output_dir=$(pwd)/out

         # real example multimer (takes about 2 hours and 40 minutes)
         alphafold-multimer --fasta_paths=/projects/intro/alphafold/6E3K.fasta \
           --max_template_date=2020-05-14 \
           --model_preset=multimer \
           --db_preset=full_dbs \
           --only_msas=true \
           --output_dir=$(pwd)/out

      Next, you construct the submission script that will use GPU
      resources, which we will call ``example_submit_gpu_part.sh``.

      ::

         #!/bin/bash
         #SBATCH --account=pXXXX ## YOUR ACCOUNT pXXXX or bXXXX
         #SBATCH --partition=gengpu
         #SBATCH --nodes=1 ## how many computers do you need
         #SBATCH --ntasks-per-node=1 ## how many cpus or processors do you need on each computer
         #SBATCH --gres=gpu:a100:1 ## type of GPU requested, and number of GPU cards to run on
         #SBATCH --time=04:00:00 ## how long does this need to run (remember different partitions have restrictions on this param)
         #SBATCH --mem=85G ## how much RAM do you need per CPU (this effects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --job-name=run_AlphaFold ## When you run squeue -u NETID this is how you can identify the job
         #SBATCH --output=AlphaFold-GPU.log ## standard out and standard error goes to this file

         #########################################################################
         ### PLEASE NOTE:                           ###
         ### The above CPU, Memory, and GPU resources have been selected based ###
         ### on the computing resources that alphafold was tested on      ###
         ### which can be found here:                     ###
         ### https://github.com/deepmind/alphafold#running-alphafold)     ###
         ### It is likely that you do not have to change anything above    ###
         ### besides your allocation, and email (if you want to be emailed).  ###
         #########################################################################

         module purge
         module load alphafold/2.1.1-only-msas-flag-addition

         # To run alphafold more efficiently,
         # we split the CPU and GPU parts of the pipeline into two separate submissions.
         # Below we provide a way to run the GPU part of alpahfold-multimer and alphafold-monomer
         # which will depend on the CPU part finishing before it runs

         # real example monomer (takes about 3 hours and 15 minutes)
         alphafold-monomer --fasta_paths=/projects/intro/alphafold/T1050.fasta \
           --max_template_date=2020-05-14 \
           --model_preset=monomer \
           --db_preset=full_dbs \
           --use_precomputed_msas=true \
           --output_dir=$(pwd)/out

         # real example multimer (takes about 2 hours and 40 minutes)
         alphafold-multimer --fasta_paths=/projects/intro/alphafold/6E3K.fasta \
           --max_template_date=2020-05-14 \
           --model_preset=multimer \
           --db_preset=full_dbs \
           --use_precomputed_msas=true \
           --output_dir=$(pwd)/out

      Finally, we use the following bash script which we will call
      ``submit_alphafold_workflow.sh`` to submit the CPU job first, and
      then submit the GPU job as dependent on the CPU job finishing with
      status OK.

      ::

         #!/bin/bash
         cpu_job=($(sbatch example_submit_cpu_part.sh))
         echo "cpu_job ${cpu_job[-1]}" >> slurm_ids
         gpu_job=($(sbatch --dependency=afterok:${cpu_job[-1]} example_submit_gpu_part.sh))
         echo "gpu_job ${gpu_job[-1]}" >> slurm_ids

.. container:: panel panel-default

   .. container:: panel-heading

      AlphaFold 2.1.1

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      .. rubric:: How is AlphaFold 2.1.1 Installed On Quest?
         :name: how-is-alphafold-2.1.1-installed-on-quest

      AlphaFold 2.1.1 is installed inside of a Singularity container
      following the `instructions from the DeepMind
      team <https://github.com/deepmind/alphafold/blob/c9ffb0bc187077602c1940269612fa8df1674fbf/docker/Dockerfile>`__.

      The container contains CUDA 11.0, Python 3.7.10, and TensorFlow
      2.5.0.

      Instead of calling singularity directly, we provide a module which
      wraps the call to the ``singularity run``.

      ``module load alphafold/2.1.1``

      This creates a\ **two** shell functions, one for running Alphafold
      multimer (``alphafold-multimer``), and one for use with Alpha
      monomer (``alphafold-monomer``).

      .. code:: code

         alphafold-monomer --fasta_paths=/full/path/to/fasta \
           --output_dir=/full/path/to/outdir \
           --max_template_date= \
           --model_preset=[monomer|monomer_casp14|monomer_ptm] \
           --db_preset=full_dbs \

      -  **model_preset**

         -  **monomer**: This is the original model used at CASP14 with
            no ensembling.
         -  **monomer_casp14**: This is the original model used at
            CASP14 with num_ensemble=8, matching our CASP14
            configuration. This is largely provided for reproducibility
            as it is 8x more computationally expensive for limited
            accuracy gain (+0.1 average GDT gain on CASP14 domains).
         -  **monomer_ptm**: This is the original CASP14 model fine
            tuned with the pTM head, providing a pairwise confidence
            measure. It is slightly less accurate than the normal
            monomer model.

      .. code:: code

         alphafold-multimer --fasta_paths=/full/path/to/fasta \
           --output_dir=/full/path/to/outdir \
           --max_template_date= \
           --model_preset=multimer \
           --db_preset=full_dbs \
           --is_prokaryote_list=[true|false] 

      -  **model_preset**

         -  **multimer**: This is the AlphaFold-Multimer model. To use
            this model, provide a multi-sequence FASTA file. In
            addition, the UniProt database should have been downloaded.

      -  **is_prokaryote_list**

         -  optionally set the –is_prokaryote_list flag with booleans
            that determine whether all input sequences in the given
            fasta file are prokaryotic. If that is not the case or the
            origin is unknown, set to false for that fasta.

      If you would like to see the contents of the shell function
      ``alphafold``, you can run ``type alphafold`` on the command line.

      .. rubric:: How do you run AlphaFold on Quest?
         :name: how-do-you-run-alphafold-on-quest

      Below, we provide an example submission script for running
      AlphaFold on Quest.

      .. code:: code

         #!/bin/bash
         #SBATCH --account=pXXXX  ## YOUR ACCOUNT pXXXX or bXXXX
         #SBATCH --partition=gengpu  ### PARTITION (buyin, short, normal, etc)
         #SBATCH --nodes=1 ## how many computers do you need - for AlphaFold this should always be one
         #SBATCH --ntasks-per-node=12 ## how many cpus or processors do you need on each computer
         #SBATCH --gres=gpu:a100:1  ## type of GPU requested, and number of GPU cards to run on
         #SBATCH --time=48:00:00 ## how long does this need to run 
         #SBATCH --mem=85G ## how much RAM do you need per node (this effects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --job-name=run_AlphaFold  ## When you run squeue -u <NETID> this is how you can identify the job
         #SBATCH --output=AlphaFold.log ## standard out and standard error goes to this file
         #SBATCH --mail-type=ALL ## you can receive e-mail alerts from SLURM when your job begins and when your job finishes (completed, failed, etc)
         #SBATCH --mail-user=email@northwestern.edu ## your email, non-Northwestern email addresses may not be supported

         #########################################################################
         ### PLEASE NOTE:                                                      ###
         ### The above CPU, Memory, and GPU resources have been selected based ###
         ### on the computing resources that alphafold was tested on           ###
         ### which can be found here:                                          ###
         ### https://github.com/deepmind/alphafold#running-alphafold)          ###
         ### It is likely that you do not have to change anything above        ###
         ### besides your allocation, and email (if you want to be emailed).   ###
         ######################################################################### module purge

         module purge
         module load alphafold/2.1.1

         # template monomer
         # alphafold-monomer --fasta_paths=/full/path/to/fasta \
         # --output_dir=/full/path/to/outdir \
         # --max_template_date= \
         # --model_preset=[monomer|monomer_casp14|monomer_ptm] \
         # --db_preset=full_dbs
         ### 
         ### monomer: This is the original model used at CASP14 with no ensembling.
         ### 
         ### monomer_casp14: This is the original model used at CASP14 with num_ensemble=8, matching our CASP14 configuration. This is largely provided for reproducibility as it is 8x more computationally expensive for limited accuracy gain (+0.1 average GDT gain on CASP14 domains).
         ### 
         ### monomer_ptm: This is the original CASP14 model fine tuned with the pTM head, providing a pairwise confidence measure. It is slightly less accurate than the normal monomer model.

         # template multimer
         # alphafold-multimer --fasta_paths=/full/path/to/fasta \
         # --output_dir=/full/path/to/outdir \
         # --max_template_date= \
         # --model_preset=multimer \
         # --is_prokaryote_list=[true|false] \
         # --db_preset=full_dbs
         ### 
         ### multimer: This is the AlphaFold-Multimer model. To use this model, provide a multi-sequence FASTA file. In addition, the UniProt database should have been downloaded.
         ###
         ### optionally set the --is_prokaryote_list flag with booleans that determine whether all input sequences in the given fasta file are prokaryotic. If that is not the case or the origin is unknown, set to false for that fasta.

         # real example monomer (takes about 3 hours and 15 minutes)
         alphafold-monomer --fasta_paths=/projects/intro/alphafold/T1050.fasta \
          --max_template_date=2020-05-14 \
          --model_preset=monomer \
          --db_preset=full_dbs \
          --output_dir=$(pwd)/out

         # real example multimer (takes about 2 hours and 40 minutes)
         alphafold-multimer --fasta_paths=/projects/intro/alphafold/6E3K.fasta \
          --max_template_date=2020-05-14 \
          --model_preset=multimer \
          --db_preset=full_dbs \
          --output_dir=$(pwd)/out

.. container:: panel panel-default

   .. container:: panel-heading

      AlphaFold 2.0.0

   .. container:: panel panel-body js-panelnormalswitches2 collapse

      .. rubric:: How is AlphaFold 2.0.0 Installed On Quest?
         :name: how-is-alphafold-2.0.0-installed-on-quest

      AlphaFold 2.0.0 is installed inside of a Singularity container
      following the `instructions from the DeepMind
      team <https://github.com/deepmind/alphafold/blob/c9ffb0bc187077602c1940269612fa8df1674fbf/docker/Dockerfile>`__.

      The container contains CUDA 11.0, Python 3.7.10, and TensorFlow
      2.5.0.

      Instead of calling singularity directly, we provide a module which
      wraps the call to the ``singularity run``.

      ``module load alphafold/2.0.0``

      This creates a shell function called ``alphafold`` which can be
      used as follows:

      .. code:: code

         alphafold --fasta_paths=/full/path/to/fasta \
         --output_dir=/full/path/to/outdir \
         --model_names= \
         --preset=[full_dbs|casp14] \
         --max_template_date=

      If you would like to see the contents of the shell function
      ``alphafold``, you can run ``type alphafold`` on the command line.

      .. rubric:: How do you run AlphaFold on Quest?
         :name: how-do-you-run-alphafold-on-quest-1

      Below, we provide an example submission script for running
      AlphaFold on Quest.

      .. code:: code

         #!/bin/bash
         #SBATCH --account=pXXXX  ## YOUR ACCOUNT pXXXX or bXXXX
         #SBATCH --partition=gengpu  ### PARTITION (buyin, short, normal, etc)
         #SBATCH --nodes=1 ## how many computers do you need - for AlphaFold this should always be one
         #SBATCH --ntasks-per-node=12 ## how many cpus or processors do you need on each computer
         #SBATCH --gres=gpu:a100:1  ## type of GPU requested, and number of GPU cards to run on
         #SBATCH --time=48:00:00 ## how long does this need to run 
         #SBATCH --mem=85G ## how much RAM do you need per node (this effects your FairShare score so be careful to not ask for more than you need))
         #SBATCH --job-name=run_AlphaFold  ## When you run squeue -u <NETID> this is how you can identify the job
         #SBATCH --output=AlphaFold.log ## standard out and standard error goes to this file
         #SBATCH --mail-type=ALL ## you can receive e-mail alerts from SLURM when your job begins and when your job finishes (completed, failed, etc)
         #SBATCH --mail-user=email@northwestern.edu ## your email, non-Northwestern email addresses may not be supported

         #########################################################################
         ### PLEASE NOTE:                                                      ###
         ### The above CPU, Memory, and GPU resources have been selected based ###
         ### on the computing resources that alphafold was tested on           ###
         ### which can be found here:                                          ###
         ### https://github.com/deepmind/alphafold#running-alphafold)          ###
         ### It is likely that you do not have to change anything above        ###
         ### besides your allocation, and email (if you want to be emailed).   ###
         #########################################################################

         module purge 
         module load alphafold/2.0.0

         # template
         # alphafold --fasta_paths=/full/path/to/fasta \
         #    --output_dir=/full/path/to/outdir \
         #    --model_names= \
         #    --preset=[full_dbs|casp14] \
         #    --max_template_date=

         # real example
         alphafold --output_dir $HOME/alphafold --fasta_paths=/projects/intro/alphafold/T1050.fasta --max_template_date=2021-07-28 --model_names model_1,model_2,model_3,model_4,model_5 --preset casp14
