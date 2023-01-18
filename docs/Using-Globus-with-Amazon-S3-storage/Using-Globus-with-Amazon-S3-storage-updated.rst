Using Globus with Amazon S3 storage
===================================

The Globus data transfer service can be used to transfer data to and
from Amazon’s S3 cloud storage service. This document will describe how
to set up Globus to work with Amazon S3.

For more information on why and how to use Amazon S3 storage, see `Using
Amazon S3
Storage <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=2024>`__

Setup
-----

To use Globus with Amazon S3, you need to: create a bucket (if you don’t
have one already), create an IAM access key with permission to write to
the bucket, and install the access key on the endpoint.

#. `Create an Amazon S3
   bucket <https://kb.northwestern.edu/create-s3-bucket>`__
#. `Create an AWS access
   key <https://kb.northwestern.edu/generate-aws-iam-access-key>`__ that
   has permission to read from and write to the bucket
#. Give Globus access to the bucket via the IAM access key you created
   (see below).

Authenticate in the Globus interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once you have an AWS Access Key ID, you can use these credentials to
authenticate in the Globus interface.

#. First, log into `Globus File
   Manager <https://app.globus.org/file-manager>`__ using your
   Northwestern NetID

#. Then search for the Northwestern AWS endpoints.

   Northwestern has 3 endpoints to transfer data to Amazon Web Service
   S3 storage.

   -  **Northwestern AWS us-east-1 N. Virginia**
   -  **Northwestern AWS us-east-2 Ohio**
   -  **Northwestern AWS us-west-2 Oregon**

   These endpoints are now region-agnostic and can be used to transfer
   data stored in any AWS region.

#. Once you have selected an endpoint, follow the instructions in
   Globus’s `How to Access Your Files on AWS S3 with
   Globus <https://docs.globus.org/how-to/access-aws-s3/>`__
   documentation.

Email quest-help@northwestern.edu for additional support in setting up
Globus with Amazon S3.
