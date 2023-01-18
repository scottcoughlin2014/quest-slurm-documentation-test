File Systems, Storage, and Permissions on Quest
===============================================

Tips on managing storage and file permissions on Quest.

For further information on storage limits and access expiration, please
refer to the `Quest Storage and Data
Policy <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/storage-data-policy.html>`__.

Quest has three locations where you may store files: ``/home/<netid>``,
``/projects/<allocationID>``, and if you make a request following the
instructions
`here <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-storage.html>`__,
``/scratch/<netid>``. These locations serve different purposes, have
different disk quotas, and have different behavior when it comes to
permissions. We summarize these in the form of a table up top, before
going into more details below.

.. container:: table-responsive

   +--------+--------+--------+--------+--------+--------+--------+--------+
   | **S    | *      | **What | **What | **How  | **     | **How  | **How  |
   | torage | *Where | should | are    | much   | Number | do you | do I   |
   | Name** | is     | go     | the    | disk   | of     | get    | check  |
   |        | this   | h      | d      | sp     | Files? | a      | disk   |
   |        | l      | ere?** | efault | ace?** | **     | ccess? | space  |
   |        | ocated |        | pe     |        |        | **     | ut     |
   |        | on     |        | rmissi |        |        |        | ilizat |
   |        | Qu     |        | ons?** |        |        |        | ion?** |
   |        | est?** |        |        |        |        |        |        |
   +========+========+========+========+========+========+========+========+
   | `Home  | ``/ho  | Subm   | Only   | 80GB   | *Unli  | Your   | ``ho   |
   | Dire   | me/<ne | ission | the    |        | mited* | home   | medu`` |
   | ctorie | tid>`` | sc     | user   |        |        | dir    |        |
   | s <#ho |        | ripts, | has    |        |        | ectory |        |
   | me>`__ |        | job    | read,  |        |        | is     |        |
   |        |        | log    | write  |        |        | a      |        |
   |        |        | files, | and    |        |        | utomat |        |
   |        |        | and    | e      |        |        | ically |        |
   |        |        | local  | xecute |        |        | c      |        |
   |        |        | p      | permis |        |        | reated |        |
   |        |        | ackage | sions. |        |        | the    |        |
   |        |        | and    |        |        |        | first  |        |
   |        |        | so     |        |        |        | time   |        |
   |        |        | ftware |        |        |        | you    |        |
   |        |        | i      |        |        |        | are    |        |
   |        |        | nstall |        |        |        | given  |        |
   |        |        | ations |        |        |        | access |        |
   |        |        |        |        |        |        | to     |        |
   |        |        |        |        |        |        | Quest  |        |
   |        |        |        |        |        |        | via    |        |
   |        |        |        |        |        |        | j      |        |
   |        |        |        |        |        |        | oining |        |
   |        |        |        |        |        |        | an     |        |
   |        |        |        |        |        |        | ex     |        |
   |        |        |        |        |        |        | isting |        |
   |        |        |        |        |        |        | re     |        |
   |        |        |        |        |        |        | search |        |
   |        |        |        |        |        |        | allo   |        |
   |        |        |        |        |        |        | cation |        |
   |        |        |        |        |        |        | or     |        |
   |        |        |        |        |        |        | cr     |        |
   |        |        |        |        |        |        | eating |        |
   |        |        |        |        |        |        | a new  |        |
   |        |        |        |        |        |        | one.   |        |
   +--------+--------+--------+--------+--------+--------+--------+--------+
   | `Allo  | ``/pr  | High   | All    | 1TB    | *Unli  | Join   | ``     |
   | cation | ojects | -speed | m      | with a | mited* | an     | checkp |
   | or     | /<allo | s      | embers | Re     |        | ex     | roject |
   | P      | cation | torage | of the | search |        | isting |  <allo |
   | roject | _id>`` | which  | allo   | Allo   |        | re     | cation |
   | Di     |        | should | cation | cation |        | search | _id>`` |
   | rector |        | be     | will   | I and  |        | allo   |        |
   | ies <# |        | used   | have   | 2 TB   |        | cation |        |
   | projec |        | for    | read,  | with a |        | or     |        |
   | ts>`__ |        | compu  | write  | Re     |        | create |        |
   |        |        | tation | and/or | search |        | a new  |        |
   |        |        | Input/ | e      | Allo   |        | one to |        |
   |        |        | Output | xecute | cation |        | get    |        |
   |        |        | (IO)   | permi  | II.    |        | a      |        |
   |        |        | and/or | ssions |        |        | ccess. |        |
   |        |        | data   | on     | buyin  |        |        |        |
   |        |        | ana    | files  | alloc  |        |        |        |
   |        |        | lyses. | and    | ations |        |        |        |
   |        |        |        | direc  | can    |        |        |        |
   |        |        |        | tories | `pu    |        |        |        |
   |        |        |        | c      | rchase |        |        |        |
   |        |        |        | reated | s      |        |        |        |
   |        |        |        | in the | torage |        |        |        |
   |        |        |        | allo   | by the |        |        |        |
   |        |        |        | cation | TB <   |        |        |        |
   |        |        |        | dire   | https: |        |        |        |
   |        |        |        | ctory. | //it.n |        |        |        |
   |        |        |        |        | orthwe |        |        |        |
   |        |        |        |        | stern. |        |        |        |
   |        |        |        |        | edu/de |        |        |        |
   |        |        |        |        | partme |        |        |        |
   |        |        |        |        | nts/it |        |        |        |
   |        |        |        |        | -servi |        |        |        |
   |        |        |        |        | ces-su |        |        |        |
   |        |        |        |        | pport/ |        |        |        |
   |        |        |        |        | resear |        |        |        |
   |        |        |        |        | ch/com |        |        |        |
   |        |        |        |        | puting |        |        |        |
   |        |        |        |        | /quest |        |        |        |
   |        |        |        |        | /quest |        |        |        |
   |        |        |        |        | -stora |        |        |        |
   |        |        |        |        | ge.htm |        |        |        |
   |        |        |        |        | l>`__. |        |        |        |
   +--------+--------+--------+--------+--------+--------+--------+--------+
   | `S     | ``     | High   | Only   | 5TB    | *5     | S      | ``ch   |
   | cratch | /scrat | -speed | the    |        | M      | cratch | eckscr |
   | Space  | ch/<ne | s      | user   |        | illion | is     | atch`` |
   | D      | tid>`` | torage | has    |        | *      | unique |        |
   | irecto |        | which  | read,  |        |        | in     |        |
   | ries < |        | should | write  |        |        | that   |        |
   | #scrat |        | be     | and    |        |        | you do |        |
   | ch>`__ |        | used   | e      |        |        | not    |        |
   |        |        | for    | xecute |        |        | have   |        |
   |        |        | s      | permis |        |        | access |        |
   |        |        | toring | sions. |        |        | to it  |        |
   |        |        | tem    |        |        |        | be     |        |
   |        |        | porary |        |        |        | d      |        |
   |        |        | files  |        |        |        | efault |        |
   |        |        | from   |        |        |        | after  |        |
   |        |        | r      |        |        |        | j      |        |
   |        |        | unning |        |        |        | oining |        |
   |        |        | jobs,  |        |        |        | an     |        |
   |        |        | downl  |        |        |        | ex     |        |
   |        |        | oading |        |        |        | isting |        |
   |        |        | data   |        |        |        | re     |        |
   |        |        | for    |        |        |        | search |        |
   |        |        | proce  |        |        |        | allo   |        |
   |        |        | ssing, |        |        |        | cation |        |
   |        |        | and    |        |        |        | or     |        |
   |        |        | shor   |        |        |        | cr     |        |
   |        |        | t-term |        |        |        | eating |        |
   |        |        | s      |        |        |        | a new  |        |
   |        |        | torage |        |        |        | one.   |        |
   |        |        | for    |        |        |        |        |        |
   |        |        | large  |        |        |        | **To   |        |
   |        |        | dat    |        |        |        | get    |        |
   |        |        | asets. |        |        |        | a      |        |
   |        |        |        |        |        |        | ccess, |        |
   |        |        |        |        |        |        | make a |        |
   |        |        |        |        |        |        | r      |        |
   |        |        |        |        |        |        | equest |        |
   |        |        |        |        |        |        | fol    |        |
   |        |        |        |        |        |        | lowing |        |
   |        |        |        |        |        |        | the    |        |
   |        |        |        |        |        |        | i      |        |
   |        |        |        |        |        |        | nstruc |        |
   |        |        |        |        |        |        | tions\ |        |
   |        |        |        |        |        |        |  **\ ` |        |
   |        |        |        |        |        |        | here < |        |
   |        |        |        |        |        |        | https: |        |
   |        |        |        |        |        |        | //it.n |        |
   |        |        |        |        |        |        | orthwe |        |
   |        |        |        |        |        |        | stern. |        |
   |        |        |        |        |        |        | edu/de |        |
   |        |        |        |        |        |        | partme |        |
   |        |        |        |        |        |        | nts/it |        |
   |        |        |        |        |        |        | -servi |        |
   |        |        |        |        |        |        | ces-su |        |
   |        |        |        |        |        |        | pport/ |        |
   |        |        |        |        |        |        | resear |        |
   |        |        |        |        |        |        | ch/com |        |
   |        |        |        |        |        |        | puting |        |
   |        |        |        |        |        |        | /quest |        |
   |        |        |        |        |        |        | /quest |        |
   |        |        |        |        |        |        | -stora |        |
   |        |        |        |        |        |        | ge.htm |        |
   |        |        |        |        |        |        | l>`__\ |        |
   |        |        |        |        |        |        |  **\ . |        |
   |        |        |        |        |        |        | **     |        |
   +--------+--------+--------+--------+--------+--------+--------+--------+

