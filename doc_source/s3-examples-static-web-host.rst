.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


###################################################################
Using an |S3| Bucket as a Static Web Host with |sdk-php| Version 3
###################################################################

.. meta::
   :description: Get, set, and remove website configuration for an Amazon S3 bucket with the AWS SDK for PHP version 3.
   :keywords: Amazon S3 code examples

You can host a static website on |S3|. To learn more, see :S3-dg:`Hosting a Static Website on S3 <WebsiteHosting>`.

The following example shows how to:

* Get the website configuration for a bucket using :aws-php-class:`GetBucketWebsite <api-s3-2006-03-01.html#getbucketwebsite>`.
* Set the website configuration for a bucket using :aws-php-class:`PutBucketWebsite <api-s3-2006-03-01.html#putbucketwebsite>`.
* Remove the website configuration from a bucket using :aws-php-class:`DeleteBucketWebsite <api-s3-2006-03-01.html#deletebucketwebsite>`.

All the example code for the |sdk-php| Version 3 is available `here on GitHub <https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/php/example_code>`_.

Credentials
===========

Before running the example code, configure your AWS credentials. See :doc:`guide_credentials`.


Get, Set, and Delete the Website Configuration for a Bucket
===========================================================

**Imports**

.. literalinclude::  example_code/s3/s3WebHost.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/s3/s3WebHost.php
   :lines: 25-
   :language: php
