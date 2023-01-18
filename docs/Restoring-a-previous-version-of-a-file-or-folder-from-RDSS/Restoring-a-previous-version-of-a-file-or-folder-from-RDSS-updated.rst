Restoring a previous version of a file or folder from RDSS
==========================================================

This document explains how to to restore previous versions of a file or
folder in RDSS/FSMResFiles.

The Research Data Storage Service (RDSS) creates snapshots of files
stored on the system that are kept for 28 days. Snapshots occur daily
between 7 pm and midnight. This feature allows users to retrieve files
that were accidentally modified or deleted with some exceptions:

-  Files that were modified/deleted more than 28 days ago cannot be
   restored because snapshots are deleted after 28 days.
-  Files that were created then modified/deleted within 24 hours may not
   be able to be restored because they may not have been on the file
   system when a snapshot occurred.

Retrieving older versions of modified/deleted files
---------------------------------------------------

The instructions below describe how to restore older versions of files
or deleted files contained in snapshots on RDSS.

**Note**: This process can only be done on computers running the Windows
operating system. If you cannot mount your RDSS share on a Mac or Linux
machine, please contact consultant@northwestern.edu for assistance in
retrieving your files.

.. container:: panel panel-default

   .. container:: panel-heading

      View Previous Versions

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      To see the previous versions of a file that are available:

      #. **Open a**\ `File
         Explorer <https://support.microsoft.com/en-us/windows/windows-explorer-has-a-new-name-c95f0e92-b1aa-76da-b994-36a7c7c413d7>`__
         window
      #. Navigate to the file or folder you want to retrieve
      #. **Right-click** on the file/folder
      #. Select **Properties** from the menu that appears.
      #. In the properties window, select the **Previous Versions** tab.

      You should see a list of previous versions of the file/folder.

      .. image:: https://kb.northwestern.edu/images/group293/shared/previous_version.jpg
         :alt: Windows Previous Versions Tab
         :width: 386px
         :height: 449px

      Select the needed version of the folder and click the **Open**
      button below the list of versions to view its contents.

      Previous Versions. This content will be shown/hidden as the panel
      is toggled.]

.. container:: panel panel-default

   .. container:: panel-heading

      Restore a previous version

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      If you would like to revert to a previous version of a folder,

      #. In the **Previous Versions** tab you opened in the previous
         steps, click the version you want to revert to.
      #. Click the **Restore** button below the version list. This will
         restore the contents of the entire directory.
      #. You will see a message warning you that the current version of
         the file will be replaced. Click **Yes**.

      The current version of this directory will now match the version
      you selected above.

.. container:: panel panel-default

   .. container:: panel-heading

      Restore a deleted file

   .. container:: panel panel-body js-panelnormalswitches2 collapse

      If you would like to retrieve a deleted file, revert to a file to
      a previous version, or make a copy of the previous version,

      #. Navigate to the folder that the file was stored in.
      #. Right click the folder and selectProperties
      #. Click on thePrevious Versionstab
      #. In thePrevious Versionstab you opened in the previous steps,
         click the version you want to revert to.
      #. Click the **Open**\ button below the version list. This will
         open a new file explorer window with the previous version’s
         contents of that folder.
      #. You can copy the previous version of the file(s) you want and
         paste the file(s) into the current folder or new location.

If you need help with any of these instructions, email
consultant@northwestern.edu to speak with a data management specialist.
