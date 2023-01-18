Running GATK4 Spark on Quest
============================

How to run Genome Analysis Toolkit’s Spark tools on Quest.

The Broad Institute’s Genome Analysis Toolkit (GATK) is a widely used
best practices pipeline for whole genome sequencing and variant calling.
As of GATK version 4, many GATK tools are also available to run on
Apache Spark, a unified analytics engine for large-scale data processing
which can significantly speed up computation time. Note that GATK4’s
Spark tools are currently in beta. Because GATK4 SPARK runs on multiple
nodes, it must be launched with a job submission script and cannot be
run on a login node.

GATK4 SPARK tools
~~~~~~~~~~~~~~~~~

To see a list of available GATK4 tools:

.. container:: code

   module load gatk/4.0.4

   gatk –-list

GATK Spark tools have the word “Spark” in their name, and can be
explicitly listed with grep:

.. container:: code

   gatk –-list \| grep Spark

Additional help is available for each tool with:

.. container:: code

   gatk ToolName –help

Converting FASTA Files for Parallel Runs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Standard FASTA files do not allow for parallel operation and must be
converted to 2bit files. To convert these files, use the binary in
$SPARK_TOOLS which is in the SPARK module path.

.. container:: code

   module load spark/2.3.0

   faToTwoBit exampleFASTA.fasta exampleFASTA.2bit

This step only needs to be done once.

Example Job Submission
~~~~~~~~~~~~~~~~~~~~~~

Below is an example of job submission file for Quest using a converted
exampleFASTA.2bit file.

| 

.. code:: filenameheader

   GATKSpark_example.sh

::

   #!/bin/bash
   #SBATCH -A <allocation>      # Allocation 
   #SBATCH -p <partition_name>  # Partition 

::

   #SBATCH -t 00:20:00          # Walltime/duration of job

::

   #SBATCH -N 2                 # Number of nodes 

::

   #SBATCH --ntasks-per-node=24 # Number of cores (processors)
   #SBATCH --mem-per-cpu=5G     # GB needed per-core for a job
   #SBATCH -J "SparkTest"       # Name of job

   # Load environment

::

   module purge all
   module load spark/2.3.0 gatk/4.0.4

::

::

   cd $SLURM_SUBMIT_DIR

   # Initialize spark cluster on hosts allocated to your job
   $SPARK_TOOLS/initialize_spark.sh

   # Run GATK HaplotypeCaller in Spark
   gatk HaplotypeCallerSpark \

::

   --reference $SLURM_SUBMIT_DIR/exampleFASTA.2bit \
   --input $SLURM_SUBMIT_DIR/exampleBAM.bam \

::

   --output $SLURM_SUBMIT_DIR/exampleVCF.txt \
   --spark-runner SPARK \
   --spark-master spark://`hostname -i`:7077 \

::

   -- --driver-cores=2 --driver-memory=6g \

::

   --executor-cores=22 --executor-memory=114GB 2>&1

::

::

   # Cleanup spark cluster on hosts allocated to your job

::

   $SPARK_TOOLS/cleanup_spark.sh
