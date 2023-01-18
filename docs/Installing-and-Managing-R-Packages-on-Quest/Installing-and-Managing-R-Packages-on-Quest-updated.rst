Installing and Managing R Packages on Quest
===========================================

Installing and managing R packages on Quest.

.. _installing-and-managing-r-packages-on-quest-1:

Installing and Managing R Packages on Quest
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| R packages are installed, whether system-wide or locally in your own
  directory, for specific versions of R. If you switch versions of R,
  you may need to reinstall packages you use. However, packages only
  need to be installed once on Quest for each version of R. Once
  installed, a package is available across all log in and compute nodes
  on Quest for that version of R. On the analytics node, due to
  technical limitations, there are a limited set of modules that are
  pre-loaded, and they are, exclusively,

.. code:: code

   gdal/3.1.3
   proj/7.1.1
   geos/3.8.1

If your R package depended on a module that is not one of the above, it will not load properly on the analytics node.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

System-wide Packages
^^^^^^^^^^^^^^^^^^^^

A limited number of R packages have been installed centrally for each
version of R. These packages can be used without downloading and
installing them first. You can get a list of the currently installed
packages with the command:

installed.packages()

Package Installation
^^^^^^^^^^^^^^^^^^^^

Users can install additional R packages from CRAN by launching RStudio
and using the Packages tab in the bottom-right window, or by running the
R command line console and using the ``install.packages`` command:

install.packages(c(“packagename1”, “packagename2”))

with a list of the packages you need. Packages should generally be
installed under your home directory. They will be available for you to
use across Quest nodes.

First-time Installation
^^^^^^^^^^^^^^^^^^^^^^^

The first time you install an R package on Quest, you may see an error
like:

::

    Warning in install.packages("glmnet", repos = "https://cloud.r-project.org/") :
     'lib = "/hpc/software/R/4.0.3/lib64/R/library"' is not writable
     Would you like to use a personal library instead? (y/n)

Answer “y” to use a personal library. Then it will ask you:

::

    Would you like to create a personal library
     ~/R/x86_64-pc-linux-gnu-library/4.0
     to install packages into? (y/n)

Answer “y” again. The installation should then proceed successfully.
This will install the packages in your home directory.

Setting the Repository
^^^^^^^^^^^^^^^^^^^^^^

If you get an error message indicating that a particular package isn’t
available for a certain version of R, or you get another error related
to the package repository, you may need to explicitly set the mirror
from which you want R to download the package. `List of CRAN
mirrors <https://cran.r-project.org/mirrors.html>`__. Choose an http
(instead of https) mirror. You can set the mirror before trying to
install packages with the command:

options(repos = “https://cloud.r-project.org/”)

or as part of the ``install.packages`` call with the ``repos`` argument:

install.packages(c(“packagename1”, “packagename2”), repos =
“https://cloud.r-project.org/”)

Compiled Packages
^^^^^^^^^^^^^^^^^

For Use on Login and Compute Nodes
''''''''''''''''''''''''''''''''''

Some packages compile underlying code as a part of the installation. To
install such packages, you may need to load a newer version of the
``gcc`` compiler before starting R and installing the package. Loading a
different compiler should NOT be done for packages you intend to use on
the `Analytics
nodes <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1560>`__,
as the updated ``gcc`` modules are not available on those nodes.

For example, if while installing a package, you see an error like:

configure: WARNING: Only g++ version 4.6 or greater can be used with…

try loading the module for a newer version of ``gcc``; For example,
after logging into Quest:

| module load gcc/5.1.0
| module load R/3.3.3
| R

then in the R console, issue the package installation commands again.

For Use on Analytics Nodes
''''''''''''''''''''''''''

R packages intended for use on the Analytics nodes should be installed
from within an R session running in RStudio in the web browser on the
Analytics nodes. If you encounter compilation or installation errors for
a package, please contact quest-help@northwestern.edu with the details
for further assistance.

R packages installed via the Quest login or compute nodes with the
module ``R/4.0.3`` will also be available on the Analytics nodes.

Note
''''

| Some compiled packages cannot be successfully installed by individual
  users. Please contact quest-help@northwestern.edu if you run into
  trouble with a particular package. Also, you can check out our page on
  `troubleshooting the installation of common R
  packages <https://kb.northwestern.edu/troubleshooting-installing-r-packages>`__
  in case your package is on this list!

More Options
^^^^^^^^^^^^

See the R help page for ``install.packages`` for more options, such as
installing local (user-created or downloaded) packages or changing the
directory where packages are installed. The latter can be used to
install packages in your project space instead of your home directory,
which might be useful if others on the allocation also need to access
the packages. To install packages from GitHub, use the
``install_github`` function in the devtools package or use the
githubinstall package.

Task Views
^^^^^^^^^^

| `CRAN Task Views <https://cran.r-project.org/web/views/>`__ are
  collections of packages used frequently in particular fields. They
  allow you to install all of the packages associated with the task view
  at once. See the R documentation for more details and the commands to
  use.

Additional Help
^^^^^^^^^^^^^^^

Please contact quest-help@northwestern.edu for additional help with R
package installation.
