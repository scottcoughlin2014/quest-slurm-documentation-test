Quest Quick Start Guide
=======================

This page provides help getting started with connecting to, accessing
software on, and submitting jobs to the Quest computing cluster.

.. container::

   You will need to be logged in to Quest to get started. For further
   instructions, visit our `Logging into
   Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__
   KB page or watch our `Logging into Quest video
   tutorial <https://youtu.be/xFYHs19gGjU>`__.

See our `Introduction To
Quest <https://www.youtube.com/watch?v=rIFbHt_2g4s&feature=youtu.be>`__
video for a 90-minute interactive tutorial that takes you step-by-step
through logging into Quest, editing a job submission script, launching
and monitoring a job, as well as a deep dive into how Quest works.

Navigating Quest
----------------

If you’d prefer a video demonstration you can get started with basic
commands and a demonstration of navigating home and project directories
on Quest by watching our video tutorial\ `Navigating Quest via
Shell <https://www.youtube.com/watch?v=cLn_1BzRxCM>`__.

Once you’ve logged into Quest, you will begin in your Home directory:
``/home/<NetID>``. The home directory has 80GB of storage and is backed
up nightly. Backups are kept for two weeks.

Note that throughout this guide, items in angle brackets (``<>``) in
example commands are parameters you need to change relative to your
account and work. Do not include ``<>`` around the values you substitute
into the commands.

You can navigate to the projects directory of your allocation by typing

.. code:: code

   cd /projects/<project_id>

Project directories are not backed up. You can check how much storage
you have by typing

.. code:: code

   checkproject <project_id>

If you are unfamiliar with using the command line, this tutorial,
`Learning the
Shell <http://linuxcommand.org/lc3_learning_the_shell.php>`__, will help
you learn the basics.

Moving Files onto Quest
-----------------------

Please visit the `Transferring Files on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
web page for information about transferring files from your local
machine, `Northwestern
Box <https://kb.northwestern.edu/internal/search.php?q=box>`__,
`Research Data Storage
Service <https://nuinfo-proto9.northwestern.edu/departments/it-services-support/research/data-storage/index.html>`__,
or elsewhere, to Quest.

Software available on Quest
---------------------------

Many `applications are
available <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1547>`__
on Quest. You can access them through
`modules <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1550>`__,
which modify your environment to enable you to run the software. Quest
uses modules so that multiple versions of the same software can be
available for different users.

See `Modules Software Environment
Manager <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1550>`__
for examples of loading software with modules and a list of common
commands.

If you would like additional software packages installed on Quest,
please fill out the `Software Installation Request
Form <https://services.northwestern.edu/TDClient/30/Portal/Requests/ServiceOfferingDet?ID=66>`__.
Additionally, you can also install software in your own home or project
directories. For details, review our `Installing Software on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1742>`__
page.

Running Jobs on Quest
---------------------

When you first log in to Quest, you will be on one of the four login
nodes: ``quser21``, ``quser22``, ``quser23``, or ``quser24``. From the
login nodes, you can submit jobs to the compute nodes, edit files, and
perform small test jobs. Login node use is limited to 4 cores and 4 GB
of RAM per user.

To take advantage of Quest’s computing power, jobs can be submitted to
the compute nodes in two ways: Interactive jobs, which are particularly
useful for GUI applications, or Batch jobs, which are the most common
jobs on Quest. Interactive jobs are appropriate for GUI applications
like Stata, Visit, Paraview, etc., testing and prototyping of programs,
running jobs with small core counts (< 6), and for short duration jobs.
Batch jobs are appropriate for jobs with no GUI interface, large core
counts, or long duration.

See `Submitting a Job on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
for more details, example job submission commands, and an example of a
submission script.

Managing Your Jobs on Quest
---------------------------

.. container:: table-responsive

   +------------------------------+--------------------------------------+
   | Command                      | Description                          |
   +==============================+======================================+
   | ``squeue -u <NetID>``        | Shows your jobs on the queue         |
   +------------------------------+--------------------------------------+
   | ``squeue -A <AllocationID>`` | Shows your jobs belonging to         |
   |                              | specified allocation                 |
   +------------------------------+--------------------------------------+
   | ``checkjob <JobID>``         | Shows detailed information about     |
   |                              | specified job                        |
   +------------------------------+--------------------------------------+
   | ``scancel <JobID>``          | Cancels your job from the command    |
   |                              | line                                 |
   +------------------------------+--------------------------------------+
   | ``scancel -u <NetID>``       | Cancels all of your jobs from the    |
   |                              | command line                         |
   +------------------------------+--------------------------------------+

For more information about managing your jobs visit `Managing Jobs on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1544>`__.

Getting Help
------------

Learn more about using Quest from our
`documentation <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=505>`__
or attend one of our Introduction to Quest
`trainings <https://nuinfo-proto9.northwestern.edu/departments/it-services-support/research/research-events.html>`__.
If you need more personalized assistance, send an email stating your
issue to quest-help@northwestern.edu and the Northwestern IT Research
Computing Services team will assist you with your issue.
