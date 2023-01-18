Quest FAQ
=========

This page contains frequently asked questions about the Quest high
performance computing cluster at Northwestern.

.. container:: expander expander1

   **Allocations And Accounts**

   .. container::

      .. rubric:: How do I get an account on Quest?
         :name: how-do-i-get-an-account-on-quest

      To access Quest, you will need to be part of an active research
      allocation. There are two ways to obtain one:

      -  Submit a new allocation request form for either a Research I or
         Research II allocation.
      -  Join an existing research allocation.

      The application forms can be found at `Request Research Allocation
      Forms <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__

      The Research Allocation I is suitable for projects requiring
      35,000 compute hours or less. This allocation provides a 1 TB
      project directory. The resources provided by Research I will fit
      the needs of the majority of our users. A Research II allocation
      is suitable for projects requiring up to 500,000 compute hours.
      This allocation provides a 2 TB project storage. Research II
      allocations are for projects with a large computational need,
      however you will need to submit a detailed proposal. Both Research
      I and II allocations are available free of charge, but we do
      request that a chartstring is provided for your research if
      possible. This chartstring would be used for internal tracking
      purposes so that we can see the impact Quest is having on research
      done on campus.

      .. rubric:: Am I currently in an active allocation on Quest?
         :name: am-i-currently-in-an-active-allocation-on-quest

      To see which allocations you belong to on Quest, `login to
      Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__
      and run the ``groups`` command:
      .. code:: code

         $ groups
         p31014 p31182 w10001

      Note that this list includes all allocations you have belonged to
      on Quest, some of which may no longer be active. To check the
      status of an allocation you belong to, use the ``checkproject``
      command:

      .. code:: code

         $ checkproject p12345
         ====================================

         Reporting for project p12345
         ------------------------------------
         768 GB in 141942 files (76.80% of 1000 GB quota)
         Allocation Type: Allocation I
         Expiration Date: 2023-03-01
         Status: ACTIVE
         Compute and storage allocation - when status is ACTIVE, this allocation has compute node access and can submit jobs
         ------------------------------------

         ====================================

      The “Status” of your allocation will either be “ACTIVE” or
      “EXPIRED”, and you may apply to renew expired allocations using
      the `form appropriate for your allocation
      type <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__.
      In addition, ``checkproject`` displays the percentage of storage
      currently used in your allocation, the date when this allocation
      expires, and if this allocation can submit jobs or is storage
      only.

      .. rubric:: How do I retain access to Quest after I leave the
         University?
         :name: how-do-i-retain-access-to-quest-after-i-leave-the-university

      As long as you have a valid Northwestern NetID/password, you can
      access to Quest. When your NetID is deactivated by the University,
      your Quest access will also end. If you want to continue using
      Quest you should obtain a guest or affiliate NetID. There are
      individuals in departments/schools who can request guest NetIDs.
      You or your supervisor can reach out to them.

      Please review the following documentation for more information:
      `Affiliate, Departmental, and Guest
      NetIDs <https://services.northwestern.edu/TDClient/30/Portal/Requests/ServiceOfferingDet?ID=26>`__.

   .. rubric:: Resources
      :name: resources

   .. container::

      .. rubric:: How do I use GPUs on Quest?
         :name: how-do-i-use-gpus-on-quest

      There are two ways to use GPUs on Quest. You can either purchase
      your own GPU nodes or use a general access allocation (i.e.
      Research I, Research II, and Education) to access shared GPU
      nodes. Please see `GPUs on
      Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1112>`__
      for more information.

   .. rubric:: Data Transfer
      :name: data-transfer

   .. container::

      .. rubric:: How do I transfer files to and from
         OneDrive/Sharepoint on Quest?
         :name: how-do-i-transfer-files-to-and-from-onedrivesharepoint-on-quest

      An option for transferring Quest storage and OneDrive would be to
      connect to Quest with the FastX client and start a Gnome desktop
      session or a Gnome terminal session. Then launch a terminal (or
      use the terminal that is already launched, if you pick a Gnome
      terminal session) and type

      .. code:: code

         firefox

      to launch the firefox browser. In that browser you can log into
      OneDrive and transfer files.
      Documentation on how to use FastX to connect to Quest is available
      here: `Connecting to Quest with
      FastX <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1511>`__.
      We recommend that for large data transfers or repeated transfers
      to use the `Globus OneDrive
      connector <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
      when it becomes available.

      .. rubric:: How do I use Amazon AWS CLI on Quest?
         :name: how-do-i-use-amazon-aws-cli-on-quest

      The Amazon CLI is a python package install and is installed system
      wide on Quest. To load this package, run:

      .. code:: code

         module load awscli/2

      .. rubric:: How do I use the Google Cloud SDK on Quest?
         :name: how-do-i-use-the-google-cloud-sdk-on-quest

      Please see `Google Cloud
      SDK <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
      for more information.
      .. rubric:: How do I get access to the Globus endpoint for RDSS
         (RESFILES/FSMRESFILES)?
         :name: how-do-i-get-access-to-the-globus-endpoint-for-rdss-resfilesfsmresfiles

      To be able to use Globus to transfer data to and from your RDSS
      (also known as RESFILES or FSMRESFILES), open a service request by
      emailing quest-help@northwestern.edu.

      .. rubric:: How do I transfer files to and from RDSS
         (RESFILES/FSMRESFILES) via Globus?
         :name: how-do-i-transfer-files-to-and-from-rdss-resfilesfsmresfiles-via-globus

      Please see `Transferring Data To and From Quest and Research Data
      Storage Service or
      FSMRESFILES <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
      for more information.
      .. rubric:: How do I share the data on Quest with my collaborators
         outside Northwestern?
         :name: how-do-i-share-the-data-on-quest-with-my-collaborators-outside-northwestern

      You can use Globus to share data with external collaborators who
      do not have Northwestern affiliation. Please follow the
      instructions here: `Sharing files with
      Globus <https://docs.globus.org/how-to/share-files/>`__. Login to
      `Globus <https://www.globus.org>`__ with Northwestern affiliation.
      The collection name (i.e. the endpoint) you should select is
      “Northwestern Quest” to access your files/folders on Quest. You
      can share data with collaborators who have personal Globus IDs or
      accounts through their institutions’ subscription.

   .. rubric:: Scheduler and Job Submissions
      :name: scheduler-and-job-submissions

   .. container::

      .. rubric:: I get the error “Unable to allocate resources:
         Requested time limit is invalid (missing or exceeds some
         limit)” when trying to submit a job.
         :name: i-get-the-error-unable-to-allocate-resources-requested-time-limit-is-invalid-missing-or-exceeds-some-limit-when-trying-to-submit-a-job.

      | This error indicates that you have specified your job to run for
        longer than a given queue will allow. To allow this job to run,
        you will need to either reduce the amount of walltime for the
        job to be within the selected queue’s limits, or define a larger
        queue with a higher walltime.
      | You can find a list of all queues and their walltime limits at
        `Quest
        Partitions/Queues <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1549>`__.

      .. rubric:: My job was killed on a login node.
         :name: my-job-was-killed-on-a-login-node.

      From time to time, we encounter errors on the login nodes that
      require killing all running jobs on that node to prevent the node
      from crashing. Unfortunately, your job may have been one of those
      that were killed.

      It is recommended that users submit interactive or batch jobs to
      the compute nodes to avoid such job cancellations. Login nodes are
      shared resources are intended as entry points to Quest for all
      users. Submitting CPU or memory heavy jobs will affect everyone
      trying to access Quest. Please see `Submitting a Job On
      Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
      for more information about submitting interactive jobs.

      .. rubric:: I get the error “sbatch: error: Batch job submission
         failed: Invalid qos specification” when trying to submit a job.
         :name: i-get-the-error-sbatch-error-batch-job-submission-failed-invalid-qos-specification-when-trying-to-submit-a-job.

      This error is commonly observed if your allocation has expired.
      Slurm does not allow job submission if you are using an expired
      allocation. You can run the command
      ``checkproject <allocation ID>`` to see the expiration date of
      your allocation. If your research project continues and you want
      to continue using the same allocation, you will need to renew it
      by `Requesting a Research
      Allocation <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__

   .. rubric:: Software
      :name: software

   .. container::

      .. rubric:: The software I want to use is not available on Quest.
         :name: the-software-i-want-to-use-is-not-available-on-quest.

      If the software you require for your research is not available on
      Quest, there are a few options you can try. The first is to
      perform a local software installation to your home directory or
      project directory following instructions specific to the software
      you are trying to install. If a local install does not work, you
      can fill out the `Software Installation Request
      Form. <https://services.northwestern.edu/TDClient/30/Portal/Requests/ServiceOfferingDet?ID=66>`__
      We will then assist you with the software installation.

      .. rubric:: How can I install and use Tensorflow with GPUs on
         Quest?
         :name: how-can-i-install-and-use-tensorflow-with-gpus-on-quest

      Please see the **What GPU software is available on QUEST?**
      section of `GPUs on
      Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1112>`__
      for more information.\ **
      **
