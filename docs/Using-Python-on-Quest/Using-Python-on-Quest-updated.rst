Using Python on Quest
=====================

Tips for using Python on Quest.

-  `Using Python on Login Nodes <#loginrun>`__
-  `Using Python with Scheduled Jobs <#scheduled>`__
-  `Python Package Management <#packages>`__
-  `Jupyter Notebooks on Compute Nodes via Interactive
   Jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1791>`__

Availability
------------

.. container::

   Python is available for use on Quest login and compute nodes via the
   command line and GUI IDEs. You can access the compute nodes via
   `scheduled
   jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__.

 Using Python on Login Nodes
---------------------------

Python scripts can be run on the login nodes with some limits. Users
cannot use more than 4 cores or 4 GB of memory in total for all the
applications they are running on a login node. These limits are
implemented to ensure a smooth operation for all usersconnecting to
Quest through login nodes.

All Quest nodes natively include Python (version 2.7.5). However, this
Python version has no package support and users are **strongly**
encouraged to use Python distributions provided by the module system.
You can see the available versions of Python with the command

.. container:: code

   module spider python

The command output will include several ``python/<distribution>``
modules. Currently, only Anaconda distributions are actively supported
on Quest. You can make a particular version of Python available to use
by loading the corresponding module. For instance, if you would like to
work with Anaconda distribution of Python 3.6, you should type the
command

.. container:: code

   module load python/anaconda3.6

To start the interpreter, type ``python`` on the command line. You can
also run your Python script directly by typing the following line on the
command line

.. container:: code

   python <your_script_name.py>

 Scheduled Jobs: Basic Python Use
--------------------------------

Python can be used interactively on login without scheduling jobs as
discussed above. Note that, however, the login node use is for
development and non-computationally intensive tests runs. Compute nodes
can be accessed via scheduling jobs on Quest. There are two classes of
jobs: interactive and batch.

Interactive Use on the Compute Nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To use Python interactively, please first submit an `interactive
job <page.php?id=69247#interactive>`__ to obtain the cpu and memory
resources you need from a compute node. Once your job starts, your
command line will move to a compute node where you can proceed as if you
are on the login node. If you are planning to do computationally
intensive work while maintaining interactivity with the script or the
interpreter, this type of use recommended. This is also the way to
request resources to use an IDE such as Spyder (which is available as
part of the Anaconda distributions of Python).

Batch Jobs
~~~~~~~~~~

In submission scripts for `batch jobs <page.php?id=69247#batch>`__, you
must include the command to load the version of Python that you want to
run, then add the line where you can call Python:

.. container:: code

   python <your_script_name.py>

