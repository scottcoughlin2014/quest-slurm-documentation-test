Running Jupyter Notebook on Quest
=================================

Jupyter Notebook can be launched and run on a Quest compute node through
an interactive job on Quest.

To schedule the interactive job from the command line on Quest, ssh into
a login node and type:

::

   srun -A <allocation_name> -p <queue_name> -N 1 --ntasks-per-node=1 --mem-per-cpu=4G --time=04:00:00 --pty bash -l

This example requests a single core for a 4 hour job. Substitute an
active allocation name and queue name, for example if using allocation
p12345 this might be:

::

   srun -A p12345 -p short -N 1 --ntasks-per-node=1 --mem-per-cpu=4G --time=04:00:00 --pty bash -l

Note that the more cores requested, the longer the wait for the
interactive session to start. Do not request more than 1 node for
Jupyter notebook sessions.

Once the session begins, get the name of the compute node the session
has landed on:

``hostname``

Load the version of python that you require for the session. For
example, to load Python 3.7 use
``module load python-anaconda3/2019.10``. Once the correct Python module
has loaded, type:

::

   jupyter notebook --port=8899 --no-browser

This example uses port 8899, *but a please specify a different port as
this example port is likely already in use.*

Once jupyter notebook is running on the compute node, open a new
terminal window on your **local computer**, and type:

::

   ssh -L 8899:localhost:8899 <your_netID>@quest.northwestern.edu ssh -N -L 8899:localhost:8899 qnode<number>

If using a different port number, substitute it for the number “8899” in
the command. Be sure and use your netID, and at the end replace
``qnode<number>`` with the name of the compute node. You will be
prompted for your Quest password, which will not return a prompt.

On your **local computer**, open up your browser and connect to
``http://localhost:8899/``. If not using port 8899, change the address
to use the port specified when you launched jupyter notebook. Your
browser is now connected to the jupyter notebook session running on
Quest.

Note that your jupyter notebook session will quit abruptly when the
walltime of the interactive job comes to an end. Save often and be aware
of walltime to avoid losing your work.

Accessing Conda Environments in Jupyter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, jupyter will use the “root” environment of the Anaconda
module you have loaded. If you are using a conda environment, for
example to be able to install specific versions of software or because
you are sharing an environment with other people in your group, you will
need to manually install the environment into your jupyter
configuration. This can only be done from the command line, not within
the jupyter notebook. First, load whichever Anaconda module you are
using and activate your environment, for example:

Create a iPython Kernel
~~~~~~~~~~~~~~~~~~~~~~~

::

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

Then, when you launch jupyter using the commands given above, you will
have the option of launching a notebook using your newly installed
environment as the kernel:

.. image:: https://kb.northwestern.edu/images/group293/88448/jupyternewmenu.PNG
   :alt: jupyter new notebook menu
   :width: 248px
   :height: 250px
