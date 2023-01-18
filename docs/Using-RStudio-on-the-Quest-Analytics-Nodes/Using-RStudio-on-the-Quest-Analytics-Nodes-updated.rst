Using RStudio on the Quest Analytics Nodes
==========================================

How to use RStudio on the Quest Analytics Nodes.

Overview
--------

The Quest Analytics Nodes allow users with active Quest allocations to
use RStudio from a web browser. See `Research Computing: Quest Analytics
Nodes <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-analytics-nodes.html>`__
for an overview of the system.

-  `Connect <#connect>`__
-  `Transfer Files <#transfer>`__
-  `Install Packages <#packages>`__
-  `Troubleshoot <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1694>`__

Note: you cannot `submit
jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
to the Quest job scheduling system using the Quest Analytics Nodes.

Connecting
----------

Users must already have a Quest account and an active Quest allocation.

Connections to the Quest Analytics Nodes are limited to the Northwestern
network. If you are connecting from off-campus, you must use the
`Northwestern
VPN <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1818>`__.

Using any browser, connect to:
https://rstudio.questanalytics.northwestern.edu

Sign in with your NetID and password.

Transferring and Accessing Files
--------------------------------

All of the `Quest file
systems <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1546>`__
are accessible from the Quest Analytics Nodes. You can `transfer
files <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
using the methods to transfer files to Quest more generally.

You can also transfer files *smaller than 500MB* using the Files tab in
RStudio. There is an upload button that will allow you to select files
from your computer to transfer.

.. image:: https://kb.northwestern.edu/images/group293/71895/filestab_upload.jpeg
   :class: fr-fic fr-dii
   :width: 595px
   :height: 115px

When you connect to RStudio, you will initially be in your home
directory. This is ``/home/<netid>`` on Quest.

To access a project directory, click on the button with three small dots
on the right in the Files tab. Then enter the full path to your project
folder (ex. ``/projects/<allocationID>``).

.. image:: https://kb.northwestern.edu/images/group293/71895/filestab.jpeg
   :class: fr-fic fr-dii
   :width: 595px
   :height: 115px

| 

.. image:: https://kb.northwestern.edu/images/group293/71895/projectdialog.jpeg
   :class: fr-fic fr-dii
   :width: 417px
   :height: 184px

To transfer files larger than 500MB to Quest, please use `SFTP or
another transfer
method <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__.

Installing Packages
-------------------

Users may install packages using the Packages tab in RStudio version
1.3.1093 or the install.packages function in R. Any packages you install
on Quest for R version 4.0.3 will also be available to you on the Quest
Analytics Nodes. Some packages that require code to be compiled may need
to be installed by `connecting to a Quest login
node <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__
and following the tips below. Please see `Installing and Managing R
Packages on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1627>`__\ for
more information.

System Details
--------------

RStudio is version 1.3.1093. R is version 4.0.3. The Analytics Nodes are
limited to running a single version of R for all users.

The Quest Analytics Nodes are shared by many researchers. Please be
aware of your memory use when analyzing large data sets. Users utilizing
a large amount of memory, especially those using over 60GB of RAM, may
be asked to move their analysis to other systems. Multicore and parallel
processes should not be run on the Analytics nodes. Users needing to run
computationally intensive jobs should schedule `interactive or batch
jobs on
Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
instead of using the Analytics nodes. Please contact Research Computing
at quest-help@northwestern.edu with questions about R memory use or
analyzing large data sets.

Issues or Problems with the Analytics Nodes
-------------------------------------------

To report issues or problems with the Analytics Nodes, please email
quest-help@northwestern.edu with information on which service you were
using (RStudio), the error you received or problem you encountered, and
what you were doing prior to the problem occurring. Please note that you
were using the Analytics Nodes.
