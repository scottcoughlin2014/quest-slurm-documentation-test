Logging in to Quest
===================

The two commonly used methods for logging in to Quest are using the
FastX client and through a secure shell (SSH) terminal.

If you’ll be working with GUI (graphical window) applications, FastX is
recommended.

| The `Quest Analytics
  Nodes <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/quest-analytics-nodes.html>`__
  can be accessed directly via a web browser.

For a video tutorial please see `Logging Into
Quest <https://www.youtube.com/watch?v=xFYHs19gGjU>`__.

Connecting with FastX
~~~~~~~~~~~~~~~~~~~~~

| The FastX Desktop Client is an application for Microsoft Windows and
  Mac OS X personal computers that makes a network connection to Quest
  using SSH. It gives you a connection to the Quest Linux servers with
  full graphics support via the Linux X Windows system, also called X11.
  `Logging on using
  FastX <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1511>`__
  is recommended if you will be using GUI-based applications. If you are
  running Linux on your personal computer, please connect to Quest with
  SSH via the terminal.

Connecting with an SSH Terminal
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. container::

   *Please note that your netid/username is case-sensitive. For
   instance, using ABC123 instead of abc123 will not work in any of the
   methods below.
   *

Mac or Linux computer
^^^^^^^^^^^^^^^^^^^^^

To login to Quest from a Linux or Mac computer, open a terminal and at
the command line enter:

ssh -X <netid>@quest.northwestern.edu

Example

ssh -X abc123@quest.northwestern.edu

Windows computer
^^^^^^^^^^^^^^^^

Windows users will first need to download an ssh client, such as
`PuTTY <http://www.chiark.greenend.org.uk/~sgtatham/putty/>`__, which
will allow you to interact with the remote Unix command line. Use the
following information to set up your connection:

| **Hostname** : quest.northwestern.edu
| **Username** : your Northwestern NetID
| **Password** : your Northwestern NetID password
