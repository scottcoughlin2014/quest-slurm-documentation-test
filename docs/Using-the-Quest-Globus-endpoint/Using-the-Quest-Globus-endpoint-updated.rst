Using the Quest Globus endpoint
===============================

This document describes how to configure and use the Quest Globus
endpoint

.. container::
   :name: doc-summary

   The Northwestern Quest endpoint allows users to transfer data to and
   from Northwestern’s `Quest High Performance computing
   cluster <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/index.html>`__.

   .. rubric:: Setup
      :name: setup

   To transfer data to or from Quest, you must have a Quest account.
   Follow the linked instructions to\ `request a Quest
   allocation <https://it.northwestern.edu/departments/it-services-support/research/computing/quest/general-access-allocation-types.html>`__.

   .. rubric:: Activating the Quest Endpoint
      :name: activating-the-quest-endpoint

   Once you have access to Quest, `log into Globus using your
   Northwestern
   credentials <https://docs.globus.org/how-to/get-started/#log_in_with_an_existing_identity>`__.
   Then search for the **Northwestern Quest** collection. Note that
   there are similarly named endpoints (like Northwestern Quest RDSS) so
   make sure the name matches.

   .. image:: https://kb.northwestern.edu/images/group293/71271/select-quest-endpoint.png
      :alt: Image of the search results of "Northwestern Quest" in the
      globus interface with a red box around the Northwesrern Quest
      Endpoint
      :width: 500px
      :height: 207px

   When you select the Northwestern Quest Collection, Globus will prompt
   you to authenticate. Click Continue.

   .. image:: https://kb.northwestern.edu/images/group293/71271/Authenticate-quest.png
      :alt: Authentication screen for Northwestern Quest endpoint
      :width: 500px
      :height: 378px

   Then, Globus will ask for consent to use your login credentials and
   ask you to pick your identity provider. Select Northwestern
   University.

   .. image:: https://kb.northwestern.edu/images/group293/71271/quest-id-provider.png
      :alt: Screen to select Northwestern as the identity provider
      :width: 500px
      :height: 421px

   After selecting Northwestern, you may see the Northwestern NetID
   login screen. Use your NetID and password to log in. If you have
   already logged in with your NetID in your browser recently, you might
   skip this step.

   Finally you should see your home directory on Quest and can initiate
   file transfers from this location.

   If you would like to access your projects directory, hit the up arrow
   to navigate to the parent directory

   .. image:: https://kb.northwestern.edu/images/group293/71271/quest-nav-up.png
      :alt: Globus interface navigate to parent directory
      :width: 500px
      :height: 452px

   Then select projects and find the name of your project directory.

   .. image:: https://kb.northwestern.edu/images/group293/71271/Quest-projects.png
      :alt: Show projects directory
      :width: 500px
      :height: 513px

   .. rubric:: Extending Quest Endpoint activation
      :name: extending-quest-endpoint-activation

   By default, this activation lasts for 10 days. After the 10 day
   period, Globus will ask you to reauthenticate when you try to access
   the endpoint. You can also extend the activation period by 10 days
   prior to the activation expiring.

   To do this,

   -  Find the Northwestern Quest Endpoint in the File Manager window.
   -  Select the Manage Activation icon that looks like a power button
      at the bottom of the column between the file browser windows (red
      box)
   -  Click Extend Activation in the Endpoint Overview view for the
      managed endpoint.

   .. image:: https://kb.northwestern.edu/images/group293/71271/extend-quest-endpoint.png
      :alt: Window to extend quest activation. red box around button to
      click to open window.
      :width: 500px
      :height: 621px

   -  Globus will notify you that you must reauthenticate with your
      Northwestern NetID. Hit continue
   -  Globus will then ask you to select an identity provider. Select
      Northwestern University then click “Log On”
   -  You should see a message that says “Access certificate extended
      successfully” and tells you when the endpoint activation will
      expire. Click ok.

   If you would like to create a stable endpoint that does not expire,
   you can create a Shared Collection on the Quest Endpoint. See
   Globus’s `How to Share Data Using Globus
   documentation <https://docs.globus.org/how-to/share-files/#:~:text=Log%20into%20Globus%20and%20navigate,in%20the%20right%20command%20pane.&text=Sharing%20is%20available%20for%20folders.>`__
   for more information.