which causes the output and errors to be written to the files for
``stdout`` and ``stderr``, respectively, which are determined by your
job submission parameters (see `Submitting a Job on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__).

See `Example of Python Batch Jobs for
Quest <https://github.com/nuitrcs/examplejobs/tree/master/python>`__ for
template files you can adapt for your work.

Usage Notes
~~~~~~~~~~~

While ``module load python`` will load the current default version of
Python, to ensure the future compatibility of your scripts with the
Quest system, **we highly recommend that you always load a specific
version of Python rather than relying on the default version**.

Python Package Management
-------------------------

.. container::

   .. rubric:: Checking Available Packages Provided by Module
      :name: checking-available-packages-provided-by-module

   It is important to make sure that the required packages for a Python
   script are already installed in the Python module you have loaded. If
   the packages are not available your job will fail. The most reliable
   way to test the availability is to import the package within the
   Python interpreter. If you don’t receive any errors after the import,
   that means the package is available. You can also inquire the version
   of the package with the Python command ``<package-name>.__version__``
   as most packages include this information. For instance if you would
   like to test Numpy’s existence and query its version you could do the
   following:

   .. code:: code

      $ module load python/anaconda3.6
      $ python
      >>> import numpy as np
      >>> np.__version__

   ``pip freeze`` will list packages in your current environment, but
   the list may be incomplete. It is best to test package availability
   by importing the package, as above.

   .. rubric:: Installing and using Python packages when module packages
      insufficient
      :name: installing-and-using-python-packages-when-module-packages-insufficient

   .. container::

      Although you may find references to using
      ``pip install --user <mypackage>`` as a method to install external
      Python packages on shared computing systems like Quest, we
      strongly recommend that you utilize virtual environments instead.
      Using ``--user`` to install Python packages locally can often lead
      to having multiple, sometimes conflicting, versions of a package.
      For instance, you may want to use TensorFlow which relies on
      having one version of numpy installed, but also you want to use
      some other package which requires a version of numpy which is
      incompatible with that TensorFlow installation. The method
      ``--user``\ cannot handle these conflicts the way that having
      separate isolated virtual environments can.

Anaconda Virtual Environments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use `conda
environments <https://conda.io/docs/user-guide/tasks/manage-environments.html>`__
with the Anaconda Python modules on Quest, or `virtual
environments <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`__
with any Python module. There are multiple ways to utilize virtual
environments. First, you can use conda to create an environment with
only Python installed in it. For example, the commands below will create
an isolated environment in your HOME folder in the following location
``~/.conda/envs/my-virtenv-py38`` with only Python 3.8 installed.

::

   $ module load python-miniconda3
   $ conda create --name my-virtenv-py38 python=3.8 --yes
   $ source (conda) activate my-virtenv-py38

Once the environment is activated, you have full control over what
Python packages are installed into the environment. In many ways, you
are the system administrator of this Python installation as you have
full read/write/execute privileges inside of that folder in your HOME
directory. At this point, you can use Python’s native package manager,
`PyPi <https://pypi.org/>`__, to install whatever Python package(s) you
want to as long as they are available via pip. For example,
``pip install numpy matplotlib scipy pandas``.

In some instances, you may find it useful to install Python packages
using anaconda itself instead of PyPi. An overwhelming majority of
software (both Python and non-Python) is available via anaconda in one
of three locations. These locations are called “channels”, which can be
thought of as the remote/cloud repository in which anaconda looks for
the package. These three locations are

`–channel=conda-forge <https://conda-forge.org/feedstock-outputs/>`__

`–channel=anaconda <https://anaconda.org/anaconda/repo>`__\ (a.k.a the
default location)

`–channel=bioconda <https://anaconda.org/bioconda/repo>`__

If you do opt to install packages directly from anaconda (which can help
insure that all the packages are compatible with each other), it is good
practice to make sure they are all installed from the same remote
repository/channel.

Finally, if you are need of C and/or C++ compilers you can install those
directly into your environment via

``conda install -c conda-forge gxx_linux-64 gcc_linux-64``

To share an environment with other members of your allocation, you can
create the environment in a specific directory.

With conda, use the –prefix option to create the environment in a
specific location, such as in your /projects/<allocationID> directory.
To use the environment (either interactively from the command line or as
part of your job submission script), you then need to point to this
directory when activating it.

For example, to create a conda environment called env1 in a subdirectory
of a project directory (here p10000 as an example), with the package
sqlalchemy included, run the following:

::

   $ module load python-miniconda3
   $ cd /projects/p10000
   $ mkdir pythonenvs
   $ conda create --prefix /projects/p10000/pythonenvs/env1 sqlalchemy python=3.8 --yes

To use this environment, specify the full path to the environment when
you activate it:

::

   $ module load python-miniconda3
   $ source activate /projects/p10000/pythonenvs/env1

“conda activate” vs. “source activate”:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Older conda versions used the source activate <environment name> syntax
to activate an environment on Linux, which differed from how
environments were activated on Windows. In order for conda to work
consistently across platforms, its behavior changed slightly in version
4.6, and now conda activate <environment name> works on all platforms.
To use the new conda, run these commands one time to set up your
environment:

::

   $ module load python-miniconda3
   $ conda init bash

Then log out of Quest and log back in. Once you have done this, you can
use conda activate instead of source activate, although *the latter will
still work*, so existing batch scripts for example can continue to use
source activate.

If you need help creating an anaconda virtual environment, please e-mail
quest-help@northwestern.edu.