Summary of File Permissions
---------------------------

Understanding how file permissions work in Linux is critical to
understanding who has access to files in each of the storage types.
Users get access to files in three different ways:

-  **user**: Your account.
-  **group**: A specific permissions group that your account belongs to.
-  **other**: Any account that is not yours and that does not belong to
   a permissions group that your account belongs to.

There are three basic types of permissions which can be assigned *by the
owner of the file or directory* to each of these three access points:
**read (r)** **write (w) and** **execute (x)**.

What does read, write and execute mean for a directory?

-  **read**: Allowed to list the contents of the directory
-  **write**: Allowed to create, modify or delete files in the directory
-  **execute**: Allowed to access a file in the directory if you know
   the name of the file

What does read, write and execute mean for a file?

-  **read**: Allowed to read the contents of the file
-  **write**: Allowed to modify or delete the file
-  **execute**: Allowed to run the file as a process, if possible

How do I view permissions on a file or directory? You use the utility
``ls``. For example,

::

   [quest_demo@quser10 ~]$ ls -ltd /home/quest_demo/
   drwx------ 40 quest_demo quest_demo 16384 May 4 14:57 /home/quest_demo/

.. _home:

Home Directories
----------------

As a Quest account holder, you will be given a home directory
(``/home/<netid>``) the first time you log in to the system. All home
directories have a fixed quota of 80GB. **Your home directory is only
available to you and may not be shared**.

