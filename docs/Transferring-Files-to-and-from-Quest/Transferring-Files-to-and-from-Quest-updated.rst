Transferring Files to and from Quest
====================================

This page provides information on transferring files to and from Quest.

Quest supports several secure file transfer protocols and systems for
transferring files to and from Quest. *We strongly recommend using the
Globus client if you want to transfer files to or from a Windows, Linux,
or Mac computer.*

Transferring Data To and From Quest and ….

-  `Personal Laptop or Workstation   <#PersonalLaptop>`__
-  `Research Data Storage Service or FSMRESFILES   <#RDSS-FSMRES>`__
-  `OneDrive/SharePoint   <#onedrive>`__
-  `Amazon S3   <#amazon-s3>`__
-  `Google Cloud   <#google-cloud>`__

Globus
------

`Globus <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1557>`__
is a Software-as-a-Service (SaaS) that provides a program interface for
file transfer and sharing, as well as identity, profile, and group
management. You can use Globus to transfer very large data sets to and
from Quest. It provides a high performing and secure method to transfer
data between endpoints. A Globus transfer handles all the difficult
aspects of data transfer by optimizing bandwidth usage, managing
security configurations, providing automatic fault recovery, and
notifying users of completion and problems. Northwestern University
affiliated users can use Globus to transfer data between
Northwestern-controlled endpoints (such as Quest storage and the
Research Data Storage Service) and any other endpoint they have access
to, including a personal endpoint that can be set up on a laptop or
workstation. More information about Globus data transfer can be found at
https://www.globus.org/data-transfer.

Personal Laptop or Workstation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Solution: `Personal Globus
Endpoint <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1557>`__

Research Data Storage Service or FSMRESFILES
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Solution: Globus can be used to transfer files to and from the Research
Data Storage Services and FSMRESFILES. When transferring files to/from
the Research Data Storage Service or FSMRESFILES, please avoid the use
of symbolic links on Quest. This results in duplicate copies of your
data on the Research Data Storage Service and FSMRESFILES. In order to
transfer files between Quest and FSMRES via Globus, you must be added to
the fsmresfiles/Globus access list. Please contact
quest-help@northwestern.edu your netid and your full share name so we
can add you to the list.

Example of the full share name/path:  

``\\fsmresfiles.fsm.northwestern.edu\fsmresfiles\path\to\your\share\  ``

After we have added you, you can follow the instructions below to enable
Globus to transfer data to and from FSMRESFILES and Quest.

#. SSH to ``qglobus02.it.northwestern.edu`` using your NetID as your
   username. (Note: you must be on the campus network or logged into
   `GlobalProtect
   VPN <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1818>`__
   to access this node.) This operation will automatically mount your
   RDSS share. There is no need to keep the SSH connection open, so you
   may exit at any time.  
#. Open https://app.globus.org/file-manager in the browser and log in as
   a Northwestern user.  
#. The Quest endpoint name is “Northwestern Quest”. the RDSS endpoint
   (for RESFILES and FSMRESFILES shares) is “Northwestern Quest RDSS”.

OneDrive/SharePoint
~~~~~~~~~~~~~~~~~~~

Solution: We are currently piloting a OneDrive/Sharepoint Globus
Endpoint.

See our `help
documentation <https://kb.northwestern.edu/globus-onedrive-sharepoint>`__
for how to get started.

Please reach out to quest-help@northwestern.edu if you would like to
know more about how to access this endpoint.

Amazon S3
~~~~~~~~~

Solution: `Using Globus Online with Amazon
S3 <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1968>`__

Google Cloud:
~~~~~~~~~~~~~

Solution: The Google Cloud SDK is installed system-wide on Quest. To
load this package, run:

::

   module load gcloud/322.0.0

Following the instructions on this page, `Google Cloud SDK Quickstart
Linux <https://cloud.google.com/sdk/docs/quickstart-linux>`__, you can
configure the Google Cloud SDK with your credentials, etc.

Other Data Transfer Options
---------------------------

Web Browser
~~~~~~~~~~~

For example, an option for transferring Quest storage and OneDrive would
be to connect to Quest with the FastX client and start a Gnome desktop
session or a Gnome terminal session. Then launch a terminal (or use the
terminal that is already launched, if you pick a Gnome terminal session)
and type

``firefox``

to launch the firefox browser. In that browser you can log into OneDrive
and transfer files.

Documentation on how to use FastX to connect to Quest is available here:
`Connecting to Quest with
FastX <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1511>`__.

We recommend that for large data transfers or repeated transfers to use
the Globus OneDrive connector when it becomes available.

Secure Copy (SCP)
~~~~~~~~~~~~~~~~~

You can use Secure Copy to transfer individual files to or from Quest
using the Linux or Mac OSX Terminal application on the computer you want
to transfer files to/from. If you are using a Windows system, you will
need `PuTTY <http://www.putty.org/>`__; Windows users substitute the
command ``pscp`` for ``scp`` in the examples below.

These commands are executed from your computer, NOT from Quest (your
Terminal prompt should reference your computer, not Quest).

| To transfer files from your computer to Quest:
| ``scp /PathToSourceFile/file NetID@quest.it.northwestern.edu:/PathToTargetDir/file``
| Example:
| ``scp /Users/willie/myfile.txt abc123@quest.it.northwestern.edu:/home/abc123/myfile.txt``

| To transfer files from Quest to your computer:
| ``scp NetID@quest.it.northwestern.edu:/PathToSourceDir/file /PathToTargerDir/file``
| Example:
| ``scp abc123@quest.it.northwestern.edu:/home/abc123/myfile.txt .``
| This would copy the file ``/home/abc123/myfile.txt`` from Quest to the
  current working directory on your computer with the same name
  (myfile.txt).

For more examples of Secure Copy, see `In Unix, how do I use SCP to
securely transfer files between two
computers? <https://kb.iu.edu/d/agye>`__ Note that ``scp`` will
overwrite existing files without prompting.

wget
~~~~

You can download datasets directly to Quest from external ftp and web
servers using the ``wget`` command (see the `Wget
manual <https://www.gnu.org/software/wget/manual/wget.html>`__).

| While connected to Quest, you could use commands like:
| ``wget http://www.nber.org/nberces/bbg96_87.dta``
| ``wget ftp://ftp.bls.gov/pub/doc/ap.txt``
| To copy files into your current working directory on Quest.

With additional options ``wget`` can also download whole directories of
files recursively and download from password-protected servers.
