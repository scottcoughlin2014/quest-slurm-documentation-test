Disconnecting from a RDSS/FSMResFiles share
===========================================

This document describes how to disconnect your RDSS/FSMResFiles from
your computer.

**Select your operating system below for specific instructions.**

.. container:: panel panel-default

   .. container:: panel-heading

      MacOSX

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      #. Open a Finder window
      #. Locate your mounted file share on the left side of the window.
         Depending on where your share is located, it will be called one
         of the following:

         -  resfiles.northwestern.edu
         -  fsmresfiles.northwestern.edu
         -  resfiles-audit.northwestern.edu

      #. Click the eject button to the right of the mounted drive.

      .. image:: https://kb.northwestern.edu/images/group293/118991/mac-disconnect.png
         :alt: MacOS finder window with eject button highlighted on RDSS
         share
         :width: 300px

.. container:: panel panel-default

   .. container:: panel-heading

      Windows

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      #. Open the File Explorer
      #. Locate your mounted network drive in the navigation pane on the
         left side of the File Explorer window. The mounted drive will
         have a drive letter assigned to it (R:, Z:, etc.) and it will
         be called one of the following:

         -  SHARE_NAME (\\resfiles.northwestern.edu)
         -  SHARE_NAME (\\fsmresfiles.northwestern.edu)
         -  SHARE_NAME (\\resfiles-audit.northwestern.edu)

      #. To unmount the drive, use the right mouse button on the mounted
         drive to bring up the contextual menu and select Disconnect

.. container:: panel panel-default

   .. container:: panel-heading

      Linux

   .. container:: panel panel-body js-panelnormalswitches2 collapse

      .. rubric:: Requirements
         :name: requirements

      You must have root level access to your machine.

      #. Login as root to your linux machine
      #. From the command line, use the mount command to output a list
         of your mounted drives. Depending on where your share is
         located, it will be called one of the following:

         -  <netid>@resfiles.northwestern.edu on /mountpoint
         -  <netid>@ fsmresfiles.northwestern.edu on /mountpoint
         -  <netid>@ resfiles-audit.northwestern.edu on /mountpoint

      Where <NetID> is your Northwestern NetID and mountpoint is the
      mountpoint you created when you mapped the network drive.

      3. To unmount, type umount /mountpoint, replacing /mountpoint with
      the local directory that your share is mounted to.

Email consultant@northwestern.edu if you have questions or need help
using RDSS.
