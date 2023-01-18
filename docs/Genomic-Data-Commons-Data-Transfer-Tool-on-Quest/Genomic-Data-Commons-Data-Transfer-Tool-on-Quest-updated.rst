Genomic Data Commons Data Transfer Tool on Quest
================================================

Downloading data from the Genomic Data Commons onto Quest via gdc-client

Genomic Data Commons
--------------------

The National Cancer Institute’s `Genomic Data
Commons <https://gdc.cancer.gov/>`__\ (GDC) provides the cancer research
community with a unified data repository that enables data sharing
across cancer genomic studies in support of precision medicine.

To download genomics data from the Genomic Data Commons onto Quest, PIs
must have an `electronic Research Administration (eRA) commons
account <https://era.nih.gov/commons/faq_commons.cfm>`__ from the
National Institutes of Health that has been given `DBGap
access <https://www.ncbi.nlm.nih.gov/books/NBK99225/>`__ for the Genomic
Data Commons. Once this access has been granted by the NIH, PIs can then
disseminate Genomic Data Commons access to other individuals in their
lab. De-identified data can then be downloaded from the Genomics Data
Portal onto Quest into the PI’s allocation for computational research
purposes, using gdc-client.

Using gdc-client
~~~~~~~~~~~~~~~~

At the command line on Quest in the directory that you’d like to
transfer the Genomic Data Commons data into, type:

.. container:: code

   module load gdc-client

Help is available by typing:

.. container:: code

   gdc-client –help

Additional instructions for using gdc-client can be found on the
National Cancer Institute’s webinar on using the `GDC Data Transfer
Tool <https://gdc.cancer.gov/node/757>`__.
