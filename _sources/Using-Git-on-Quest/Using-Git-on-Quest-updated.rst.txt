Using Git on Quest
==================

Git is an open source, distributed version control system that is
commonly used for collaboration on code or documents. While it is often
used with a hosting service like GitHub, Git can be used locally to
track changes within a single repository stored on disk.

Making Git Available
--------------------

Git is available as part of the operating system, but that version of
Git is old. It is strongly recommended that you load a newer version of
Git with the git
`module <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1550>`__
before using Git:

module load git

Setting up Git
--------------

Before you use Git for the first time on Quest, you need to set the user
name and email address it will use when it records your commits. Use the
following commands to set this information:

.. code:: code

   git config --global user.name "John Doe"
   git config --global user.email johndoe@northwestern.edu

You only need to do this one time. Git will remember your name and
email.

To view your Git settings, at any time you can run this command:

.. code:: code

   git config --list

Repositories
------------

You can create repositories at any time in any directory on Quest you
have write access to with the ``git init`` command. However, if you wish
to clone a repository from a remote server such as GitHub or Bitbucket,
you have a choice of using either HTTPS or SSH to clone.

Cloning from GitHub with HTTPS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| To clone a repository from GitHub on Quest using the HTTPS protocol,
  click the “Clone or download” button:
| |Clone or Download button|

| Then copy the URL that appears below it:
| |Git clone HTTP URL example|

Finally, in your Quest terminal, run this command (pasting in the URL
you copied):

.. code:: code

   git clone https://github.com/<organization>/<repository>.git

That will download the repository from GitHub into a directory with the
same name as the repository.

If the repository is private (and you have access to it), you will be
asked to provide your GitHub username and password. Similarly, if you
push to a repository you have cloned from GitHub using the HTTPS
protocol, you will have to provide your username and password. Because
it is annoying to have to type your username and password over and over
again, Git provides a way to cache your credentials.

Type the following command to enable username and password caching:

.. code:: code

   git config --global credential.helper cache

By default Git will remember your credentials for 15 minutes. To set a
different timeout (in seconds), use a command like this one:

.. code:: code

   git config --global credential.helper 'cache --timeout=3600'

This will allow Git to remember your credentials for one hour.

Cloning from GitHub with SSH
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To clone a repository from GitHub on Quest using the SSH protocol, you
must first load your SSH key from Quest into GitHub.

First, in your Quest terminal, print out the contents of your SSH public
key:

.. code:: code

   cat ~/.ssh/id_rsa.pub

| This will display several lines of text. Copy it to your computer’s
  clipboard, then go to GitHub and click your profile in the upper right
  corner, then click “Settings”:
| |GitHub Settings link|

| In the left sidebar, click “SSH and GPG Keys”
| |GitHub SSH Keys menu link|

| Click “New SSH Key”
| |New SSH Key button|

In the “Title” field, enter a descriptive name for the key, such as
“Quest”. Paste the public key you copied earlier into the “Key” field,
then click the “Add SSH Key” button. If prompted, confirm your GitHub
password.

Now you are able to clone using the SSH protocol.

| On the page of a repository you want to clone on GitHub, click “Clone
  or Download”.
| |GitHub Clone or Download Button|

| Then click the “Use SSH” link.
| |GitHub Clone via SSH Link|

| Next, copy the repository URL.
| |GitHub copy SSH repository URL|

Finally, in your Quest terminal, run this command (pasting in the URL
you copied):

.. code:: code

   git clone git@github.com/<organization>/<repository>.git

That will download the repository from GitHub into a directory with the
same name as the repository. You will now be able to push and pull from
this remote repository without having to enter a username and password.

Git Resources
-------------

There are many resources for learning Git on the web. See the `Research
Computing Git Workshop page <https://github.com/nuitrcs/gitworkshop>`__
for a list of resources.

.. |Clone or Download button| image:: https://kb.northwestern.edu/images/group293/78598/clone-1.png
   :width: 800px
   :height: 94px
.. |Git clone HTTP URL example| image:: https://kb.northwestern.edu/images/group293/78598/clone-2-https.png
   :width: 350px
   :height: 202px
.. |GitHub Settings link| image:: https://kb.northwestern.edu/images/group293/78598/userbar-account-settings.png
   :width: 227px
   :height: 381px
.. |GitHub SSH Keys menu link| image:: https://kb.northwestern.edu/images/group293/78598/settings-sidebar-ssh-keys.png
   :width: 261px
   :height: 104px
.. |New SSH Key button| image:: https://kb.northwestern.edu/images/group293/78598/new-ssh-key.png
   :width: 211px
   :height: 91px
.. |GitHub Clone or Download Button| image:: https://kb.northwestern.edu/images/group293/78598/clone-1.png
   :width: 800px
   :height: 94px
.. |GitHub Clone via SSH Link| image:: https://kb.northwestern.edu/images/group293/78598/clone-2-ssh.png
   :width: 400px
   :height: 202px
.. |GitHub copy SSH repository URL| image:: https://kb.northwestern.edu/images/group293/78598/clone-3-ssh.png
   :width: 400px
   :height: 198px
