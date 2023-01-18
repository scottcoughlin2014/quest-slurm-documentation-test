Globus Endpoints and Collections
================================

This document describes the different types of endpoints and collections
you can use with the Globus data transfer tool.

.. container::
   :name: doc-summary

   Globus refers to the places that data are stored (such as your
   computer, Quest, RDSS, the cloud) as
   `endpoints <https://docs.globus.org/faq/globus-connect-endpoints/#what_is_an_endpoint>`__.
   Data stored on these endpoints are organized in collections. The
   terms endpoint and collection are used somewhat interchangeably,
   however one endpoint can host many different collections. You will
   need to setup and configure each endpoint that you want to use with
   Globus.

   .. rubric:: Personal Endpoints
      :name: personal-endpoints

   Personal endpoints allow you to transfer data to and from your laptop
   or workstation. To use a personal endpoint, install and configure
   `Globus Connect
   Personal <https://www.globus.org/globus-connect-personal>`__ on your
   computer.

   -  `Mac
      Instructions <https://docs.globus.org/how-to/globus-connect-personal-mac/>`__
   -  `Linux
      Instructions <https://docs.globus.org/how-to/globus-connect-personal-linux/>`__
   -  `Windows
      Instructions <https://docs.globus.org/how-to/globus-connect-personal-windows/>`__

   **Note**: By default, Globus will allow transfer access to the home
   directory on your computer. See the Configuration section
   (`Mac <https://docs.globus.org/how-to/globus-connect-personal-mac/#configuration>`__,
   `Linux <https://docs.globus.org/how-to/globus-connect-personal-linux/#config-paths>`__,
   `Windows <https://docs.globus.org/how-to/globus-connect-personal-windows/#configuration>`__)
   of the Globus Personal endpoint documentation for more information on
   how to adjust the folders that Globus can see on your computer.

   .. rubric:: Northwestern Managed Endpoints
      :name: northwestern-managed-endpoints

   Northwestern University provides Globus endpoints on Quest, RDSS,
   OneDrive and AWS S3. You must activate these endpoints before you can
   use them.

   Select an endpoint for more information on how to use it.

   -  `Using the Quest Globus
      endpoint <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1962>`__
   -  `Using the RDSS Globus
      Endpoint <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1967>`__
   -  `Using Globus with Amazon S3
      storage <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1968>`__
   -  `Using the OneDrive/SharePoint Globus
      Endpoint <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1969>`__

   .. rubric:: Shared Collections
      :name: shared-collections

   Your collaborators, especially those outside of Northwestern
   University, may not have access to all of the locations that you
   store your data, including data on Northwestern managed endpoints.
   Globus allows you to grant read and/or write access to collections to
   any Globus user, even if they do not have a Northwestern NetID.
   Additionally, you can create shared collections on managed endpoints
   to avoid having to reauthenticate after the activation expires. This
   feature is especially useful in automated workflows.

   See Globus’s `How to Share Data Using Globus
   documentation <https://docs.globus.org/how-to/share-files/#:~:text=Log%20into%20Globus%20and%20navigate,in%20the%20right%20command%20pane.&text=Sharing%20is%20available%20for%20folders.>`__
   for more information.

   .. rubric:: 
      :name: section
