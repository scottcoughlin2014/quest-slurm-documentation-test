Using SAS Studio on the Quest Analytics Nodes
=============================================

How to connect to, transfer files to, and use SAS Studio on the Quest
Analytics Nodes.

Overview
--------

| The Quest Analytics Nodes allow users with active Quest allocations to
  use SAS Studio from a web browser. See `Research Computing: Quest
  Analytics
  Nodes <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-analytics-nodes.html>`__
  for an overview of the system.
| Note: you cannot `submit
  jobs <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
  to the Quest job scheduling system using the Quest Analytics Nodes.

Connecting
----------

| Users must already have a Quest account and an active Quest
  allocation.
| Connections to the Quest Analytics Nodes are limited to the
  Northwestern network. If you are connecting from off-campus, you must
  use the `Northwestern
  VPN <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1818>`__.
| Using any browser, connect to:
  `https://sas.questanalytics.northwestern.edu
   <https://sas.questanalytics.northwestern.edu>`__
| Sign in with your NetID and password.
| Sessions time out after one hour of inactivity.

Transferring and Accessing Files
--------------------------------

| All of the `Quest file
  systems <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1546>`__
  are accessible from the Quest Analytics Nodes. You can `transfer
  files <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__
  using the methods to transfer files to Quest more generally.
| You can also transfer files *smaller than 500MB* using the SAS Studio
  interface. See the upload icon in the Server Files and Folders window
  in the upper left of the session.
| |image1|
| When you first connect to SAS Studio, you will be in your Quest home
  directory (``/home/<netid>``). To access your projects directory
  (``/projects/<allocationID>``), create a new folder shortcut. To do
  this, click on the icon in the upper left in the Server Files and
  Folder window that looks like a rectangle with an \* on it.
| |image2|
| From the Menu, choose Folder Shortcut. Then give the shortcut a name
  and enter the path to your projects folder. For example, if your
  allocation ID was p200000
| |image3|
| When you click Save, the folder is now accessible from the Folder
  Shortcuts menu in Server Files and Folders:
| |image4|
| You can access any of your files on Quest in your scripts by
  specifying the full path to the file.
| To transfer files larger than 500MB to Quest, please use `SFTP or
  another transfer
  method <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1535>`__.

System Details
--------------

| SAS is version 9.4 with maintenance release 4 applied (STAT module is
  version 14.2).
| Users are limited to 4GB of RAM (memory) for SAS sessions on the
  Analytics Nodes. Researchers requiring more RAM for SAS processes
  should use `interactive or batch jobs on
  Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__.
  Multicore and parallel processes should not be run on the Analytics
  nodes. Users needing to run computationally intensive jobs should
  schedule `interactive or batch jobs on
  Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1964>`__
  instead of using the Analytics nodes.

Issues or Problems with the Analytics Nodes
-------------------------------------------

| To report issues or problems with the Analytics Nodes, please email
  quest-help@northwestern.edu with information on which service you were
  using (SAS Studio), the error you received or problem you encountered,
  and what you were doing prior to the problem occurring. Please note
  that you were using the Analytics Nodes.

.. |image1| image:: https://kb.northwestern.edu/images/group293/76583/sasfiles.jpeg
   :width: 325px
   :height: 160px
.. |image2| image:: https://kb.northwestern.edu/images/group293/76583/sasfiles_newshortcut.jpeg
   :width: 325px
   :height: 160px
.. |image3| image:: https://kb.northwestern.edu/images/group293/76583/sasnewfolder.jpeg
   :width: 472px
   :height: 288px
.. |image4| image:: https://kb.northwestern.edu/images/group293/76583/sasfiles_shortcuts.jpeg
   :width: 325px
   :height: 160px
