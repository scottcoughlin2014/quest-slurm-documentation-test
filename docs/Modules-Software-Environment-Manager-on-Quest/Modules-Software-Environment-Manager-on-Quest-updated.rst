Modules Software Environment Manager on Quest
=============================================

How to load and manage environment modules on Quest to access software.

Quest uses `Environment Modules <http://modules.sourceforge.net/>`__ to
give users access to the software installed on Quest.

Modules are used to manage:

-  multiple versions of applications, tools and libraries

-  software where complex changes to the environment are necessary

-  software where name conflicts with other software would cause
   problems

Loading a module for a particular piece of software often adds the path
to the executable to ``$PATH``, the path to the library to
``$LD_LIBRARY_PATH``, and so on. Loading a module, then, relieves the
user of having to remember or look up and type long path names.

Available Modules
~~~~~~~~~~~~~~~~~

Use the command

module avail

to list all modules available on Quest.

A listing of key software packages can also be found on the `Quest
Software <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1547>`__
page.

Loading a Module
~~~~~~~~~~~~~~~~

To access almost every software application on Quest, you must first
load the appropriate module:

module load <modulename>

Then you can reference software commands by name.

Examples of Loading Popular Software via Modules
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

There may be other ways to run these programs as well. These commands
are only provided as a convenient reference.

Python
^^^^^^

.. code:: code

   module load python-anaconda3
   python myscript.py

| There are multiple versions of Python available on Quest.
  ``module load python-anaconda3``\ will load Python 3.7. For other
  versions of Python, use the appropriate Anaconda Python module. See
  `Using Python on
  Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1672>`__
  for more information and examples.

R
^

.. code:: code

   module load R/4.1.1
   Rscript mycode.R

See `Using R on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1556>`__
for more information and examples.

MATLAB
^^^^^^

.. code:: code

   module load matlab/r2018a
   matlab -nosplash -nodesktop -singleCompThread -r 'commands;exit'

or

.. code:: code

   module load matlab/r2018a
   matlab -nosplash -nodesktop -singleCompThread -r myscript

Where myscript is a .m file. Note that there are multiple versions of
MATLAB. You should specify the version you want, as the default may
change over time. Seeing `Using MATLAB on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1548>`__
for more information and examples.

Stata
^^^^^

.. code:: code

   module load stata/14
   stata-mp < myprog.do > myprog.log

or

.. code:: code

   module load stata/14
   stata-mp -b do myprog

Stata will read its commands from ``myprog.do`` and write its output to
``myprog.log`` in the working directory.

Note that Stata-MP is licensed for 4 cores, so please request exactly 4
cores per job in your job submission script. If you request more, you’ll
be wasting resources; if you request less and do not set the
corresponding options appropriately in your script, Stata may use more
cores than you’ve been assigned, resulting in your job being terminated.
If you don’t need Stata-MP’s parallelization capabilities, you can run
Stata SE with ``stata`` instead.

SAS
^^^

.. code:: command

   module load sas
   sas myprog.sas

SAS can write very large temporary files. You can redirect such files to
your project space with the ``-work`` option when you start-up SAS.

Mathematica
^^^^^^^^^^^

.. code:: command

   module load mathematica
   math -script input.txt > output.txt

Module Versions
~~~~~~~~~~~~~~~

There are multiple versions of many software programs on Quest. Modules
are named with the format ``softwarename/version``. For example, if you
look at the modules available for Python, you’ll see several versions:

module avail python

::

   ---------------------------------------- /software/Modules/3.2.9/modulefiles -----------------------------------------
   python/2.7.13            python/ActivePython-3.2  python/anaconda(default) python/anaconda3.6
   python/ActivePython-2.7  python/Canopy            python/anaconda3         python/epd-7.3-2

``(default)`` indicates which module will be loaded if you don’t specify
the version (e.g. ``module load python``). Defaults can change over
time, however, so it is **strongly recommended that you always specify
the module version** even if you are using the default version. For
example,

module load python-anaconda3

Useful Module Commands
~~~~~~~~~~~~~~~~~~~~~~

.. container:: table-responsive

   +-----------------------------------+-----------------------------------+
   | Command                           | Action                            |
   +-----------------------------------+-----------------------------------+
   | module avail                      | Shows the available software      |
   |                                   | packages                          |
   +-----------------------------------+-----------------------------------+
   | module avail <search>             | Shows all modules that have       |
   |                                   | <search> in the main part of the  |
   |                                   | name (not in the version, which   |
   |                                   | comes after the /). <search> is   |
   |                                   | case sensitive. This is useful    |
   |                                   | for viewing the versions          |
   |                                   | available for a particular        |
   |                                   | program instead of having to sort |
   |                                   | through the long list of all      |
   |                                   | modules.                          |
   +-----------------------------------+-----------------------------------+
   | module -r spider ‘^p’             | Finds all the modules that start  |
   | module -r spider mpi              | with \`p’ or \`P’                 |
   | module -r spider ’mpi$            | Finds all modules that have “mpi” |
   |                                   | in their name.                    |
   |                                   | Finds all modules that end with   |
   |                                   | “mpi” in their name.              |
   +-----------------------------------+-----------------------------------+
   | module list                       | Shows which modules are currently |
   |                                   | loaded                            |
   +-----------------------------------+-----------------------------------+
   | module load <module>              | Loads a software package’s path   |
   |                                   | information into your local       |
   |                                   | environment so your session can   |
   |                                   | find the software to run it       |
   +-----------------------------------+-----------------------------------+
   | module purge <module>             | Takes the software package’s      |
   |                                   | information out of your local     |
   |                                   | environment; this is generally    |
   |                                   | more reliable than                |
   |                                   | ``module unload``                 |
   +-----------------------------------+-----------------------------------+
   | module purge all                  | Unloads all of the module         |
   |                                   | packages currently in your local  |
   |                                   | environment                       |
   +-----------------------------------+-----------------------------------+
   | module display <foo>              | Displays the changes that are     |
   |                                   | made to the environment by        |
   |                                   | loading module <foo> without      |
   |                                   | actually loading it               |
   +-----------------------------------+-----------------------------------+
