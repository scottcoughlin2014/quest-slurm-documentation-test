Creating an Amazon S3 bucket
============================

How to create an S3 bucket using the S3 management console.

.. container::
   :name: doc-summary

   For general information on AWS S3, please see the following document:
   `Using Amazon S3
   Storage <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=2024>`__

   .. rubric:: Setup
      :name: setup

   Contact consultant@northwestern.edu to request access to the NU AWS
   organization, which will allow you to log in with your Northwestern
   credentials.

   .. rubric:: Using the AWS Management Console
      :name: using-the-aws-management-console

   To log in to the AWS Management console,

   #. go to https://aws.northwestern.edu and log in with your
      Northwestern NetID.
   #. Then, select an appropriate roleand click the blue Sign In button.

   To Access Amazon S3:

   #. Click the magnifying glass in the upper left hand corner of the
      screen.
   #. Type “S3” into the search box
   #. Select S3 (Scalable Storage in the cloud)

   .. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-search-s3.png
      :alt: visual of where the AWS console search is. Indicates how to
      search for S3
      :width: 480px
      :height: 225px

   By default, you will see a navigation pane on the left, with the
   “Buckets” option selected. On the right, you’ll see an “Account
   snapshot” that shows how much storage is being used among all S3
   buckets you have access to. Below that, you’ll see the Bucket
   sections, which lists the buckets you have access to, if any, with
   additional information about each bucket.

   .. rubric:: Creating an Amazon S3 bucket
      :name: creating-an-amazon-s3-bucket-1

   | Click the orange “Create bucket” button in the Buckets section on
     the lower right of the screen.
   | |Amazon S3 console with create bucket highlighted|
   | On the Create Bucket screen you will need to give your bucket a
     name and choose a region.

   -  **Bucket name**: The name must be globally unique and contain only
      alphanumeric characters and dashes
   -  **Region**: If you use other AWS resources, pick the same region
      as you use for other work. Otherwise, pick the region that is the
      most geographically near to where you do your work.

   Most users should keep the rest of the options on this screen as
   defaults. By default, S3 buckets have the following properties:

   #. **Object Ownership**: All objects stored in this bucket are owned
      by the account/role you logged in with, even if it is written to
      this bucket by another account.
   #. **Public access**: Public access to buckets and objects are
      blocked. Access is granted on a case by case basis to those who
      need it. This setting is recommended for content that you don’t
      want freely available on the web.
   #. **Bucket versioning**: This feature is disabled to save space.
      Select Enable if you have a need to keep previous versions of
      files that are being changed in this bucket. Keep in mind you will
      pay storage costs for each version.
   #. **Encryption**: Encrypting objects stored in the bucket at is
      disabled.
   #. **Object lock** This feature is disabled. Enabling this feature
      will prevent objects from being deleted or overwritten.

   If you need help deciding if you need to modify these defaults, email
   `quest-help@northwestern.edu. <mailto:quest-help@northwestern.edu>`__

   When you have entered the required information, click the orange
   “Create Bucket” button at the bottom right of the screen.

   You should see a green banner at the top of the screen that says
   “Successfully created bucket ‘<bucketname>’”. The webpage will return
   to the Buckets page.

   To grant read/write permissions to this bucket, see `Generating an
   AWS IAM Key for Amazon
   S3 <https://kb.northwestern.edu/generate-aws-iam-access-key>`__.

.. |Amazon S3 console with create bucket highlighted| image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-s3-create-bucket.png
   :width: 600px
   :height: 277px
