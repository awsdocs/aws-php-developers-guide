.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

####################################################################
Working with |S3| Bucket Policies with the AWS SDK for PHP Version 3 
####################################################################

.. meta::
   :description: Return, replace, or delete Amazon S3 bucket policies using the AWS SDK for PHP version 3.
   :keywords: Amazon S3 code examples for PHP

You can use a bucket policy to grant permission to your |S3| resources. To learn more, see `Using Bucket Policies and User Policies <http://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html>`_.

The following example shows how to:

* Return the policy for a specified bucket using :aws-php-class:`GetBucketPolicy <api-s3-2006-03-01.html#getbucketpolicy>`.
* Replace a policy on a bucket using :aws-php-class:`PutBucketPolicy <api-s3-2006-03-01.html#putbucketpolicy>`.
* Delete a policy from a bucket using :aws-php-class:`DeleteBucketPolicy <api-s3-2006-03-01.html#deletebucketpolicy>`.

.. include:: text/git-php-examples.txt



Get, Delete, and Replace a Policy on a Bucket
=============================================

**Imports**

.. literalinclude::  example_code/s3/s3BucketPolicy.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/s3/s3BucketPolicy.php
   :lines: 25-
   :language: php
