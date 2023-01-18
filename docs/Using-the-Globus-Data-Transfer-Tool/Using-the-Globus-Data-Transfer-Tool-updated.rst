Using the Globus Data Transfer Tool
===================================

This document provides an overview of how Globus works and how to use
Globus to transfer files.

Overview
--------

Transferring and sharing files is a major bottleneck in data workflows.
The Globus service allows users to automate file transfer between data
storage and compute systems as well as share data with any other Globus
user quickly and securely. The Globus web interface allows users to
easily initiate and schedule file transfers in a point and click
interface.

Once a file transfer begins, Globus handles the rest:

-  Globus maximizes use of your network bandwidth to prevent
   connectivity problems.
-  If you lose network connection, Globus will continue where you left
   off.
-  When your file transfer is complete, Globus checks for data integrity
   and will notify you of any problems.
-  If your file transfer fails, Globus will restart the transfer
   automatically.

This document describes how to use Globus to transfer and share data
using Northwestern University’s subscription.

Restrictions
------------

Globus should not be used to transfer legally and contractually
restricted data, such as data subject to HIPAA and FISMA.

Endpoints and Collections
-------------------------

Globus refers to the places that data are stored (such as your computer,
Quest, RDSS, the cloud)
as\ `endpoints <https://docs.globus.org/faq/globus-connect-endpoints/#what_is_an_endpoint>`__.
Data stored on these endpoints are organized in collections.

See `Globus Endpoints and
Collections <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1970>`__
for more information on the endpoints and collections you can use with
Globus.

Transferring data
-----------------

Once your endpoints are configured, you can use Globus to initiate file
transfers between these endpoints.

See `Transferring files using
Globus <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=2014>`__
for instructions on how to transfer files using Globus.

Advanced features
-----------------

You can do much more with Globus than transfer data. See the following
sections for more information on Sharing data and automating data
workfows using Globus timer and Globus’s command line interface.

See `Advanced Globus
Features <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=2015>`__
for more information.

Resources
---------

Need help using Globus? The following resources are available to you:

At Northwestern
~~~~~~~~~~~~~~~

Contact Northwestern data management specialists by emailing
quest-help@northwestern.edu

Also see our `Research Data Management
Guide <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=2016>`__
for quick links to our help documentation.

From Globus
~~~~~~~~~~~

-  Globus documentation main page: https://docs.globus.org/
-  Globus How Tos: https://docs.globus.org/how-to/
-  Globus FAQ: https://docs.globus.org/faq/
