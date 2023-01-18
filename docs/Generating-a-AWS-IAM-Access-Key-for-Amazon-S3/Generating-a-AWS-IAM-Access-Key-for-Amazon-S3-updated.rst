Generating a AWS IAM Access Key for Amazon S3
=============================================

This document describes how to generate an AWS IAM access key for
read/write access to S3.

For general information on AWS S3, please see the following
document:`Amazon S3
Storage. <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=2024>`__

Amazon Web Services (AWS) provides an `Identity and Access
Management <https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html>`__
`(IAM) <https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html>`__
service that allows you to securely control access to AWS resources. One
way to provide access to a resource is by generating an IAM access key.
The instructions below walk you through this process using the AWS
Management Console.

Using the AWS Management Console
--------------------------------

To log in to the AWS Management console,

#. go
   to\ `https://aws.northwestern.edu <https://aws.northwestern.edu/>`__\ and
   log in with your Northwestern NetID.
#. Then, select an appropriate roleand click the blue Sign In button.

To access the IAM service:

#. Click the magnifying glass in the upper left hand corner of the
   screen.
#. Type “IAM” into the search box
#. Select IAM (Manage access to AWS resources)

.. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-iam-search.png
   :alt: Searching IAM in text box in AWS management console.
   :width: 500px
   :height: 248px

You will see a view of the IAM dashboard that summarizes how many
groups, users, roles, policies and identity providers you have access to
on the right. On the left, you will see a sidebar with different aspects
of IAM. We will be using the Users option in the next section.

.. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-iam-dashboard.png
   :alt: View of the AWS IAM dashboard
   :width: 700px
   :height: 248px

Creating an IAM user
--------------------

| To generate an access key you must first create an IAM user. To create
  an IAM user:
| Click “Users” in the left navigation and then click the blue“Add
  users” button on the right.
| |View of the AWS IAM user page with the add user button highlighted|

User Details
~~~~~~~~~~~~

You will see the “Add user” screen that asks you to provide user details
and select AWS access type.

-  **User names** must be unique within the account. The names are not
   case sensitive and must be 64 or fewer characters. They can contain
   letters, numbers, plus (+), equal (=), comma (,), period (.), at sign
   (@), underscore (_), and hyphen (-).
-  Select **Access key - Programmatic access** as the AWS credential
   type. This option allows direct access to the resource but not to log
   in to the AWS management console.

Then click the blue “Next: Permissions” button on the lower right hand
side of the screen.

Permissions
~~~~~~~~~~~

Next, you must specify what types of access the user will have to
various resources. Do one of the following options depending on your
preference:

Access to all S3 Buckets
^^^^^^^^^^^^^^^^^^^^^^^^

If you want to provide full access to all S3 buckets in your account,

#. Select the **Attach Existing Policies Directly** tab.
#. Then search for \`AmazonS3FullAccess\` in the filter policies search
   box.
#. Then click the checkbox to the left of the policy name

.. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-iam-s3-full-access.png
   :alt: View of applying full access to S3 for an AWS IAM user
   :width: 700px
   :height: 280px

Access to a specific S3 bucket
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To allow access to a specific S3 bucket only, you must create a user
policy. To create a user policy:

#. Select the **Attach Existing Policies Directly** tab.
#. Click the gray Create Policy Button on the left.

.. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-iam-user-create-policy.png
   :alt: View of creating a new user access policy AWS IAM
   :width: 700px
   :height: 215px

Visual Editor
'''''''''''''

A new window will open containing the “Create Policy” interface. You can
create a policy using the Visual Editor. You must specify the following
information:

-  **Service**: S3
-  **Actions**: Choose these based on what you want the IAM user to be
   able to do. The options here are incredible granular. Each Checkbox
   has a sub menu with more options. These options have info buttons
   that explain exactly what these options do.
-  **Resources**: After you select Actions, you can select what
   resources to apply these actions to. The Visual Editor will prompt
   you to add resources based on Actions you chose.

Once you have selected the above information, click the “Next: Tags”
button.\ **Tags** are optional metadata that describes what this policy
does. Each tag has a “Key” (eg: Service) and a “Value” (eg: S3) that you
can specify. When you have entered appropriate tags, click “Next:
Review”.

The review page will ask for the following information about your
policy:

-  Name: Use alphanumeric and ‘+=,.@-\_’ characters. Maximum 128
   characters.
-  Description: Maximum 1000 characters. Use alphanumeric and ‘+=,.@-\_’
   characters.

It also displays a summary of the permissions granted and tags applied
to the policy. When you are done, Click “Create Policy” in the lower
right.

JSON Template
'''''''''''''

Some standard processes have JSON templates that give appropriate
permissions for a specific task. For example, the following template
grants read/write permissions for a specific S3 bucket.

.. container:: panel panel-default

   .. container:: panel-heading

      JSON Template

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      To use this template, click to the JSON tab of the Create Policy
      screen and paste it in, then change “YOUR-BUCKET-NAME-HERE” in the
      Resource lines to match your bucket name:

      .. code:: code

         {
             "Version": "2012-10-17",
             "Statement": [
                 {
                     "Sid": "AllBuckets",
                     "Effect": "Allow",
                     "Action": [
                         "s3:ListAllMyBuckets",
                         "s3:GetBucketLocation"
                     ],
                     "Resource": "*"
                 },
                 {
                     "Sid": "Bucket",
                     "Effect": "Allow",
                     "Action": [
                         "s3:ListBucket",
                         "s3:ListBucketMultipartUploads"
                     ],
                     "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME-HERE"
                 },
                 {
                     "Sid": "Objects",
                     "Effect": "Allow",
                     "Action": [
                         "s3:GetObject",
                         "s3:PutObject",
                         "s3:DeleteObject",
                         "s3:ListMultipartUploadParts",
                         "s3:AbortMultipartUpload"
                     ],
                     "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME-HERE/*"
                 }
             ]
         }

Applying the IAM policy to your user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once your policy is created, return to the “Add User” Screen.

#. Refresh the policies window so it registers the new policy you
   created.
#. Search for the policy name.
#. Check the box to the left of the policy to apply it to your IAM user.

.. image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-iam-refresh-policies.png
   :alt: View of adding an existing policy to an AWS IAM user
   :width: 700px
   :height: 260px

.. code:: code

.. code:: code

Then you can enter tags as you did when creating the IAM policy. Then
click “Next: Review”

The review page gives you an opportunity to review the permissions you
have given to this user. When you are satisfied, click “Create User”.

Downloading credentials
~~~~~~~~~~~~~~~~~~~~~~~

You will be prompted to download a CSV file containing the user’s
credentials (access key ID and secret access key). Either download the
file or copy and paste the credentials from this page. The credentials
will only be displayed one time so make sure to save them from this
page.

We recommend storing this information in a password manager or `AWS
Secrets Manager <https://aws.amazon.com/secrets-manager/>`__.

.. |View of the AWS IAM user page with the add user button highlighted| image:: https://kb.northwestern.edu/images/group293/shared/researchdatastorage/aws-iam-add-user.png
   :width: 700px
   :height: 143px
