.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##########################################################################
Managing |S3| Bucket Access Permissions with the AWS SDK for PHP Version 3
##########################################################################

.. meta::
   :description: Get access control lists (ACLs) and set permissions for Amazon S3 buckets using the AWS SDK for PHP version 3.
   :keywords: Amazon S3 code examples for PHP

Access control lists (ACLs) are one of the resource-based access policy options you can use to manage access to your buckets and objects.
You can use ACLs to grant basic read/write permissions to other AWS accounts. To learn more, see `Managing Access with ACLs <http://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html>`_.


The following example shows how to:

* Get the access control policy for a bucket using :aws-php-class:`GetBucketAcl <api-s3-2006-03-01.html#getbucketacl>`.
* Set the permissions on a bucket using ACLs, using :aws-php-class:`PutBucketAcl <api-s3-2006-03-01.html#putbucketacl>`.

.. include:: text/git-php-examples.txt


Get and Set an Access Control List Policy
=========================================

**Imports**

.. literalinclude::  example_code/s3/s3BucketAcl.php
   :lines: 20-23
   :language: PHP

**Sample Code**

.. literalinclude:: example_code/s3/s3BucketAcl.php
   :lines: 25-
   :language: php
