Connecting to a Research Data Storage Service Share
===================================================

Research Data Storage Service provides campus researchers the ability to
subscribe to a minimum of one Terabyte (TB) of desktop mountable storage
for research purposes. This storage should be used when storing files
larger than 15 GB or when using regulated data, such as PHI, PII, etc.
This article gives instructions on how to connect to (mount) the storage
location to your desktop for various operating systems.

.. container:: panel panel-default

   .. container:: panel-heading

      Windows

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      .. rubric:: Connecting to your share
         :name: connecting-to-your-share

      -  Identify your server:

         -  If you are a member of Feinberg School of Medicine, the
            address is fsmresfiles.northwestern.edu
         -  If you are not in Feinberg, but have selected auditing
            capability, the address is resfilesaudit.northwestern.edu
         -  If you are not in Feinberg and have no auditing, the address
            is resfiles.northwestern.edu

      -  Open a Windows Explorer window:
      -  Type \\\\ followed by you server in the address bar
      -  Press enter. For example, if you fall into category c. from
         above, you will type: \\\resfiles.northwestern.edu
      -  You will be prompted to authenticate to the server. Enter your
         username as “ads\<NetID>” (replacing <NetID> with your actual
         NetID, do not include brackets), and enter your NetID password.
      -  Once connected, you will see the list of shares on the server.
         Select your share to copy (and write, if you have the
         appropriate privileges) files

      .. rubric:: Mapping your share as a network drive
         :name: mapping-your-share-as-a-network-drive

      -  From any Windows Explorer window, navigate to Tools -> Map
         Network Drive
      -  Pick a drive letter to use and enter the full path to the
         share. For example, if you are using drive letter T: and
         mounting the share called northwesternit_research from the
         server resfiles, you would type in the Folder:
         \\\resfiles.northwestern.edu\northwesternit_research
      -  Check “Reconnect at login” if you would like the drive to mount
         at login.
      -  Click “Connect using a different user name”. Enter your
         username as “ads\<NetID>” (replacing <NetID> with your actual
         NetID, do not include brackets), and enter your NetID password.

.. container:: panel panel-default

   .. container:: panel-heading

      MacOS

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      .. rubric:: Connecting to your share
         :name: connecting-to-your-share-1

      -  From the Desktop of your Mac computer, on the top Menu Bar
         select Go -> Connect to Server
      -  Identify your server:

         -  If you are a member of Feinberg School of Medicine, the
            address is
            smb://fsmresfiles.fsm.northwestern.edu/fsmresfiles
         -  If you are not in Feinberg, but have selected auditing
            capability, the address is
            smb://resfilesaudit.northwestern.edu
         -  If you are not in Feinberg and have no auditing, the address
            is\ smb://resfiles.northwestern.edu

      -  Enter in the server indicated for your storage allocation,
         click Connect
      -  Enter your NetID and password to authenticate
      -  If you are using a Mac that is not part of the FSM Domain, or
         your own personal machine, then you need to put “FSM\<NetID>”
         (replacing <NetID> with your actual NetID, do not include
         brackets)
      -  Select the share you would like to mount, click OK. This share
         will be given based on your research group name given

      .. rubric:: Mounting your share automatically at login
         :name: mounting-your-share-automatically-at-login

      -  Go to System Preferences -> Users & Groups
      -  Select your user account from the list on the left
      -  Click the Login Items tab on the right side of the window. This
         will show you all of the apps, scripts, documents, and user
         services that are configured to launch automatically when your
         user account logs in
      -  To add your network drive to this list, simply locate the
         network drive’s icon on your Desktop, and then drag and drop it
         into the Login Items list

.. container:: panel panel-default

   .. container:: panel-heading

      Linux

   .. container:: panel panel-body js-panelnormalswitches2 collapse

      .. rubric:: Connecting to your share
         :name: connecting-to-your-share-2

      -  Identify your server:

         -  If you are a member of Feinberg School of Medicine, the
            address is fsmresfiles.northwestern.edu
         -  If you are not in Feinberg, but have selected auditing
            capability, the address is resfilesaudit.northwestern.edu
         -  If you are not in Feinberg and have no auditing, the address
            is resfiles.northwestern.edu

      Mounting the share using the Ubuntu GUI:

      -  If using Ubuntu GUI – From the desktop, you may also SMB to the
         share via the default FILE MANAGER
      -  Select Connect to Server and then input your Research Data
         Storage share (smb://server\ \_name/share_name)
      -  Select Connect
      -  Username: “ads\<NetID>”; do not include brackets
      -  Password: NetID password

      Mounting the share from the command line:

      -  Make sure you have following:

         -  Your server name
         -  Root level access on Linux
         -  The share name that was granted to you when the share was
            created
         -  CIFS installation (type sudo apt-get install cifs-utils or
            on older systems: sudo apt-get install smbfs)

      -  Login as root to your linux machine
      -  Create the required mount point (For example: # mkdir -p
         /mnt/mountpoint)
      -  Use the mount command as follows: # mount -t cifs -o
         username=<NetID>,password=<NetID
         password>,domain=ads//resfiles.northwestern.edu/sharename
         /mnt/resfiles.northwestern.edu; do not include brackets for
         NetID or NetID password
      -  Access the share using cd and ls command: # cd /mnt/mountpoint;
         ls -lWhere:

         -  -t smbfs : File system type to be mount (outdated, use cifs)
         -  -t cifs : File system type to be mount
         -  -o : are options passed to mount command, in this example
            three options are used (NetID, NetID password, and ADS
            domain) (//server_name/share_name or /mnt/mountpoint Linux
            mount point (to access share after mounting))
