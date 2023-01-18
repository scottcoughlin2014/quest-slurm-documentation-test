Connecting to Quest with FastX
==============================

How to connect to the Quest High Performance Computing Cluster using the
FastX client.

The `FastX3 Desktop
Client <https://starnet.com/download/fastx-client>`__ is an application
for Microsoft Windows, Linux and Mac OS X personal computers that makes
a network connection to Quest using SSH. It connects you to the Quest
Linux servers with full graphics support via the Linux X Windows system,
also called X11. FastX3 is made available to members of the Northwestern
community under a campus site license.

.. container::

   FastX3 is *not* required for `connecting to
   Quest <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__.
   You are welcome to use other SSH clients you may already be familiar
   with in your operating system. FastX3 is provided as an option with
   convenient features such as using terminal and display in one
   application, remote desktops and ability to pause and resume
   sessions.

   You will only need to follow the configuration steps the first time
   you connect to Quest. After FastX3 is configured, you can use it to
   connect to Quest at any time.

   For a video tutorial on using FastX to login to Quest, please see
   `Logging Into Quest <https://www.youtube.com/watch?v=xFYHs19gGjU>`__.

   FastX3 Web Access to Quest: While on the Northwestern GlobalProtect
   VPN, users wishing to connect to Quest without installing the FastX3
   client or a terminal program can use the web browser-based FastX3
   connection at
   `http://quest.northwestern.edu:3300 <https://quest.northwestern.edu:3300>`__.

   FastX3 Web Access to KLC: While on the Northwestern GlobalProtect
   VPN, users wishing to connect to KLC without installing the FastX3
   client can use the following `FastX Browser Instructions for
   KLC. <https://www.kellogg.northwestern.edu/research-support/computing/kellogg-linux-cluster/connect.aspx>`__

   **Install and Configure FastX3**

   -  `Download and Install FastX3 <#config1>`__
   -  `Run FastX3 for the First Time <#config2>`__
   -  `Create a Quest Connection <#config3>`__

   **Connect with FastX3**

   -  `Open a Connection to a Quest Login Node <#connect1>`__
   -  `Start a Session <#connect2>`__
   -  `Disconnecting and Reconnecting Sessions <#connect3>`__
   -  `Terminating Sessions <#connect4>`__

   --------------

   .. rubric:: **Install and Configure FastX3
      **
      :name: install-and-configure-fastx3

   You only need to do the steps in this section once on each computer
   you want to use to connect to Quest.

   .. rubric::  Step 1: Download and Install FastX3
      :name: step-1-download-and-install-fastx3

   **Windows:**

   #. *Download* the `FastX3
      Client <https://starnet.com/download/fastx-client>`__ to your PC.
   #. *Run* the installer named **FastX-3.X.XX-setup.exe**.

   **Mac OS X** :

   .. container::

      #. *Download* the `FastX3
         Client <https://starnet.com/download/fastx-client>`__ to your
         Mac.
      #. *Open* the disk image named **FastX3-3.X.XX.dmg**.
      #. *Drag* **FastX3** into your **Applications** folder.

   **Linux** :

   .. container::

      #. *Download* the `FastX3
         Client <https://starnet.com/download/fastx-client>`__ to your
         Linux.
      #. *Open* the tarball file named
         **FastX3-3.X.XX.rhel7.x86_64.tar.gz**.
      #. *Extract* FastX3 folder from the tarball to your home or
         Desktop folder.

   .. rubric::  Step 2: Run FastX3 for the First Time
      :name: step-2-run-fastx3-for-the-first-time

   **Windows** :

   Use the **Start Menu (**\ *Click* **Start Menu > All Programs >
   FastX3**) or use the **Start Screen** to find the **FastX3**
   application and *Run* it. When prompted, allow FastX3 to communicate
   through the firewall.

   **Mac OS X** :

   #. In the Finder, *open* the **Applications** folder and *locate*
      **FastX3**.
   #. *Press* the **Control-key** and *click* the **FastX3** icon.
   #. *Choose* **Open** from the pop-up menu.
   #. *Click* **Open**.

   **Linux** :

   #. *Open* FastX3 folder and *locate* **FastX3** executable file.
   #. *Double-click* **FastX3** executable file.

   Note: FastX3 will be stored as an exception to your security settings
   and you will subsequently be able to run it by *double-clicking* the
   FastX3 icon. You will have to do this each time you upgrade FastX3.

   .. rubric::  Step 3: Create a Connection
      :name: step-3-create-a-connection

   FastX3 uses SSH to connect to a Quest login node.

   *1. Click* the **PLUS** icon in the upper left corner of the window.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/FastX3-Application-Launch.png
      :alt: FastX3-Application-Launch
      :class: fr-fic fr-dii

   *2.* An **Edit Connection** window will open. You must already have
   an account on Quest to login. Fill in the following fields:

   #. **Host**: quest.northwestern.edu
   #. **User:** *Enter* **your own NetID** in lower-case letters.\ **
      **
   #. **Port**: 22
   #. **Name**: Quest (*Can be whatever you want though it is for your
      own reference*)

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/FastX3-Create-Connection.png
      :alt: FastX3-Create-Connection
      :class: fr-fic fr-dii

   *4. Click* **OK**.

   .. rubric:: **Connect to Quest
      **
      :name: connect-to-quest

   This section shows you how to create a connection to Quest each time
   you want to connect and how to manage your connections.

   .. rubric::  Step 1: Open a Connection to a Quest Login Node
      :name: step-1-open-a-connection-to-a-quest-login-node

   1. With FastX3 open, *double-click* My Quest Connection in the
   **Connections** window.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/FastX3-Connections.png
      :alt: FastX3-Connections
      :class: fr-fic fr-dii

   | 2. A window will pop up.If this is the first time you have logged
     into Quest, a question like the one below will show up and will ask
     if you are sure that you want to continue, say *yes.*
   | Every time after, a screen will pop up which only requests that you
     *Enter* your **NetID password**. Then *click* **Continue**.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/want-to-continue.png
      :alt: want-to-continue
      :class: fr-fic fr-dii

   FastX will open an SSH connection to Quest and open a Session
   Management window for Quest. You will have no sessions to begin with,
   so the window will appear to be empty.

   .. rubric::  Step 2: Start a Session
      :name: step-2-start-a-session

   *1. Click* the **PLUS** icon in the upper left corner of the Session
   Management window to *create* a new Session.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/FastX3-Plus-Button.png
      :alt: FastX3-Plus-Button
      :class: fr-fic fr-dii

   2. A new window will appear with icons. We recommend *selecting* the
   **GNOME** icon from this menu. Below, we select **GNOME** and the
   **Command** box will change to start a single application named,
   **gnome-session**. *Click* **OK**.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/Application-Selection-Menu-fastX.png
      :alt: ApplicationSelectionMeny
      :width: 510px
      :height: 400px

   3. Your **GNOMEVirtual Desktop** window will open, and your **Session
   Management** window will change to show that you have a single
   session named, **GNOME**.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/Active-GNOME-Desktop.png
      :alt: GNOME-Desktop
      :width: 663px
      :height: 330px

   To launch a terminal inside of your GNOME virtual desktop, click
   *Activities* -> *Terminal*. This will give you a traditional `GNOME
   Terminal <http://help.gnome.org/users/gnome-terminal/stable/>`__
   session running a Bash shell; it provides full graphics support via
   X11. Type Linux commands in this window. Manage batch jobs. Launch
   programs with graphical interfaces. Edit your files with the **evim**
   GUI. Explore the menu bar items to learn more about the features of
   GNOME Terminal. GNOME Desktop is the recommended way to connect to
   Quest.

   .. rubric::  Step 3: Disconnecting and Reconnecting Sessions
      :name: step-3-disconnecting-and-reconnecting-sessions

   Running sessions are listed in the FastX Session Management window.
   Sessions continue to run on the login host until you terminate them.

   You can disconnect from a running session without terminating it by
   *clicking* the **Person Icon** on the upper left corner of the
   session and then *selecting* **Disconnect** from the menu. You might
   want to disconnect in your office, go home, and then reconnect. You
   can reconnect to a paused session by *clicking* on the big “play”
   button on the session box.

   Note that the paused FastX session remains on the login node that you
   have initially connected. If you connect to another login node next
   time, you will not see your paused session.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/FastX3-Pause-Resume.png
      :alt: Pausing-Resuming-FastX3
      :class: fr-fic fr-dii

   .. rubric:: Optional: Create your own Bookmark
      :name: optional-create-your-own-bookmark

   If you would like to create your own FastX shortcut, you can do so by
   creating your own *Bookmark*. First, you will need to launch the
   bookmarks window by going to *View* -> *Bookmarks*.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/Launch-Bookmark-Window.png
      :alt: launch-bookmark
      :width: 488px
      :height: 292px

   Next, in the Bookmarks window, please select *New* this will create a
   new line where you can name your bookmark, and write the command
   associated with this Bookmark.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/Make-my-bookmark-screen.png
      :alt: find-bookmark
      :width: 492px
      :height: 322px

   In this example, we name our book mark *RStudio* which will run the
   command, ``module load R/4.1.1; rstudio``.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/Custom-RStudio-Bookmark.png
      :alt: custom-rstudio
      :width: 496px
      :height: 324px

   After closing the bookmarks window and returning to the application
   launch menu, you will see a new section called *My Bookmarks*.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/Launch-Application-Global-My-Bookmark.png
      :alt: fastx-with-my-bookmark
      :width: 500px
      :height: 393px

   When you select *My Bookmarks* you will see an icon called RStudio
   which, when selected, will show the command
   ``module load R/4.1.1; rstudio``.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/Launch-Application-My-Bookmark.png
      :alt: launch-app-after-clicking-my-bookmark
      :width: 496px
      :height: 390px

   Once you select *OK*, FastX will run these commands which will launch
   RStudio on one of the Quest log-in nodes.

   .. image:: https://kb.northwestern.edu/images/group293/69237/FastX3/result-of-rstudio-bookmark.png
      :alt: rstudio-bookmark-main-screen
      :width: 508px
      :height: 396px
