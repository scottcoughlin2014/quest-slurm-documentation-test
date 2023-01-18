Using the OneDrive/SharePoint Globus Endpoint
=============================================

This document describes how to transfer data to/from OneDrive/SharePoint
using Globus.

The Northwestern Quest OneDrive Pilot endpoint allows users to access
files from both OneDrive and Sharepoint.

Setup
~~~~~

All Northwestern faculty, staff, and students have access to OneDrive.
No further authorization is needed to use Globus with OneDrive.

To access files from a Northwestern SharePoint document library, you
need to:

-  **Check that you have access**. Generally, if you can see the content
   in OneDrive, you will be able to see it in Globus.

   -  Check for access to entire SharePoint document libraries in the
      Quick Access section of the left OneDrive sidebar.
   -  Check for access to specific files/folders shared with you in the
      “Shared” section on the left OneDrive sidebar.

.. image:: https://kb.northwestern.edu/images/group293/71271/globus-sharepoint-library.png
   :alt: left sidebar of onedrive interface
   :width: 250px
   :height: 305px

-  **Follow the SharePoint library**. To follow a library in SharePoint,
   click the star next to the library in
   https://nuwildcat.sharepoint.com/

Once you have access and follow the SharePoint site, you will be able to
access the files using Globus.

Activating the OneDrive/Sharepoint Endpoint
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To activate the OneDrive/Sharepoint endpoint,

#. First `log into Globus using your Northwestern
   credentials <https://docs.globus.org/how-to/get-started/#log_in_with_an_existing_identity>`__.
#. Then search for Northwestern Quest OneDrive Pilot and select the
   endpoint. Globus will then ask you to authenticate with your
   Northwestern Microsoft account via the Northwestern Online Passport.
#. Once you authenticate, you will not need to provide your credentials
   again to activate this endpoint.

Accessing files
~~~~~~~~~~~~~~~

After authenticating, you should see the files contained in your
OneDrive account. To see other files that are shared with you via
OneDrive or SharePoint, click the up arrow above the files listed in
your OneDrive account.

.. image:: https://kb.northwestern.edu/images/group293/71271/globus-OneDrive-pilot-nav-up.png
   :alt: Globus interface showing OneDrive endpoint with red box around
   upward arrow to go up a directory
   :width: 500px
   :height: 302px

Here you should see 3 folders:

-  **/My files/** contains OneDrive files that you own
-  **/Shared/** contains files that have been shared with you
-  **/Shared libraries/** contains SharePoint libraries from SharePoint
   sites that you follow.

.. image:: https://kb.northwestern.edu/images/group293/71271/globus-OneDrive-pilot-root.png
   :alt: Globus file manager showing onedrive connector root directory
   :width: 500px
   :height: 405px

You can navigate to the files you would like to transfer from here.

Troubleshooting
---------------

See the following sections for instructions on how to resolve common
problems with the Northwestern Quest OneDrive Pilot endpoint.

My file transfer fails because of a checksum error when I try to send a Microsoft Office Document to a SharePoint library.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, SharePoint changes the metadata on MS Office files when they
are moved into a SharePoint library. This change to the metadata changes
the file checksum that Globus relies on to check file integrity. Here
are two options to resolve this issue:

#. **Deactivate the checksum verification feature**: This is the
   simplest way to work around this issue, however you risk missing file
   integrity problems that are unrelated to SharePoint changing the file
   metadata. We do not recommend using this option if you are
   transferring large files, large batches of files, and non-MS Office
   documents.

   -  Click on *Transfer & Timer Options* in the middle of the File
      Manager window.
   -  Then, check the box next to “do NOT verify file integrity after
      transfer”.
   -  Your transfer should complete without the checksum error

#. **Turn off the SharePoint feature that changes the metadata**: This
   process has to be done using the SharePoint API by an administrator.
   Please contact consultant@northwestern.edu with the subject "
   SharePoint Globus checksum error" for help with this process.

I can’t see a SharePoint Library I have access to.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you do not see a SharePoint Library you have access to in the Shared
Libraries folder, please check that you are following the Library by
clicking the star next to the library at
https://nuwildcat.sharepoint.com/.

If you are following the library and still don’t see it in Shared Libraries,
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  Make sure that the site you are following has a document library that
   contains files
-  Try unfollowing and refollowing the SharePoint site.

If none of these strategies work, email consultant@northwestern.edu with
the subject “Can’t see SharePoint library in Globus” and we will help
you troubleshoot the issue further.
