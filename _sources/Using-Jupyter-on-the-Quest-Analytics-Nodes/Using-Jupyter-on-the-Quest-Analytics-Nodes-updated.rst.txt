Using Jupyter on the Quest Analytics Nodes
==========================================

Jupyter notebooks are available on the Quest Analytics nodes.

Overview
--------

The Quest Analytics nodes allow users with active Quest allocations to
launch Jupyter notebooks from a web browser. See `Research Computing:
Quest Analytics
Nodes <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-analytics-nodes.html>`__
for an overview of the system.

-  `Connect <#connect>`__
-  `Transfer Files <#transfer>`__
-  `Environments and Packages <#packages>`__

Note: you cannot `submit
jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
to the Quest job scheduling system using the Quest Analytics nodes.

Connecting
----------

Users must already have a Quest account and an active Quest allocation.

Connections to the Quest Analytics Nodes are limited to the Northwestern
network. If you are connecting from off-campus, you must use the
`Northwestern
VPN <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1818>`__.

Using any browser, connect to:
https://jupyter.questanalytics.northwestern.edu and sign in with your
NetID and password.

Note: notebook servers are automatically killed after 18 hours of
inactivity.

Transferring and Accessing Files
--------------------------------

All of the `Quest
filesystem <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1546>`__
is accessible from the Quest Analytics nodes. You can `transfer
files <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
using the methods to transfer files to Quest more generally.

You can also transfer files *smaller than 500MB* using the the Jupyter
web interface. There is an Upload button that will allow you to select
files from your computer to transfer.

.. image:: https://kb.northwestern.edu/images/group293/94116/jupyterupload.PNG
   :alt: Jupyter upload button
   :width: 1155px
   :height: 123px

When you connect to Jupyter, you will initially be in your home
directory. This is ``/home/<netid>`` on Quest. You cannot navigate to a
directory “outside” of your home directory (e.g., to a project
directory) via the Jupyter interface. However, you can SSH to Quest and
use the terminal to create a symlink to a project directory within your
home directory, allowing you to navigate there. For example:

.. code:: code

   ln -s /projects/<projectID> ~/<projectID>

This will create a symlink ``<projectID>`` in your home directory, and
when you click on it in the Jupyter interface you will be taken to the
project directory itself.

To transfer files larger than 500MB to Quest, please use `SFTP or
another transfer
method <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__.

Environments and Packages
-------------------------

The default “root” environment for Jupyter on Quest Analytics is Python
3.7 and Anaconda version 2018.12. The list of installed packages and
versions in this environment can be found here:
https://docs.anaconda.com/anaconda/packages/old-pkg-lists/2018.12/py3.7_linux-64/.

If you need other packages (or versions of packages) installed, you can
create a conda environment and add it to your list of available
environments using the commands below (Note you must SSH or use FastX to
access Quest and type these commands in the terminal):

Create a iPython Kernel
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: code

   module purge all
   module load python-miniconda3/4.12.0
   conda create --name jupyter-kernel-py38 -c conda-forge python=3.8 numpy pandas matplotlib ipykernel --yes
   source activate jupyter-kernel-py38
   python -m ipykernel install --user --name jupyter-kernel-py38 --display-name "Python (jupyter-kernel-py38)"

The last command adds the environment to your list of available
environments in Jupyter, and once you have done this it will be
available for you under the “New” menu.

Create a R Kernel
~~~~~~~~~~~~~~~~~

::

   module purge all
   module load python-miniconda3/4.12.0
   conda create -c conda-forge --name r-kernel r-irkernel r=4.1 jupyterlab
   source activate r-kernel
   R -e "IRkernel::installspec(name = 'ir41', displayname = 'R4.1')"

The last command adds the environment to your list of available
environments in Jupyter, and once you have done this it will be
available for you under the “New” menu.

In addition, to list all of the available kernels that you have
installed, you can run

::

   module load anaconda3/2018.12
   jupyter kernelspec list

For more information about managing environments with conda, see `Using
Python on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1672>`__.

System Details
--------------

The Quest Analytics nodes are shared by many researchers. Please be
aware of your memory use when analyzing large data sets. Users utilizing
a large amount of memory, especially those using over 60GB of RAM, may
be asked to move their analysis to other systems. Multicore and parallel
processes should not be run on the Analytics nodes. Users needing to run
computationally intensive jobs should schedule `interactive or batch
jobs on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
instead of using the Analytics nodes. Please contact Research Computing
at quest-help@northwestern.edu with questions about memory use or
analyzing large data sets.

Issues or Problems with the Quest Analytics nodes
-------------------------------------------------

To report issues or problems with the Quest Analytics nodes, please
email quest-help@northwestern.edu with information on which service you
were using (Jupyter), the error you received or problem you encountered,
and what you were doing prior to the problem occurring. Please note that
you were using the Quest Analytics nodes.
