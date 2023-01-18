Using the RDSS Globus Endpoint
==============================

This document describes how to transfer data to/from the Research Data
Storage Service (RDSS) and FSMResfiles using Globus.

The Northwestern Quest RDSS endpoint allows you to transfer data between
RDSS/FSMResfiles and Quest. Currently, all data to/from RDSS must go
through Quest due to current system architecture.

Setup
-----

To use the RDSS endpoint you must:

#. **Have a Quest Allocation**: Follow the linked instructions to
   `request a Quest
   allocation <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__.
#. **Have access to an RDSS/FSMResFiles Allocation**: See the linked
   page for `more information about
   RDSS <https://www.it.northwestern.edu/service-catalog/research/data-storage/rdss.html>`__
#. **Request access to RDSS/FSMResFiles via Globus**: Send an email to
   quest-help@northwestern.edu with the subject line “RDSS Globus
   Access”. Include the following in the body of your email

   -  your NetID
   -  The path of the RDSS share you would like to access via Globus

Mounting your RDSS/FSMResFiles share
------------------------------------

Once these steps are complete, mount RDSS on Quest. To do this, make
sure you are either on campus, or are connected to `GlobalProtect
VPN <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1818>`__.
Then, open your `terminal program of
choice <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__
and type the following command

.. container::

   ``ssh <netid>@qglobus12.ci.northwestern.edu``

   Where you replace ``<netid>``\ with your Northwestern NetID. Enter
   your NetID password when prompted.

   This operation will automatically mount your RDSS/FSMResFiles share.
   The share will remain mounted after you end your terminal session.

   Note: You will need to repeat this process after Quest downtime.

   .. rubric:: Activating the endpoint
      :name: activating-the-endpoint

   To activate the RDSS endpoint:

.. container::

   Open https://app.globus.org/file-manager in the browser and log in as
   a Northwestern user.

   ThenSearch for the the RDSS endpoint “Northwestern Quest RDSS”.

.. container::

   .. image:: https://kb.northwestern.edu/images/group293/71271/select-RDSS-endpoint.png
      :alt: Searching for and selecting RDSS endpoint in Globus file
      manager
      :width: 500px
      :height: 276px

When you select this endpoint, Globus will then ask you to authenticate
using your Northwestern NetID and password

.. image:: https://kb.northwestern.edu/images/group293/71271/authenticate-rdss.png
   :alt: Authentication screen for RDSS
   :width: 500px
   :height: 408px

Then, you should see the /rdss/ directory, which contains folders with
users’ NetIDs.

To navigate to access your RDSS/FSMResFiles content, navigate into the
folder with your NetID as the name. Inside that folder, you will see a
folder called “resfiles”, “fsmresfiles”, or “resfiles-audit”, depending
on where your allocation is located. Navigate into that folder. You
should see the files contained in the directory that you requested
access to in the Setup section.

We recommend bookmarking this folder to avoid having to navigate to this
folder every time you initiate a file transfer. To create a bookmark,
select the bookmark icon to the right of the path you would like to
bookmark. The icon will appear yellow if the path is bookmarked.

.. image:: https://kb.northwestern.edu/images/group293/71271/globus-bookmarks.png
   :alt: Bookmark a globus endpoint
   :width: 700px
   :height: 157px

Transferring files to/from RDSS\ **/FSMResFiles**
-------------------------------------------------

The Northwestern Quest RDSS endpoint only allows data transfers within
RDSS and to the Northwestern Quest managed endpoint.

-  **To transfer to Quest**: No staging is necessary.
-  **To transfer anywhere else**: To transfer files from
   RDSS/FSMResfiles to any other endpoint, first transfer the data to
   Quest. Your Quest allocation must have enough storage space to
   accommodate the files that you’re transferring. Then, you can
   transfer the files to any other Globus endpoint.

Extending RDSS\ **/FSMResFiles** endpoint activation
----------------------------------------------------

Northwestern Quest RDSS endpoint activation on Globus expires after 1
day by default. Once it expires, you must reauthenticate.

To extend the activation period:

First, on the Globus website, find the Northwestern Quest RDSS Endpoint
in the File Manager window.

Then, select the Manage Activation icon that looks like a power button
at the bottom of the column between the file browser windows (red box)

.. image:: https://kb.northwestern.edu/images/group293/71271/extend-rdss-activation-1.png
   :alt: extend rdss activation page, red box around button to open page
   :width: 300px
   :height: 380px

Click Extend Activation. Globus will ask you to reauthenticate with your
Northwestern NetID

**BEFORE YOU AUTHENTICATE**, click Advanced under the password field.
This will allow you to specify how long you would like the endpoint to
remain activated up to 168 hours (7 days). Enter the amount of time you
would like to extend the activation for in hours.

.. image:: https://kb.northwestern.edu/images/group293/71271/extend-rdss-activation-2.png
   :alt: Illustration of how to click the advanced button to extend
   activation time
   :width: 700px
   :height: 269px

If you would like to create a stable endpoint that does not expire, you
can create a Shared Collection on the RDSS Endpoint. See Globus’s `How
to Share Data Using Globus
documentation <https://docs.globus.org/how-to/share-files/#:~:text=Log%20into%20Globus%20and%20navigate,in%20the%20right%20command%20pane.&text=Sharing%20is%20available%20for%20folders.>`__
for more information.

