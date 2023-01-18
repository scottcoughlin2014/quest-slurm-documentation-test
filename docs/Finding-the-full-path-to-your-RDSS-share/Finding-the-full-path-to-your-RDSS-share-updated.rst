Finding the full path to your RDSS share
========================================

This page describes how to format your RDSS/FSMResfiles filepath when
requesting access to Globus

To allow users to access RDSS via Globus, we first need to tell our data
transfer node where to look for your files. To do this, we need the full
path to your RDSS share.

Step 1: Identify your access zone
---------------------------------

Your access zone was determined when the share was created. Two of these
access zones are allowed to use Globus to transfer data to and from
Quest.

-  **fsmresfiles.fsm.northwestern.edu** is for Feinberg School of
   Medicine users
-  **resfiles.northwestern.edu** is for other unaudited shares

If you’re not sure which group you fall into, look at the address that
you use to `connect to your RDSS
share <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1536>`__.

If you are not in one of these access zones, you will not be able to use
Globus to transfer data.

Step 2: Identify the full path to your share
--------------------------------------------

The easiest way to find the path you want is to `connect to your RDSS
share <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1536>`__,
navigate to the folder that you want to access, and copy the path. For
detailed instructions, see the sections below.

Note: If you need access to multiple shares, repeat this process for
each share. Include the information for each share in your response to
`the
form <https://app.smartsheet.com/b/form/d563d02a9a6c4e28ba515fec4e5f6635>`__.

.. container:: panel panel-default

   .. container:: panel-heading

      Mac instructions

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      #. `Connect to your RDSS
         share <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1536>`__
      #. Navigate to the location of your sharein Finder
      #. With that folder open in Finder, type Command-I to open the
         “Info” Window about this folder, or right-click on the folder
         icon and choose “Get Info” from the menu
      #. In the window that pops up, under the “General” section, copy
         the path in the **Server** field. It will start with ``smb://``
         followed by your access zone, then a path to the share
         separated by slashes. See screen shot below.
      #. Paste this file path in `the
         form <https://app.smartsheet.com/b/form/d563d02a9a6c4e28ba515fec4e5f6635>`__
         to request Globus access.

      .. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/mac-share-path.png
         :alt: mac share path
         :width: 309px
         :height: 141px

      Examples:

      -  ``smb://resfiles.northwestern.edu/SHARENAME/``
      -  ``smb://fsmresfiles.fsm.northwestern.edu/fsmresfiles/DEPARTMENT/LAB_GROUP/``
      -  ``smb://fsmresfiles.fsm.northwestern.edu/fsmresfiles/COREFACILITY/``
      -  ``smb://fsmresfiles.fsm.northwestern.edu/fsmresfiles/CENTER/PROJECT/``

.. container:: panel panel-default

   .. container:: panel-heading

      Windows instructions

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      #. `Connect to your RDSS
         share <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1536>`__
      #. Open Windows Command prompt by clicking the Windows search bar
         and typing **cmd**
      #. Type the following command: ``net use``
      #. The output will look like the screen shot below
      #. Paste this file path in `the
         form <https://app.smartsheet.com/b/form/d563d02a9a6c4e28ba515fec4e5f6635>`__
         to request Globus access.

      .. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/windows-share-path.png
         :alt: windows share path in cmd prompt
         :width: 500px
         :height: 92px

      Examples:

      -  ``\\resfiles.northwestern.edu\SHARENAME\``
      -  ``\\fsmresfiles.fsm.northwestern.edu\fsmresfiles\DEPARTMENT\LAB_GROUP\``
      -  ``\\fsmresfiles.fsm.northwestern.edu\fsmresfiles\COREFACILITY\``
      -  ``\\fsmresfiles.fsm.northwestern.edu\fsmresfiles\CENTER\PROJECT\``

Multiple Shares
---------------

If you need to access multiple shares, please enter all relevant file
paths in the form.
