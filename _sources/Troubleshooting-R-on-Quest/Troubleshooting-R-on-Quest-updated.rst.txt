Troubleshooting R on Quest
==========================

Tips for addressing common problems using R on Quest.

Common Issues
-------------

-  `My RStudio session is frozen <#rstudiofrozen>`__
-  `My R script runs in R but gives me errors as a batch
   job <#batcherror>`__
-  `I’m getting an error when trying to install an R package that looks
   like “ERROR: compilation failed for package…” <#compilationerror>`__

My RStudio session is frozen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If your RStudio session is frozen, one option is to delete your
``.rstudio`` and ``.rstudio-desktop`` directories in your Quest home
directory.

The risks of doing this are:

-  Losing any unsaved files or objects in the environment in RStudio
   sessions
-  Disrupting any running sessions
-  RStudio not remembering what project or files you had open

To delete these directories, first log out of all `Quest
Analytics <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-analytics-nodes.html>`__
sessions and close any RStudio sessions you have running from Quest
login nodes or `interactive jobs <page.php?id=69247#interactive>`__.
Then, if you aren’t already, `connect to a Quest login
node <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__.

**NOTE: Please type the following commands carefully. Removing files on
Linux systems can’t be undone.**

.. code:: code

   cd
   rm -r .rstudio
   rm -r .rstudio-desktop

The try launching a new RStudio session however you were using it
before. If you’re still encountering a problem, contact
quest-help@northwestern.edu for further assistance.

My R script runs in R but gives me errors as a batch job
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Case 1: Your script runs in R or RStudio on the login nodes *and* as an `interactive job <page.php?id=69247#interactive>`__ on the compute nodes but gives you an error as a `batch job <page.php?id=69247#batch>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you are using ``Rscript`` as the R executable in your batch job
submission script, you may need to add ``library(methods)`` to the top
of your R script (your ``.R`` file).

The ``R`` executable that runs in RStudio, or starts the R console, or
is invoked via ``R CMD BATCH`` automatically loads the methods library
by default. ``Rscript`` doesn’t. This often leads to errors concerning
object types or classes, or errors surrounding types of matrices.

Case 2: Your script runs in R or RStudio on the login nodes but you get an error on the compute nodes via both an `interactive job <page.php?id=69247#interactive>`__ and `batch job <page.php?id=69247#batch>`__
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

One of the following situations likely applies:

-  A package you are using needs to access a system library that isn’t
   installed on the compute nodes. This most commonly occurs surrounding
   locale information or fonts.
-  You installed a package with a non-default version of a compiler
   module loaded. You may need to add a module load statement for the
   relevant compiler to your job submission script.
-  You’re accidentally using different versions of R in the two places.
   Make sure you’re loading the R module with the version specified.

If you’re still encountering a problem after trying the above, contact
quest-help@northwestern.edu for further assistance.

I’m getting a compilation error when installing or updating a package
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you’re using the `Quest Analytics
Nodes <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-analytics-nodes.html>`__,
you may need to `log in to
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__
directly to install or update the package. This is because you may need
to load a newer version of system libraries via the `module
system <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1550>`__
to compile the package during installation successfully. See `installing
R packages <page.php?id=71270#packages>`__ for an example.

If you’re not using the Analytics Nodes, or you’re not sure what you
need to change to get the package to install successfully, contact
quest-help@northwestern.edu for further assistance.