Troubleshooting
---------------

Here are some common issues with using the Quest RDSS Globus endpoint
and how to resolve them.

I get a “permission denied” error when I try to access my files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Seeing a “permission denied” error when trying to access a folder on the
Quest RDSS endpoint (below) means that Globus does not think you have
permission to see the folder you’re trying to access.

.. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/globus-Permission-denied.png
   :alt: Globus permission denied error
   :width: 500px
   :height: 349px

The two most common reasons for this are:

Reason 1: You don’t have access to the file or folder on RDSS/FSMResFiles
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To check whether you have access on RDSS/FSMResFiles, `connect to your
shared drive on your
computer <https://kb.northwestern.edu/connect-rdss>`__ and see if you
have access. If you do not,

-  RDSS users should contact the owner of your share. They can grant you
   access using our `self-service
   tool <https://kb.northwestern.edu/managing-rdss-access>`__.
-  FSMResFiles users should contact FSM-IT (fsmhelp@northwestern.edu)
   for access.

Reason 2: Your RDSS share was unmounted from the Quest RDSS Globus node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Occasionally, the Quest RDSS Globus (qglobus02) node has to be restarted
for system maintenance. Our current system architecture requires users
to log in to the qglobus02 node to remount the share. Please see
instructions in the “Mounting your RDSS/FSMResFiles share” section above
for details.

If neither of these methods solve your problem, email
quest-help@northwestern.edu and we will help you troubleshoot.

I can’t get to the folder I need in the Globus interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When we set up access to the Quest RDSS Globus endpoint for users, we
have to choose a mountpoint, which determines what you see inside your
NetID folder in Globus. For example, if you need access to multiple
shares in the resfiles zone or multiple lab groups/departments in
FSMResFiles, we set your mountpoint to the folder that contains these
folders. If you can only see one of those shares/folders, we will have
to change your mountpoint.

To request this change, email quest-help@northwestern.edu with the
following information:

-  Your NetID
-  Where you want your mountpoint to be OR the filepaths you need to see
   in Globus. We can help you figure out the correct mountpoint if you
   give us all the locations you need access to.

My file transfer fails when I try to transfer files between RDSS and my personal endpoint
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Our current infrastructure only allows transfers between the RDSS and
Quest endpoints. You can transfer your data to Quest and then transfer
your data to your personal endpoint or any other Globus endpoint. Do not
use Quest to store sensitive data even temporarily.

Email quest-help@northwestern.edu if you need help setting up this
two-stage transfer, don’t have enough storage space on Quest, or are
working with sensitive data that cannot be stored on Quest.