Your home directory is a good place to store submission scripts, job log
files, and local package and software installations.

Home directories (unlike any other storage volume) are backed up. If you
inadvertently delete a file from your home directory, immediately
contact quest-help@northwestern.edu as it may be possible to restore it.
*As long as the file or directory in question was not created and
deleted within 24 hours, any files or directory can be recovered for up
to 4 weeks.*

Check Home SpaceUtilization
~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can check how much space you’re using in your home directory with
``homedu``:

::

   $ homedu

   Beginning detailed disk usage report for /home/<netid>.
   Please be patient - this can be a time-consuming operation.

   GPFS quota for /home/<netid>

   24.22 GB used in 240098 files (30.28% of 80 GB quota)

**If you leave the University, your NetID will expire. In this
circumstance, your home directory will be deleted and cannot be
restored.**

Before leaving the university and well ahead of your NetID expiration,
we recommend moving the files in your home directory. Solutions include:

-  Backup all the files you need from your home directory in a place you
   will maintain access to after your NetID expires.
-  Copy files that your colleagues need access to into a shared project
   directory.

If you need help backing up or copying your data to locations accessible
by your colleagues, please email
`quest-help@northwestern.edu. <mailto:quest-help@northwestern.edu>`__

.. _projects:

Project Space
-------------

The projects volume consists of high-speed storage and should be used
for computation Input/Output (IO) and/or data analyses. While project
and home directories are both reachable by a compute node, the home
directory provides much smaller storage than your allocation directory
provides.

The size of your project directory is determined by the details of your
allocation type. A Research Allocation I is usually set up with a 1TB
quota on the projects space, and a Research Allocation II may have up to
2TB of space. Researchers requiring additional storage may `purchase it
by the
TB <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-storage.html>`__.

Check Project Space Utilization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check how much space is used in your projects directory with

::

   $ checkproject <allocation_id> 

   ==================================== 
   Reporting for project <allocation_id>
   ------------------------------------
   1 GB in 4623 files (0% of 1000 GB quota)
   Allocation Type: Allocation I
   Expiration Date: 2022-12-01
   Status: ACTIVE
   Compute and storage allocation - when status is ACTIVE, this allocation has compute node access and can submit jobs
   ------------------------------------
   ====================================

Project Space Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~

Each Quest allocation has a projects directory:
``/projects/<allocationID>``. In the projects space anyone belonging to
an allocation may be able to read, write, and execute files that belong
to other users of that same allocation. There are exceptions to this
rule. For example, if you *copy* data from your home directory into your
project space it will inherit the permissions of the directory it is
being copied into as expected, but if you *move* data from your home
directory into your project space, the data retains the original
permissions and so the ownership and group ownership will belong to you.
No one else will be able to read your files in project space unless you
change the file permissions or the group ownership of your files is
changed.

