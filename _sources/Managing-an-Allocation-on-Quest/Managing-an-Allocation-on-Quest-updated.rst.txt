Managing an Allocation on Quest
===============================

How to check allocation resources, expiration dates, and project
members.

Allocation Resource Management
------------------------------

Add People to an Allocation
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use the `Join an Existing
Allocation <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__
form. The Allocation Manager for the allocation will be notified.
Individuals added to an allocation will have access to the
``/projects/<allocationID>`` directory, but they will have their own
home directory.

Renew an Existing Allocation or Create a New Allocation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use the appropriate `Research Allocation Original or Renewal
Request <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__
form for your allocation type (I or II).

List Allocation Members
-----------------------

Display all current members of the allocation:

.. code:: code

   module load utilities
   grouplist <allocationID>

Check Allocation Resources
--------------------------

You can find out your allocations by using ``groups`` command on Quest.
The output of this command will include your NetID and the IDs of
allocations that you belong.

To check the storage use of ``/projects/<allocationID>`` and allocation
expiration:

.. code:: code

   checkproject <allocationID>

Example Output
^^^^^^^^^^^^^^

.. code:: code

   ====================================

   Reporting for project p123XX
   ------------------------------------
   156 GB in 1380 files (31.21% of 1000 GB quota)
   Allocation Type: Allocation I 
   Expiration Date: 2022-12-01
   ------------------------------------

   ====================================
