Managing access to Research Data Storage Service (RDSS) Shares
==============================================================

This document describes how to manage access to Research Data Storage
Service (RDSS) shares.

After the initial share setup, the RDSS allows share owners/managers to
control who has access to the files and folders stored within their
share using the Self-Service Group Management Tool.

**Note**: FSMResFiles share owners do not have access to this tool and
should contact FSM-IT (fsmhelp@northwestern.edu) for help managing user
access to their shares.

Access to each share is controlled through two Active Directory (AD)
groups:

-  NU SMB <sharename> Read-Only gives users **read only access**
-  NU SMB <sharename> Read-Write gives users **read-write access**

Adding/removing users from these groups will grant/remove permissions to
the RDSS share.

To access these AD groups:

#. Log in to the `Self-Service Group Management
   Tool <https://adsselfservice.it.northwestern.edu/>`__ with your
   Northwestern NetID and password
#. Click on **My AD groups** in the left hand panel.
#. You should see a read only and a read-write group for each share that
   you own/manage

See `Managing group permissions using the Self-Service Group Management
tool <page.php?id=86126>`__ for instructions on how to add or remove
users from these lists.
