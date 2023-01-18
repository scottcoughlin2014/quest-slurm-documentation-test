Using Amazon S3 Storage
=======================

When and how to use Amazon Web Service’s (AWS) S3 storage to store data
in the cloud.

.. container::
   :name: doc-summary

   ``Amazon S3 is a cloud-based "object storage" service, allowing files to be securely stored and accessed from anywhere. It can scale to handle any amount of data and pricing is based solely on usage, with multiple tiers of service available to allow for flexibility and cost optimization. More information about Amazon S3 can be found at https://aws.amazon.com/s3/.``\ ````

   .. rubric:: When to Use Amazon S3
      :name: when-to-use-amazon-s3

   -  Backups and data archiving. S3 is a relatively low cost solution
      for data storage ($0.023/GB/month) and can be used to
      automatically transfer data to the even lower cost AWS Glacier
      service for long term archiving of data that will not be
      frequently accessed.
   -  Data analysis using Amazon Web Services. If you will be using
      Amazon’s other services (AWS EC2, AWS Batch, AWS Elastic
      MapReduce, etc.) to analyze your data, storing it in S3 will
      provide significantly faster data access than transferring from
      other services.

   The Northwestern Globus S3 connector service can access S3 buckets in
   the us-east-1 (Northern Virginia), us-east-2 (Ohio), and us-west-2
   (Oregon) regions.

   .. rubric:: Amazon S3 Pricing and Cost Considerations
      :name: amazon-s3-pricing-and-cost-considerations

   | Amazon S3 pricing can be found here:
     https://aws.amazon.com/s3/pricing/. For the regions supported by
     this service, the standard storage tier is $0.023/GB/Month. There
     is a charge for requests (that is, API commands interacting with
     the S3 service) as well but it is usually only a few cents per
     month unless a very large amount of requests is issued.
   | Data transfer \*in\* to Amazon S3 is always free. However, data
     transfer out to the internet is $0.09/GB and data transfer from one
     S3 region to another costs $0.01/GB or $0.02/GB, depending on the
     region. Therefore it is important to consider which region you
     create your buckets in to minimize cost and latency of data
     transfer.
   | Northwestern does have a consolidated billing account in place with
     Amazon Web Services, and accounts created within this billing
     structure have their data transfer out cost waived (provided
     certain conditions are met). More information about accounts can be
     found here: http://www.cloud.northwestern.edu/aws/.

   See the following documents to learn how to:

   -  `Create an Amazon S3
      bucket <https://kb.northwestern.edu/using-globus-with-s3>`__
   -  `Create IAM
      credentials <https://kb.northwestern.edu/generate-aws-iam-access-key>`__
   -  `Use Globus to transfer data to/from
      S3 <https://kb.northwestern.edu/using-globus-with-s3>`__