Below, we show the default permissions for a file in your allocation
directory and then provide an example of modifying those default
permissions.

::

   $ cd /projects/<pXXXXX>/
   $ touch myfile.txt
   $ ls -l myfile.txt
   -rw-rw-r-- 1 <netid> <pXXXXX> 0 May 24 14:58 myfile.txt

The above file can be modified (the write (w) permission), by anyone in
your allocation. If you would like to leave the file as being able to be
read (r) by anyone in the allocation but want to remove the ability for
it to be modified by anyone in the allocation, you can use the ``chmod``
command to remove the \`w\` permission for the *group*.

::

   $ chmod g-w myfile.txt
   $ ls -l myfile.txt
   -rw-r--r-- 1 <netid> <pXXXXX> 0 May 24 14:58 myfile.txt

.. _scratch:

Scratch Space
-------------

The scratch volume consists of high-speed storage that provides
temporary storage for actively used research data at no cost to the
user. Appropriate use of scratch could reduce storage costs to
researchers and provide better data lifecycle management with less
storage resource waste. Some scratch storage use cases include:

-  storing temporary files created while running jobs,
-  downloading/staging data for processing soon to be performed,
-  large data transfers,
-  short-term storage for large datasets

Scratch directories are available to any Quest users currently in an
active allocation. You can apply by making a request following the
instructions
`here <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-storage.html>`__.
Access to Quest scratch space will typically be granted within one week
of completing the application. Once you have been approved, you will be
allocated a directory at the following location: ``/scratch/<netid>``.
You scratch directories can be accessed from all Quest nodes including
login nodes, compute nodes, and the Quest Analytics Nodes.

Quest scratch space is not backed up and files stored there are deleted
regularly. It is not appropriate for long-term storage or as the sole
location for research data that cannot be regenerated. Reference files,
datasets, software and scripts should be stored in a user’s allocation
directory rather than in scratch to avoid accidental deletion.

*When downloading data or transferring data to scratch, please carefully
note the last modified date of the file. Under certain download or
transfer conditions, the data will retain its original “last modified”
date and not receive a new “last modified” date. For example,
*

::

   $ date
   Mon Oct 31 11:54:57 CDT 2022
   $ wget https://github.com/pytorch/pytorch/releases/download/v1.12.0/pytorch-v1.12.0.tar.gz
   $ ls -lt pytorch-v1.12.0.tar.gz
   -rw-rw-r-- 1 <netid> <netid> 206507948 Jun 28 11:51 pytorch-v1.12.0.tar.gz

*In this situation, under the default retention period of 30 days, this
file would be deleted within the day. Although you
are*\ **not**\ *allowed to use* ``touch``, *to continually update the
last modification dates of files in an effort to save them from
deletion, you may use* ``touch`` *to ensure that a file’s initial
modification date reflects the time the file first lands in scratch.*

::

   $ touch pytorch-v1.12.0.tar.gz
   $ ls -lt pytorch-v1.12.0.tar.gz
   -rw-rw-r-- 1 <netid> <netid> 206507948 Oct 31 11:59 pytorch-v1.12.0.tar.gz

Note that jobs can access multiple directories when they run, and jobs
that write their temporary files into scratch can also access reference
files and write results in ``/projects`` or ``/home``.

Global Scratch Settings
~~~~~~~~~~~~~~~~~~~~~~~

To ensure scratch space availability for everyone, global scratch limits
may be adjusted by Northwestern IT if storage utilization is over a
critical threshold. When a change is required, scratch space users will
receive an advance notice. The current deletion period can be viewed
through the ``checkscratch`` command or in Quest’s Message Of The Day
(MOTD), which is displayed at the top of your terminal when you login to
Quest via SSH.

During normal operation conditions, the global scratch settings are
given in the table below. These limits cannot be adjusted for and
individual or upon request.

.. container:: table-responsive

   ================================ =======================
   Storage quota per user           5 TB
   File retention period            30 days after last edit
   Maximum number of files per user 5,000,000
   ================================ =======================

.. _checkscratch:

Check Scratch Space Utilization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use the utility ``checkscratch`` to monitor how much scratch
space you are using, show the current deletion period for scratch, and
determine which of your files, if any, will be deleted in the coming XX
days.

::

   $ checkscratch --help
   checkscratch reports a list of your scratch files that will be deleted in the upcoming days and your scratch utilization.

   Usage: checkscratch <days>, where <days> is the integer number of days from now to check.

   Example: checkscratch 3

   reports which files, if any, will be deleted within the next three days from now. The command writes the list of files to an output file called checkscratch.out in your scratch folder.

   checkscratch.out is overwritten each time you run it and search for same <days> cannot be repeated for a period of 1 hour; to save the output file you must move it or rename it.

For example, if you would like to know if you have any files that will
be deleted in the next 20 days, you would run and receive the following:

::

   $ checkscratch 20
   The current retention period of files in /scratch is 30 days.

   You have 2 files that will be deleted within the next 20 days (OLDER than 10 days) in /scratch/<netid> and are listed in the file /scratch/<netid>/checkscratch.out.

   Checkscratch's output file is overwritten each time you run it; to save the checkscratch output file you must move it or rename it.

``checkscratch`` will create a file in your scratch folder with a
summary of any files (and their size) which are older than 10 days (30
day retention period - will be deleted in 20 days).

::

   $ cat /scratch/<netid>/checkscratch.out
   You currently have 4 files using 12 GB of space in /scratch/<netid>

   Displaying files in scratch/<netid> that are last modified 10 or more days ago

   # PERMS  : SIZE KB :  MOD. DATE : FILENAME ############################################################

In this case, you have no files that are older than 10 days. So you
might try to now run ``checkscratch 28`` and see if you have any files
older than 2 days.

::

   $ checkscratch 28
   The current retention period of files in /scratch is 30 days.

   You have 1 files that will be deleted within the next 28 days (OLDER than 2 days) in /scratch/<netid> and are listed in the file /scratch/<netid>/checkscratch.out.

   Checkscratch's output file is overwritten each time you run it; to save the checkscratch output file you must move it or rename it.

``checkscratch`` is now telling you that you have 1 file that is older
than 2 days. To find out what file it is, you would look in
``/scratch/<netid>/checkscratch.out``.

::

   $ cat /scratch/<netid>/checkscratch.out
   You currently have 4 files using 12 GB of space in /scratch/<netid>

   Displaying files in scratch/<netid> that are last modified 2 or more days ago

   # PERMS  : SIZE KB :  MOD. DATE : FILENAME ############################################################
   -rw-rw-r--:0:2022-08-03:/scratch/<netid>/myfile.txt

``checkscratch.out`` will tell you both how large the file is in KB,
when the file was last modified, and the name of the file.

Scratch Space Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~

Only the user can write to their scratch directory. However, the user
can allow other users to read/execute their files. Below, we provide an
example of the default file permissions and how to provide others with
read/execute access.

First, we must modify the default permissions of your scratch directory.

::

   # Default permissions of scratch directory
   $ ls -ld /scratch/<netid>/
   drwx------ 3 <netid> <netid> 4096 May 26 12:46 /scratch/<netid>/

We must take two steps to allow users in a specific allocation to be
able to read/execute files in our scratch space.

::

   # Navigate to the scratch
   $ cd /scratch
   # use the `chown` command to change the user:group of your scratch directory to <netid>:<allocation_id>
   $ chown <netid>:<pXXXXX> <netid>
   # Now we need to allow the `group` to have read (r) and execute (x) permissions on our scratch directory
   $ chmod g+rx <netid> 
   # verify that the group setting and permissions for your scratch were updated correctly.
   $ ls -ld /scratch/<netid>/
   drwxr-x--- 3 <netid> <allocation_id> 4096 May 26 12:46 /scratch/<netid>/

Now we provide an example of how to change the \`group\` on a file
created inside of your scratch directory which will enable other members
of that allocation to read your file.

::

   $ cd /scratch/<netid>/
   $ touch myfile.txt
   $ ls -l myfile.txt
   -rw-rw-r-- 1 <netid> <netid> 0 May 24 14:53 myfile.txt

To give access to other allocations members, we need to modify the
*group* setting of this file.

::

   # Find out what allocations I am a part of.
   $ groups 
   <netid> <pXXXXX>
   # use the `chown` command to change the user:group to <netid>:<allocation>
   $ chown <netid>:<pXXXXX> myfile.txt
   # verify that the group setting for your file has updated to your allocation
   $ ls -l myfile.txt
   -rw-rw-r-- 1 <netid> <pXXXXX> 0 May 24 14:53 myfile.txt

You can always revert the *group* setting on your files and directories
in scratch back to your netid so that only you have read, write and
execute permission.

::

   $ chown <netid>:<netid> myfile.txt
   $ ls -l myfile.txt
   -rw-rw-r-- 1 <netid> <netid> 0 May 24 14:53 myfile.txt
